---
title: Utilize ficheiros do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b6267dd2bc1b29229b2e8016e2429ed88b7bf676
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Utilizar os ficheiros do Azure com Kubernetes

Aplicações baseadas no contentor muitas vezes necessitam de acesso e manter os dados de um volume de dados externas. Ficheiros do Azure podem ser utilizados como este arquivo de dados externas. Este detalhes de artigo com ficheiros do Azure como um volume Kubernetes no serviço de contentor do Azure.

Para obter mais informações sobre Kubernetes volumes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure

Antes de utilizar uma partilha de ficheiros do Azure como um volume Kubernetes, tem de criar uma conta de armazenamento do Azure e a partilha de ficheiros. O script seguinte pode ser utilizado para concluir estas tarefas. Tome nota ou atualizar os valores de parâmetros, alguns deles são necessários quando criar o volume de Kubernetes.

```azurecli-interactive
# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)
```

## <a name="create-kubernetes-secret"></a>Criar Kubernetes segredo

Kubernetes necessita de credenciais para aceder à partilha de ficheiros. Estas credenciais são armazenadas num [Kubernetes segredo][kubernetes-secret], que é referenciado durante a criação de um pod Kubernetes.

Ao criar um Kubernetes secreta, os valores secretos tem de ser codificado em base64.

Em primeiro lugar, codificar o nome da conta de armazenamento. Se necessário, substitua `$AKS_PERS_STORAGE_ACCOUNT_NAME` com o nome da conta do storage do Azure.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

Em seguida, a codificar a chave de conta do storage. Se necessário, substitua `$STORAGE_KEY` com o nome da chave de conta do storage do Azure.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Crie um ficheiro denominado `azure-secret.yaml` e copie o seguinte YAML. Atualização do `azurestorageaccountname` e `azurestorageaccountkey` valores com o base64 codificado valores obtidos no passo anterior.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Utilize o [kubectl criar] [ kubectl-create] comando para criar o segredo.

```azurecli-interactive
kubectl create -f azure-secret.yaml
```

## <a name="mount-file-share-as-volume"></a>Montar a partilha de ficheiros como volume

É possível montar a partilha de ficheiros do Azure no seu pod configurando o volume na respetiva especificação. Criar um novo ficheiro designado `azure-files-pod.yaml` com o seguinte conteúdo. Atualização `aksshare` com o nome atribuído ao Azure Files partilhar.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Utilize kubectl para criar um pod.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Tem agora um contentor em execução com a partilha de ficheiros do Azure montada no `/mnt/azure` diretório. Pode ver o quando inspecionar o pod através de montagem do volume `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de Kubernetes utilizando ficheiros do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para ficheiros do Azure][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
