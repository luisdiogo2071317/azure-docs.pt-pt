---
title: Utilizar ficheiros do Azure com o AKS
description: Utilizar discos do Azure com o AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b35d0e33009f76e0b2d6f90c52c98ce5f317792d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436765"
---
# <a name="volumes-with-azure-files"></a>Volumes com ficheiros do Azure

Aplicações baseadas em contentores, muitas vezes, tem de aceder e manter os dados num volume de dados externa. Ficheiros do Azure podem ser utilizados como este arquivo de dados externo. Este artigo descreve a utilização de ficheiros do Azure como um volume do Kubernetes no Azure Kubernetes Service.

Para obter mais informações sobre volumes do Kubernetes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure

Antes de utilizar uma partilha de ficheiros do Azure como um volume do Kubernetes, tem de criar uma conta de armazenamento do Azure e a partilha de ficheiros. O script seguinte pode ser utilizado para concluir estas tarefas. Tome nota ou Atualize os valores de parâmetro, alguns deles são necessárias ao criar o volume do Kubernetes.

```azurecli-interactive
#!/bin/bash

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

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

## <a name="create-kubernetes-secret"></a>Criar segredo do Kubernetes

Kubernetes necessita de credenciais para aceder à partilha de ficheiros. Estas credenciais são armazenadas num [segredo do Kubernetes][kubernetes-secret], que é referenciado durante a criação de um pod de Kubernetes.

Utilize o seguinte comando para criar o segredo. Substitua `STORAGE_ACCOUNT_NAME` com o nome da conta de armazenamento, e `STORAGE_ACCOUNT_KEY` com a sua chave de armazenamento.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Montar a partilha de ficheiros como volume

Monte a partilha de ficheiros do Azure no seu pod ao configurar o volume na sua especificação. Crie um novo ficheiro designado `azure-files-pod.yaml` com o seguinte conteúdo. Atualização `aksshare` com o nome fornecido para ficheiros do Azure partilhar.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
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

Utilize o kubectl para criar um pod.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Agora tem um contentor em execução com a partilha de ficheiros do Azure montada no `/mnt/azure` diretório.  Pode ver o volume de montagem quando inspecionar o seu pod via `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de Kubernetes com ficheiros do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para ficheiros do Azure][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
