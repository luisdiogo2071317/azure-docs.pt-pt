---
title: "Montar um volume de ficheiros do Azure em instâncias de contentor do Azure"
description: "Saiba como montar um volume de ficheiros do Azure para manter o estado com instâncias de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: be502e6aef39ee4ed8cfc1f8926cb556dc1defb1
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Montar uma partilha de ficheiros do Azure com instâncias de contentor do Azure

Por predefinição, as instâncias de contentor do Azure são sem monitorização de estado. Se o contentor de falhas de ou para, todos os respetivo estado é perdido. Para manter o estado para além da duração do contentor, tem de montar um volume de um arquivo de externo. Este artigo mostra como montar uma partilha de ficheiros do Azure para utilização com instâncias de contentor do Azure.

> [!NOTE]
> Montar uma partilha de ficheiros do Azure está atualmente restrita para contentores de Linux. Enquanto que estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar a atual plataforma as diferenças no [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md).

## <a name="create-an-azure-file-share"></a>Criar uma partilha de ficheiros do Azure

Antes de utilizar uma partilha de ficheiros do Azure com instâncias de contentor do Azure, tem de criá-la. Execute o script seguinte para criar uma conta de armazenamento para alojar a partilha de ficheiros e a partilha de si próprio. O nome da conta de armazenamento tem de ser globalmente exclusivo para que o script adiciona um valor aleatório para a cadeia de base.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $ACI_PERS_RESOURCE_GROUP --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="get-storage-credentials"></a>Obter as credenciais do armazenamento

Para montar uma partilha de ficheiros do Azure como um volume em instâncias de contentor do Azure, terá três valores: o nome da conta de armazenamento, o nome da partilha e a chave de acesso de armazenamento.

Se utilizou o script acima, o nome da conta de armazenamento foi criado com um valor aleatório no final. Para consultar a cadeia final (incluindo a parte aleatória), utilize os seguintes comandos:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

O nome da partilha já é conhecido (definido como *acishare* no script acima), por isso, todas as que permanece é a chave de conta de armazenamento, que pode ser encontrada utilizando o seguinte comando:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a>Implementar o contentor e montagem de volume

Para montar uma partilha de ficheiros do Azure como um volume num contentor, especifique a partilha e o volume de ponto de montagem ao criar o contentor com [criar contentor de az][az-container-create]. Se tiver seguido os passos anteriores, pode montar a partilha que criou anteriormente, utilizando o seguinte comando para criar um contentor:

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

Depois do contentor é iniciado, pode utilizar a aplicação web simples implementada através de [aci/seanmckenna-hellofiles] [ aci-hellofiles] imagem para gerir os ficheiros na partilha de ficheiros do Azure no caminho de montagem que especificou. Obter o endereço IP da aplicação web com o [mostrar de contentor az] [ az-container-show] comando:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --output table
```

Pode utilizar o [portal do Azure] [ portal] ou uma ferramenta como o [Explorador de armazenamento do Microsoft Azure] [ storage-explorer] para obter e Inspecione o ficheiro de escrita a partilha de ficheiros.

## <a name="mount-multiple-volumes"></a>Montar os volumes de vários

Para montar vários volumes numa instância de contentor, tem de implementar utilizando um [modelo Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Em primeiro lugar, forneça os detalhes de partilha e definir os volumes preenchendo o `volumes` matriz no `properties` secção do modelo. Por exemplo, se criou duas partilhas de ficheiros do Azure com o nome *share1* e *share2* na conta do storage *myStorageAccount*, a `volumes` aparecia matriz semelhante ao seguinte:

```json
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Em seguida, para cada contentor no grupo de contentor no qual gostaria de montar os volumes, preencher o `volumeMounts` matriz no `properties` secção da definição de contentor. Por exemplo, isto monta dois volumes, *myvolume1* e *myvolume2*, definido anteriormente:

```json
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

Para ver um exemplo de implementação de instância de contentor com um modelo Azure Resource Manager, consulte [implementar o contentor de vários grupos em instâncias de contentor do Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a relação entre [instâncias de contentor do Azure e orchestrators contentor](container-instances-orchestrator-relationship.md).

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/r/seanmckenna/aci-hellofiles/
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show