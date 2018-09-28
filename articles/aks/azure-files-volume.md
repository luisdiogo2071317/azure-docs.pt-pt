---
title: Criar um volume estático para vários pods no Azure Kubernetes Service (AKS)
description: Saiba como criar manualmente um volume com ficheiros do Azure para utilização com vários pods em simultâneo no Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: e5518ebb2985635507368943774e6be803cfffa8
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409060"
---
# <a name="manually-create-and-use-an-azure-files-share-in-azure-kubernetes-service-aks"></a>Crie manualmente e utilizar uma partilha de ficheiros do Azure no Azure Kubernetes Service (AKS)

Aplicações baseadas em contentores, muitas vezes, tem de aceder e manter os dados num volume de dados externa. Se precisam de vários pods acesso simultâneo no mesmo volume de armazenamento, pode utilizar ficheiros do Azure para ligar através da [protocolo Server Message Block (SMB)][smb-overview]. Este artigo mostra-lhe como criar uma partilha de ficheiros do Azure e anexá-lo a um pod no AKS manualmente.

Para obter mais informações sobre volumes do Kubernetes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que tem um cluster do AKS existente. Se precisar de um cluster do AKS, consulte o guia de introdução do AKS [com a CLI do Azure] [ aks-quickstart-cli] ou [no portal do Azure][aks-quickstart-portal].

Também precisa da versão 2.0.46 da CLI do Azure ou posterior instalado e configurado. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure

Antes de poder utilizar os ficheiros do Azure como um volume do Kubernetes, tem de criar uma conta de armazenamento do Azure e a partilha de ficheiros. O seguinte script cria um grupo de recursos chamado *myAKSShare*, uma conta de armazenamento e uma partilha de ficheiros com o nome *aksshare*:

```azurecli
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

Anote o nome da conta de armazenamento e a chave mostrado no final de saída do script. Estes valores são necessários quando criar o volume do Kubernetes em um dos seguintes passos.

## <a name="create-a-kubernetes-secret"></a>Crie um Kubernetes secreta

Kubernetes necessita de credenciais para aceder à partilha de ficheiro criada no passo anterior. Estas credenciais são armazenadas num [segredo do Kubernetes][kubernetes-secret], que é referenciado quando cria um pod de Kubernetes.

Utilize o `kubectl create secret` comando para criar o segredo. O exemplo seguinte cria um partilhado com o nome *azure-secret*. Substitua *STORAGE_ACCOUNT_NAME* com o nome da conta de armazenamento mostrado na saída do passo anterior, e *STORAGE_ACCOUNT_KEY* com a sua chave de armazenamento:

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Montar a partilha de ficheiros como um volume

Para montar a partilha de ficheiros do Azure no seu pod, configure o volume na especificação do contentor. Crie um novo ficheiro designado `azure-files-pod.yaml` com o seguinte conteúdo. Se tiver alterado o nome da partilha de ficheiros ou nome do segredo, atualize o *shareName* e *secretName*. Se assim o desejar, atualize o `mountPath`, que é o caminho onde os ficheiros partilham esteja montada no pod.

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

Utilize o `kubectl` comando para criar o pod.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Tem agora um pod em execução com uma partilha de ficheiros do Azure montada em */mnt/azure*. Pode usar `kubectl describe pod azure-files-pod` para verificar a partilha é montada com êxito.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre clusters do AKS interagir com os ficheiros do Azure, consulte a [Plug-in do Kubernetes para ficheiros do Azure][kubernetes-files].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
