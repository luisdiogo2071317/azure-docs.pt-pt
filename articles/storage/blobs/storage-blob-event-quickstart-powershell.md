---
title: Encaminhar eventos de armazenamento de Blobs do Azure para um ponto de final web personalizado - Powershell | Microsoft Docs
description: Utilize a Azure Event Grid para subscrever a eventos de armazenamento de Blobs.
services: storage,event-grid
keywords: 
author: david-stanford
ms.author: dastanfo
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 329a79511b810159244b5530a49a5916440d2046
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>Eventos de armazenamento de BLOBs de rota para um ponto de final web personalizado com o PowerShell

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, utilizar o Azure PowerShell para subscrever eventos de armazenamento de BLOBs, acionador um evento e ver o resultado. 

Normalmente, os eventos são enviados para um ponto final que responde ao evento, como um webhook ou uma Função do Azure. Para simplificar o exemplo apresentado neste artigo, os eventos são enviados para um URL que recolhe apenas as mensagens. Criar este URL, utilizando ferramentas de terceiros partir [RequestBin](https://requestb.in/) ou [Hookbin](https://hookbin.com/).

> [!NOTE]
> **RequestBin** e **Hookbin** não se destina a utilização de débito elevado. A utilização destas ferramentas está puramente demonstrative. Se emitir mais do que um evento simultaneamente, não poderá ver todos os eventos na ferramenta.

Quando concluir os passos descritos neste artigo, pode ver que os dados do evento foram enviados para um ponto final.

![Dados do evento](./media/storage-blob-event-quickstart/request-result.png)

## <a name="setup"></a>Configurar

Este artigo requer a utilização da versão mais recente do Azure PowerShell. Se precisar de instalar ou atualizar, veja [Instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Login-AzureRmAccount` e siga as instruções no ecrã para autenticar.

```powershell
Login-AzureRmAccount
```

> [!NOTE]
> Disponibilidade de eventos de armazenamento está associada à grelha de evento [disponibilidade](../../event-grid/overview.md) e ficará disponível noutras regiões como sucede grelha de eventos.

Este exemplo utiliza **westus2** e armazena a seleção numa variável para utilização ao longo.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os tópicos do Event Grid são recursos do Azure e têm de ser colocados num grupo de recursos do Azure. Um grupo de recursos é uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

O exemplo seguinte cria um grupo de recursos com o nome **gridResourceGroup** na localização **westus2**.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

Para utilizar eventos de armazenamento de BLOBs, precisa de um uma [conta de armazenamento de BLOBs](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ou um [conta de armazenamento de v2 de objetivo geral](../common/storage-account-options.md#general-purpose-v2). **V2 de objetivo geral (GPv2)** são contas de armazenamento que suportam todas as funcionalidades para todos os serviços de armazenamento, incluindo tabelas, filas, ficheiros e Blobs. A **conta de armazenamento de BLOBs** é uma conta do storage especializadas para armazenar os dados não estruturados como blobs (objetos) no Storage do Azure. Contas do blob storage são como as contas do storage para fins gerais e partilham todas as excelentes características de durabilidade, disponibilidade, escalabilidade e desempenho funcionalidades que utiliza atualmente, incluindo 100% de consistência de API para blobs de blocos e blobs de acréscimo. Para aplicações que requerem apenas armazenamento de blobs de blocos ou de blobs de acréscimo, recomendamos a utilização das contas de armazenamento de Blobs.  

Criar uma conta de armazenamento de Blobs com a utilização de replicação do LRS [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), em seguida, obter o contexto da conta de armazenamento que define a conta de armazenamento a ser utilizado. Quando a atuar numa conta de armazenamento, referenciar o contexto em vez de repetidamente fornecer as credenciais. Este exemplo cria uma conta de armazenamento denominada **gridstorage** com encriptação storage(LRS) e BLOBs localmente redundante (ativada por predefinição). 

> [!NOTE]
> Os nomes das contas de armazenamento estão num espaço de nomes global, por isso terá de anexar alguns carateres aleatórias para o nome fornecido neste script.

```powershell
$storageName = "gridstorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind BlobStorage `
  -AccessTier Hot

$ctx = $storageAccount.Context
```

## <a name="create-a-message-endpoint"></a>Criar um ponto final de mensagem

Antes de subscrever o tópico, vamos criar o ponto final para a mensagem de evento. Em vez de escrever código para responder ao evento, vamos criar um ponto final que recolhe as mensagens, para que possa vê-las. RequestBin e Hookbin apresentamos as ferramentas de terceiros que permitem-lhe criar um ponto final e ver pedidos que são enviados para o mesmo. Aceda a [RequestBin](https://requestb.in/)e clique em **criar um RequestBin**, ou vá para [Hookbin](https://hookbin.com/) e clique em **criar novo ponto de final**. Copie o URL de bin e substitua `<bin URL>` no seguinte script.

```powershell
$binEndPoint = "<bin URL>"
```

## <a name="subscribe-to-your-storage-account"></a>Subscrever à sua conta de armazenamento

Subscreva um tópico para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve à conta de armazenamento que criou e transmite o URL de RequestBin ou Hookbin como o ponto final da notificação de evento. 

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $binEndPoint `
  -ResourceId $storageId
```

## <a name="trigger-an-event-from-blob-storage"></a>Acionar um evento a partir do armazenamento de Blobs

Agora, vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Em primeiro lugar, vamos criar um contentor e um objeto. Em seguida, vamos carregar o objeto para o contentor.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Navegar para o URL de ponto final que criou anteriormente. Em alternativa, clique em Atualizar no seu browser abrir. Vê o evento que acabou de enviar. 

```json
[{
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Storage/storageAccounts/myblobstorageaccount",
  "subject": "/blobServices/default/containers/gridcontainer/blobs/gridTestFile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T20:33:51.0595757Z",
  "id": "4d96b1d4-0001-00b3-58ce-16568c064fab",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "Azure-Storage-PowerShell-d65ca2e2-a168-4155-b7a4-2c925c18902f",
    "requestId": "4d96b1d4-0001-00b3-58ce-16568c000000",
    "eTag": "0x8D4E4E61AE038AD",
    "contentType": "application/octet-stream",
    "contentLength": 0,
    "blobType": "BlockBlob",
    "url": "https://myblobstorageaccount.blob.core.windows.net/gridcontainer/gridTestFile.txt",
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

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como criar tópicos e subscrições de eventos, saiba mais sobre os Eventos de armazenamento de Blobs o que o Event Grid pode ajudá-lo a fazer:

- [Reagir aos eventos de armazenamento de Blobs](storage-blob-event-overview.md)
- [Sobre o Event Grid](../../event-grid/overview.md)
