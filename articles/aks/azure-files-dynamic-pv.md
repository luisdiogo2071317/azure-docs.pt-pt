---
title: Utilize ficheiros do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 21245688076cf0a21164b549eb68bc6f55d6ec6c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="persistent-volumes-with-azure-files"></a>Volumes persistentes com ficheiros do Azure

Um volume persistente representa um conjunto de armazenamento que tenha sido aprovisionado para utilização num Kubernetes cluster. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Este documento fornece detalhes sobre o aprovisionamento dinâmico de uma partilha de ficheiros do Azure como um volume persistente Kubernetes num AKS cluster.

Para obter mais informações sobre volumes persistentes Kubernetes, consulte [volumes persistentes Kubernetes][kubernetes-volumes].

## <a name="create-storage-account"></a>Criar conta de armazenamento

Quando aprovisionar dinamicamente uma partilha de ficheiros do Azure como um volume Kubernetes, pode ser utilizada qualquer conta de armazenamento, desde que está contido no mesmo grupo de recursos do cluster AKS. Se necessário, crie uma conta do storage no mesmo grupo de recursos do cluster AKS.

Para identificar o grupo de recursos correto, utilize o [lista de grupos de az] [ az-group-list] comando.

```azurecli-interactive
az group list --output table
```

Está à procura de um grupo de recursos com um nome semelhante ao `MC_clustername_clustername_locaton`, em que clustername é o nome do AKS cluster e localização é a região do Azure onde o cluster ter sido implementado.

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Utilize o [criar conta de armazenamento az] [ az-storage-account-create] comando para criar a conta de armazenamento.

Utilizar este exemplo, atualizar `--resource-group` com o nome do grupo de recursos, e `--name` para um nome à sua escolha.

```azurecli-interactive
az storage account create --resource-group MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Criar a classe de armazenamento

Uma classe de armazenamento é utilizada para definir como é criada uma partilha de ficheiros do Azure. Pode ser especificada uma conta de armazenamento específico na classe. Se não for especificada uma conta de armazenamento, um `skuName` e `location` tem de ser especificado, e todas as contas de armazenamento no grupo de recursos associados são avaliadas para uma correspondência.

Para obter mais informações sobre Kubernetes classes de armazenamento para ficheiros do Azure, consulte [Kubernetes armazenamento Classes][kubernetes-storage-classes].

Crie um ficheiro denominado `azure-file-sc.yaml` e copie o manifesto seguinte. Atualização do `storageAccount` com o nome da sua conta de armazenamento de destino.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: mystorageaccount
```

Criar a classe de armazenamento com o [kubectl aplicar] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Criar afirmações volume persistente

Uma afirmação de volume persistente (PVC) utiliza o objeto de classe de armazenamento para aprovisionar dinamicamente uma partilha de ficheiros do Azure.

O manifesto seguinte pode ser utilizado para criar uma afirmação de volume persistente `5GB` tamanho com `ReadWriteOnce` acesso.

Crie um ficheiro denominado `azure-file-pvc.yaml` e copie o manifesto seguinte. Certifique-se de que o `storageClassName` corresponde à classe de armazenamento criada no último passo.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Criar a afirmação de volume persistente com o [kubectl aplicar] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Depois de concluído, será criada a partilha de ficheiros. Um segredo Kubernetes também é criado que contém informações de ligação e as credenciais.

## <a name="using-the-persistent-volume"></a>Utilizar o volume persistente

O manifesto seguinte cria um pod que utiliza a afirmação de volume persistente `azurefile` para montar a partilha de ficheiros do Azure, o `/mnt/azure` caminho.

Crie um ficheiro denominado `azure-pvc-files.yaml`e copie o manifesto seguinte. Certifique-se de que o `claimName` corresponde PVC criado no último passo.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

Criar pod com o [kubectl aplicar] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Tem agora um pod em execução com o Azure disco montado no `/mnt/azure` diretório. Esta configuração pode ser vista quando inspecionar o pod através de `kubectl describe pod mypod`.

## <a name="mount-options"></a>Opções de montagem

Valores predefinidos para fileMode e dirMode diferem entre versões Kubernetes conforme descrito na seguinte tabela.

| versão | valor |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 ou superior | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 ou superior | 0755 |

Se utilizar um cluster de versão 1.8.5 ou superior, montagem opções podem ser especificadas num objeto de classe de armazenamento. Os seguintes conjuntos de exemplo `0777`.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Se utilizar um cluster versão 1.8.0 - 1.8.4, de um contexto de segurança pode ser especificado com o `runAsUser` valor definido como `0`. Para obter mais informações sobre o contexto de segurança Pod, consulte [configurar um contexto de segurança][kubernetes-security-context].

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes utilizando ficheiros do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para ficheiros do Azure][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
