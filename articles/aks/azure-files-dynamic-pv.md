---
title: Utilizar ficheiros do Azure com o AKS
description: Utilizar discos do Azure com o AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bac80e354a4d629bfbfc8207b9fed7c69c765839
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429272"
---
# <a name="persistent-volumes-with-azure-files"></a>Volumes persistentes com os ficheiros do Azure

Um volume persistente é uma parte do armazenamento que foi criado para utilização num cluster de Kubernetes. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente criado. Detalhes este documento **criação dinâmica** de uma partilha de ficheiros do Azure como um volume persistente.

Para obter mais informações sobre o Kubernetes persistentes volumes, incluindo a criação estática, consulte [volumes persistentes do Kubernetes][kubernetes-volumes].

## <a name="create-storage-account"></a>Criar conta de armazenamento

Ao criar dinamicamente uma partilha de ficheiros do Azure como um volume do Kubernetes, qualquer conta de armazenamento pode ser utilizada, desde que está a ser o AKS **nó** grupo de recursos. Esse é o com o `MC_` prefixo que foi criado com o aprovisionamento de recursos para o cluster do AKS. Obtenha o nome do grupo de recursos com o [show de recursos de az] [ az-resource-show] comando.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Utilize o [criar conta de armazenamento az] [ az-storage-account-create] comando para criar a conta de armazenamento.

Atualização `--resource-group` com o nome do grupo de recursos recolhidos no último passo, e `--name` para um nome à sua escolha.

```azurecli-interactive
az storage account create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

> Os ficheiros do Azure só funcionam com o armazenamento standard. Se utilizar o armazenamento premium, o volume não aprovisionar.

## <a name="create-storage-class"></a>Criar a classe de armazenamento

Uma classe de armazenamento é utilizada para definir como uma partilha de ficheiros do Azure é criada. Pode ser especificada uma conta de armazenamento específico na classe. Se não for especificada uma conta de armazenamento, um `skuName` e `location` tem de ser especificado, e todas as contas de armazenamento no grupo de recursos associados são avaliadas para uma correspondência.

Para obter mais informações sobre classes de armazenamento do Kubernetes para os ficheiros do Azure, consulte [Classes de armazenamento do Kubernetes][kubernetes-storage-classes].

Crie um ficheiro denominado `azure-file-sc.yaml` e copie o seguinte manifesto. Atualização do `storageAccount` com o nome da conta de armazenamento de destino. Consulte a seção [opções de montagem] para obter mais informações sobre `mountOptions`.

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

Criar a classe de armazenamento com o [aplicam-se de kubectl] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-sc.yaml
```

## <a name="create-persistent-volume-claim"></a>Criar a afirmação de volume persistente

Uma afirmação de volume persistente (PVC) usa o objeto de classe de armazenamento para provisionar dinamicamente uma partilha de ficheiros do Azure.

O YAML seguinte pode ser utilizado para criar uma afirmação de volume persistente `5GB` tamanho com `ReadWriteMany` acesso. Para obter mais informações sobre os modos de acesso, consulte a [volume persistente do Kubernetes] [ access-modes] documentação.

Crie um ficheiro denominado `azure-file-pvc.yaml` e copie o YAML seguinte. Certifique-se de que o `storageClassName` corresponde a classe de armazenamento criada no último passo.

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

Criar a afirmação de volume persistente com o [aplicam-se de kubectl] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-file-pvc.yaml
```

Depois de concluído, será criada a partilha de ficheiros. Segredo do Kubernetes, também é criado que inclui informações de ligação e credenciais.

## <a name="using-the-persistent-volume"></a>Com o volume persistente

O YAML seguinte cria um pod que utiliza a afirmação de volume persistente `azurefile` montar a partilha de ficheiros do Azure a `/mnt/azure` caminho.

Crie um ficheiro denominado `azure-pvc-files.yaml`e copie o YAML seguinte. Certifique-se de que o `claimName` corresponde ao PVC criado no último passo.

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

Criar o pod com o [aplicam-se de kubectl] [ kubectl-apply] comando.

```azurecli-interactive
kubectl apply -f azure-pvc-files.yaml
```

Tem agora um pod em execução com o seu disco do Azure montado no `/mnt/azure` diretório. Esta configuração pode ser vista quando inspecionar o seu pod via `kubectl describe pod mypod`.

## <a name="mount-options"></a>Opções de montagem

Valores predefinidos para fileMode e dirMode são diferentes entre versões do Kubernetes, conforme descrito na tabela seguinte.

| versão | valor |
| ---- | ---- |
| v1.6.x, v1.7.x | 0777 |
| v1.8.0-v1.8.5 | 0700 |
| V1.8.6 ou superior | 0755 |
| v1.9.0 | 0700 |
| V1.9.1 ou superior | 0755 |

Se utilizar um cluster de versão 1.8.5 ou superior e criar dinamicamente o volume de persistant com uma classe de armazenamento, opções de montagem podem ser especificadas no objeto de classe de armazenamento. O exemplo seguinte define `0777`.

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

Se utilizar um cluster de versão 1.8.5 ou superior e estaticamente a criar o objeto de volume persistant, opções de montagem precisam ser especificado o `PersistentVolume` objeto. Para obter mais informações sobre como criar um volume persistant estaticamente, consulte [Volumes persistente estático][pv-static].

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

Se utilizar um cluster de versão 1.8.0 - 1.8.4, um contexto de segurança pode ser especificado com o `runAsUser` valor definido como `0`. Para obter mais informações sobre o contexto de segurança de Pod, consulte [configurar um contexto de segurança][kubernetes-security-context].

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes com ficheiros do Azure.

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
[az-group-create]: /cli/azure/group#az-group-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-storage-account-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[mount-options]: #mount-options
