---
title: "Montar um volume de ficheiros do Azure em instâncias de contentor do Azure"
description: "Saiba como montar um volume de ficheiros do Azure para manter o estado com instâncias de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/05/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b2e8e27cecb4d1225e378690063b42f5d5242868
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Montar uma partilha de ficheiros do Azure com instâncias de contentor do Azure

Por predefinição, as instâncias de contentor do Azure são sem monitorização de estado. Se o contentor de falhas de ou para, todos os respetivo estado é perdido. Para manter o estado para além da duração do contentor, tem de montar um volume de um arquivo de externo. Este artigo mostra como montar uma partilha de ficheiros do Azure para utilização com instâncias de contentor do Azure.

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure

Antes de utilizar uma partilha de ficheiros do Azure com instâncias de contentor do Azure, tem de criá-la. Execute o script seguinte para criar uma conta de armazenamento para alojar a partilha de ficheiros e a partilha de si próprio. O nome da conta de armazenamento tem de ser globalmente exclusivo para que o script adiciona um valor aleatório para a cadeia de base.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Adquirir detalhes de acesso da conta de armazenamento

Para montar uma partilha de ficheiros do Azure como um volume em instâncias de contentor do Azure, terá três valores: o nome da conta de armazenamento, o nome da partilha e a chave de acesso de armazenamento.

Se utilizou o script acima, o nome da conta de armazenamento foi criado com um valor aleatório no final. Para consultar a cadeia final (incluindo a parte aleatória), utilize os seguintes comandos:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

O nome da partilha já é conhecido (definido como *acishare* no script acima), por isso, todas as que permanece é a chave de conta de armazenamento, que pode ser encontrada utilizando o seguinte comando:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="deploy-the-container-and-mount-the-volume"></a>Implementar o contentor e montar o volume

Para montar uma partilha de ficheiros do Azure como um volume num contentor, especifique a partilha e o volume de ponto de montagem ao criar o contentor com [criar contentor de az](/cli/azure/container#az_container_create). Se tiver seguido os passos anteriores, pode montar a partilha que criou anteriormente, utilizando o seguinte comando para criar um contentor:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Gerir ficheiros no volume montado

Depois do contentor é iniciado, pode utilizar a aplicação web simples implementada através de [aci/seanmckenna-hellofiles](https://hub.docker.com/r/seanmckenna/aci-hellofiles/) imagem para gerir os ficheiros na partilha de ficheiros do Azure no caminho de montagem que especificou. Obter o endereço IP da aplicação web com o [mostrar de contentor az](/cli/azure/container#az_container_show) comando:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles -o table
```

Pode utilizar o [portal do Azure](https://portal.azure.com) ou uma ferramenta como o [Explorador de armazenamento do Microsoft Azure](https://storageexplorer.com) para obter e Inspecione o ficheiro de escrita à partilha de ficheiros.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre a relação entre [instâncias de contentor do Azure e orchestrators contentor](container-instances-orchestrator-relationship.md).
