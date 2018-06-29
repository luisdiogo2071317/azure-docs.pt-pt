---
title: Utilize ficheiros do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 84500791887194884e1ec7d15ddfbc169ba22517
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098350"
---
# <a name="persistent-volumes-with-azure-files"></a>Volumes persistentes com ficheiros do Azure

Um volume persistente é um conjunto de armazenamento que foi criado para utilização num Kubernetes cluster. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente criado. Detalhes deste documento **criação dinâmica** de uma partilha de ficheiros do Azure como um volume persistente.

Para obter mais informações sobre Kubernetes persistentes volumes, incluindo a criação estática, consulte [volumes persistentes Kubernetes][kubernetes-volumes].

## <a name="create-storage-account"></a>Criar conta de armazenamento

Ao criar dinamicamente uma partilha de ficheiros do Azure como um volume Kubernetes, pode ser utilizada qualquer conta de armazenamento, desde que está a ser o AKS **nó** grupo de recursos. Esta é a tarefa com o `MC_` prefixo que foi criado com o aprovisionamento de recursos para o cluster AKS. Obter o nome do grupo de recursos com o [mostrar de recurso az] [ az-resource-show] comando.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Utilize o [criar conta de armazenamento az] [ az-storage-account-create] comando para criar a conta de armazenamento.

Atualização `--resource-group` com o nome do grupo de recursos recolhidos no último passo, e `--name` para um nome à sua escolha.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Ficheiros do Azure atualmente só funcionam com o armazenamento standard. Se utilizar o armazenamento premium, o volume irá falhar para aprovisionar.

## <a name="create-storage-class"></a>Criar a classe de armazenamento

Uma classe de armazenamento é utilizada para definir como é criada uma partilha de ficheiros do Azure. Pode ser especificada uma conta de armazenamento específico na classe. Se não for especificada uma conta de armazenamento, um `skuName` e `location` tem de ser especificado, e todas as contas de armazenamento no grupo de recursos associados são avaliadas para uma correspondência.

Para obter mais informações sobre Kubernetes classes de armazenamento para ficheiros do Azure, consulte [Kubernetes armazenamento Classes][kubernetes-storage-classes].

Crie um ficheiro denominado `azure-file-sc.yaml` e copie o manifesto seguinte. Atualização do `storageAccount` com o nome da sua conta de armazenamento de destino. Consulte a secção [montagem opções] para obter mais informações sobre `mountOptions`.

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

Criar a classe de armazenamento com o [kubectl aplicar] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Criar afirmações volume persistente

Uma afirmação de volume persistente (PVC) utiliza o objeto de classe de armazenamento para aprovisionar dinamicamente uma partilha de ficheiros do Azure.

O YAML seguinte pode ser utilizado para criar uma afirmação de volume persistente `5GB` tamanho com `ReadWriteMany` acesso. Para obter mais informações sobre modos de acesso, consulte o [volume persistente Kubernetes] [ access-modes] documentação.

Crie um ficheiro denominado `azure-file-pvc.yaml` e copie o seguinte YAML. Certifique-se de que o `storageClassName` corresponde à classe de armazenamento criada no último passo.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

Criar a afirmação de volume persistente com o [kubectl aplicar] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Depois de concluído, será criada a partilha de ficheiros. Um segredo Kubernetes também é criado que inclui as informações da ligação e as credenciais.

## <a name="using-the-persistent-volume"></a>Utilizar o volume persistente

O YAML seguinte cria um pod que utiliza a afirmação de volume persistente `azurefile` para montar a partilha de ficheiros do Azure, o `/mnt/azure` caminho.

Crie um ficheiro denominado `azure-pvc-files.yaml`e copie o seguinte YAML. Certifique-se de que o `claimName` corresponde PVC criado no último passo.

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

Se utilizar um cluster de versão 1.8.5 ou superior e criar dinamicamente o volume de persistant com uma classe de armazenamento, as opções de montagem podem ser especificadas num objeto de classe de armazenamento. Os seguintes conjuntos de exemplo `0777`.

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

Se utilizar um cluster de versão 1.8.5 ou superior e estaticamente a criar o objeto de volume persistant, opções de montagem tem de ser especificado o `PersistentVolume` objeto. Para obter mais informações sobre a criação de estaticamente um volume persistant, consulte [estático Volumes persistente][pv-static].

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  azureFile:
    secretName: azure-secret
    shareName: azurefile
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  ```

Se utilizar um cluster versão 1.8.0 - 1.8.4, de um contexto de segurança pode ser especificado com o `runAsUser` valor definido como `0`. Para obter mais informações sobre o contexto de segurança Pod, consulte [configurar um contexto de segurança][kubernetes-security-context].

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes utilizando ficheiros do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para ficheiros do Azure][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/#azure-file
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[pv-static]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[mount-options]: #mount-options
