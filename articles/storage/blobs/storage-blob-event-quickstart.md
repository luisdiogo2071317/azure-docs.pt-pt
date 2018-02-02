---
title: Encaminhar eventos de armazenamento de Blobs do Azure para um ponto final web personalizado | Microsoft Docs
description: Utilize a Azure Event Grid para subscrever a eventos de armazenamento de Blobs.
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 4f10d9b26cb75bee8103d986b7fa1197168c692f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-azure-cli"></a>Eventos de armazenamento de BLOBs de rota para um ponto de final web personalizado com a CLI do Azure

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, a CLI do Azure é utilizada para subscrever a eventos de armazenamento de Blobs e acionar o evento para ver o resultado. 

Normalmente, os eventos são enviados para um ponto final que responde ao evento, como um webhook ou uma Função do Azure. Para simplificar o exemplo apresentado neste artigo, enviamos eventos para um URL que apenas recolhe as mensagens. Criar este URL, utilizando ferramentas de terceiros partir [RequestBin](https://requestb.in/) ou [Hookbin](https://hookbin.com/).

> [!NOTE]
> **RequestBin** e **Hookbin** não se destina a utilização de débito elevado. A utilização destas ferramentas está puramente demonstrative. Se emitir mais do que um evento simultaneamente, não poderá ver todos os eventos na ferramenta.

Quando concluir os passos descritos neste artigo, pode ver que os dados do evento foram enviados para um ponto final.

![Dados do evento](./media/storage-blob-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão mais recente da CLI do Azure (2.0.24 ou posterior). Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli).

Se não estiver a utilizar o Cloud Shell, tem primeiro de iniciar sessão com o `az login`.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os tópicos do Event Grid são recursos do Azure e têm de ser colocados num grupo de recursos do Azure. Um grupo de recursos é uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). 

O exemplo seguinte cria um grupo de recursos com o nome `<resource_group_name>` na localização *westcentralus*.  Substitua `<resource_group_name>` por um nome exclusivo para o seu grupo de recursos.

```azurecli-interactive
az group create --name <resource_group_name> --location westcentralus
```

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

Para utilizar eventos de armazenamento de BLOBs, precisa de um uma [conta de armazenamento de BLOBs](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ou um [conta de armazenamento de v2 de objetivo geral](../common/storage-account-options.md#general-purpose-v2). **V2 de objetivo geral (GPv2)** são contas de armazenamento que suportam todas as funcionalidades para todos os serviços de armazenamento, incluindo tabelas, filas, ficheiros e Blobs. A **conta de armazenamento de BLOBs** é uma conta do storage especializadas para armazenar os dados não estruturados como blobs (objetos) no Storage do Azure. Contas do blob storage são como as contas do storage para fins gerais e partilham todas as excelentes características de durabilidade, disponibilidade, escalabilidade e desempenho funcionalidades que utiliza atualmente, incluindo 100% de consistência de API para blobs de blocos e blobs de acréscimo. Para aplicações que requerem apenas armazenamento de blobs de blocos ou de blobs de acréscimo, recomendamos a utilização das contas de armazenamento de Blobs. 

> [!NOTE]
> Disponibilidade de eventos de armazenamento está associada à grelha de evento [disponibilidade](../../event-grid/overview.md) e ficará disponível noutras regiões como sucede grelha de eventos.

Substitua `<storage_account_name>` por um nome exclusivo para a conta de armazenamento, e `<resource_group_name>` com o grupo de recursos que criou anteriormente.

```azurecli-interactive
az storage account create \
  --name <storage_account_name> \
  --location westcentralus \
  --resource-group <resource_group_name> \
  --sku Standard_LRS \
  --kind BlobStorage \
  --access-tier Hot
```

## <a name="create-a-message-endpoint"></a>Criar um ponto final de mensagem

Antes de subscrever o tópico, vamos criar o ponto final para a mensagem de evento. Em vez de escrever código para responder ao evento, vamos criar um ponto final que recolhe as mensagens, para que possa vê-las. RequestBin e Hookbin apresentamos as ferramentas de terceiros que permitem-lhe criar um ponto final e ver pedidos que são enviados para o mesmo. Aceda a [RequestBin](https://requestb.in/)e clique em **criar um RequestBin**, ou vá para [Hookbin](https://hookbin.com/) e clique em **criar novo ponto de final**.  Copie o URL do bin, porque irá precisar dele quando subscrever o tópico.

## <a name="subscribe-to-your-storage-account"></a>Subscrever à sua conta de armazenamento

Subscreva um tópico para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve à conta de armazenamento que criou e transmite o URL de RequestBin ou Hookbin como o ponto final da notificação de evento. Substitua `<event_subscription_name>` por um nome exclusivo para a sua subscrição de eventos e `<endpoint_URL>` pelo o valor da secção anterior. Ao especificar um ponto final quando subscrever, o Event Grid processa o encaminhamento de eventos para esse ponto final. Para `<resource_group_name>` e `<storage_account_name>`, utilize o valor que criou anteriormente.  

```azurecli-interactive
storageid=$(az storage account show --name <storage_account_name> --resource-group <resource_group_name> --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
```

## <a name="trigger-an-event-from-blob-storage"></a>Acionar um evento a partir do armazenamento de Blobs

Agora, vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Em primeiro lugar, vamos configurar o nome e chave para a conta de armazenamento, em seguida, vamos criar um contentor, em seguida, criar e carregar um ficheiro. Novamente, utilize os valores para `<storage_account_name>` e `<resource_group_name>` que criou anteriormente.

```azurecli-interactive
export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY="$(az storage account keys list --account-name <storage_account_name> --resource-group <resource_group_name> --query "[0].value" --output tsv)"

az storage container create --name testcontainer

touch testfile.txt
az storage blob upload --file testfile.txt --container-name testcontainer --name testfile.txt
```

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Navegar para o URL de ponto final que criou anteriormente. Em alternativa, clique em Atualizar no seu browser abrir. Vê o evento que acabou de enviar. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "text/plain",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/testcontainer/testblob1.txt",
    "sequencer": "00000000000000EB0000000000046199",
    "storageDiagnostics": {
      "batchId": "dffea416-b46e-4613-ac19-0371c0c5e352"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

## <a name="clean-up-resources"></a>Limpar recursos
Se quiser continuar a trabalhar com esta conta de armazenamento e subscrição de eventos, não limpe os recursos criados neste artigo. Se não quiser continuar, utilize o comando seguinte para eliminar os recursos que criou neste artigo.

Substitua `<resource_group_name>` pelo grupo de recursos que criou acima.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como criar tópicos e subscrições de eventos, saiba mais sobre os Eventos de armazenamento de Blobs o que o Event Grid pode ajudá-lo a fazer:

- [Reagir aos eventos de armazenamento de Blobs](storage-blob-event-overview.md)
- [Sobre o Event Grid](../../event-grid/overview.md)
