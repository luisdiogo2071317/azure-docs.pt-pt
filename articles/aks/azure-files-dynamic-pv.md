---
title: Utilize ficheiros do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4873b98c8ba4f1e574be20baebef3b6860341529
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="persistent-volumes-with-azure-files---dynamic-provisioning"></a>Aprovisionamento de volumes persistentes com ficheiros do Azure - dinâmicos

Um volume persistente representa um conjunto de armazenamento que tenha sido aprovisionado para utilização num Kubernetes cluster. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Este documento fornece detalhes sobre o aprovisionamento dinâmico de uma partilha de ficheiros do Azure como um volume persistente Kubernetes num AKS cluster. 

Para obter mais informações sobre volumes persistentes Kubernetes, consulte [volumes persistentes Kubernetes][kubernetes-volumes].

## <a name="prerequisites"></a>Pré-requisitos

Quando aprovisionar dinamicamente uma partilha de ficheiros do Azure como um volume Kubernetes, pode ser utilizada qualquer conta de armazenamento, desde que está contido no mesmo grupo de recursos do cluster AKS. Se necessário, crie uma conta do storage no mesmo grupo de recursos do cluster AKS. 

Para identificar o grupo de recursos correto, utilize o [lista de grupos de az] [ az-group-list] comando.

```azurecli-interactive
az group list --output table
```

O resultado de exemplo seguinte mostra os grupos de recursos, que ambos associados a um cluster AKS. O grupo de recursos com um nome, como *MC_myAKSCluster_myAKSCluster_eastus* contém os recursos de cluster AKS e é onde a conta de armazenamento tem de ser criado. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Assim que o grupo de recursos foi identificado, criar a conta de armazenamento com o [criar conta de armazenamento az] [ az-storage-account-create] comando.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Criar a classe de armazenamento

Uma classe de armazenamento é utilizada para definir como um volume persistente dinamicamente criado está configurado. Itens, tais como o nome da conta de armazenamento do Azure, o SKU e a região são definidas no objeto de classe de armazenamento. Para obter mais informações sobre Kubernetes classes de armazenamento, consulte [Kubernetes armazenamento Classes][kubernetes-storage-classes].

O exemplo seguinte especifica que tipo de qualquer conta de armazenamento de SKU `Standard_LRS` no `eastus` região pode ser utilizada ao pedir armazenamento. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Para utilizar uma conta de armazenamento específico, o `storageAccount` parâmetro pode ser utilizado.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Criar afirmações volume persistente

Uma afirmação de volume persistente utiliza o objeto de classe de armazenamento para aprovisionar dinamicamente um conjunto de armazenamento. Quando utilizar uma ficheiros do Azure, é criada uma partilha de ficheiros do Azure na conta de armazenamento selecionada ou especificado no objeto de classe de armazenamento.

>  [!NOTE]
>   Certifique-se de que uma conta de armazenamento adequado tiver sido previamente criada no mesmo grupo de recursos, como os recursos de cluster AKS. Este grupo de recursos possui um nome, como *MC_myAKSCluster_myAKSCluster_eastus*. A afirmação de volume persistente irão falhar para aprovisionar a Azure partilha de ficheiros se uma conta de armazenamento não está disponível. 

O manifesto seguinte pode ser utilizado para criar uma afirmação de volume persistente `5GB` tamanho com `ReadWriteOnce` acesso. Para obter mais informações sobre modos de acesso de PVC, consulte [modos de acesso][access-modes].

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

## <a name="using-the-persistent-volume"></a>Utilizar o volume persistente

Depois da afirmação persistente volume foi criada, e o armazenamento aprovisionado com êxito, um pod pode ser criado com acesso ao volume. O manifesto seguinte cria um pod que utiliza a afirmação de volume persistente `azurefile` para montar a partilha de ficheiros do Azure, o `/var/www/html` caminho. 

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
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

## <a name="mount-options"></a>Opções de montagem

Valores predefinidos para fileMode e dirMode diferem entre versões Kubernetes conforme descrito na seguinte tabela. 

| versão | valor |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| V1.8.0 v1.8.5 | 0700 |
| V1.8.6 ou superior | 0755 |
| V1.9.0 | 0700 |
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
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create