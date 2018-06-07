---
title: Encaminhar eventos de armazenamento de Blobs do Azure para um ponto de final web personalizado - Powershell | Microsoft Docs
description: Utilize a Azure Event Grid para subscrever a eventos de armazenamento de Blobs.
services: storage,event-grid
keywords: ''
author: david-stanford
ms.author: dastanfo
ms.date: 05/24/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: b6764ffa0e7cfbc888f11c22af855d48d8160372
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650507"
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>Eventos de armazenamento de BLOBs de rota para um ponto de final web personalizado com o PowerShell

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, utilizar o Azure PowerShell para subscrever eventos de armazenamento de BLOBs, acionador um evento e ver o resultado. 

Normalmente, pode enviar eventos para um ponto final que processa os dados de eventos e efetua ações. No entanto, para simplificar este artigo, enviar eventos para uma aplicação web que recolhe e apresenta as mensagens.

Quando tiver terminado, verá que os dados do evento foi enviados para a aplicação web.

![Ver resultados](./media/storage-blob-event-quickstart-powershell/view-results.png)

## <a name="setup"></a>Configurar

Este artigo requer a utilização da versão mais recente do Azure PowerShell. Se precisar de instalar ou atualizar, veja [Instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã para autenticar.

```powershell
Connect-AzureRmAccount
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

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Para utilizar eventos de armazenamento de BLOBs, precisa de um uma [conta de armazenamento de BLOBs](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ou um [conta de armazenamento de v2 de objetivo geral](../common/storage-account-options.md#general-purpose-v2). **V2 de objetivo geral (GPv2)** são contas de armazenamento que suportam todas as funcionalidades para todos os serviços de armazenamento, incluindo tabelas, filas, ficheiros e Blobs. A **conta de armazenamento de BLOBs** é uma conta do storage especializadas para armazenar os dados não estruturados como blobs (objetos) no Storage do Azure. Contas do blob storage são como as contas do storage para fins gerais e partilham todas as excelentes características de durabilidade, disponibilidade, escalabilidade e desempenho funcionalidades que utiliza atualmente, incluindo 100% de consistência de API para blobs de blocos e blobs de acréscimo. Para aplicações que requerem apenas armazenamento de blobs de blocos ou de blobs de acréscimo, recomendamos a utilização das contas de armazenamento de Blobs.  

Criar uma conta de armazenamento de Blobs com a utilização de replicação do LRS [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), em seguida, obter o contexto da conta de armazenamento que define a conta de armazenamento a ser utilizado. Ao efetuar ações em contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais. Este exemplo cria uma conta de armazenamento denominada **gridstorage** com armazenamento localmente redundante (LRS). 

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

Antes de subscrever o tópico, vamos criar o ponto final para a mensagem de evento. Normalmente, o ponto final executa ações com base nos dados de eventos. Para simplificar este guia de introdução, implementar um [aplicação web pré-criadas](https://github.com/dbarkol/azure-event-grid-viewer) que apresenta as mensagens de evento. A solução implementada inclui um plano do App Service, uma aplicação web do app Service e o código de origem a partir do GitHub.

Substitua `<your-site-name>` com um nome exclusivo para a sua aplicação web. O nome da aplicação web deve ser exclusivo, porque faz parte da entrada de DNS.

```powershell
$sitename="<your-site-name>"

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateUri "https://raw.githubusercontent.com/dbarkol/azure-event-grid-viewer/master/azuredeploy.json" `
  -siteName $sitename `
  -hostingPlanName viewerhost
```

A implementação pode demorar alguns minutos a concluir. Após a implementação é concluída com êxito, ver a sua aplicação web para se certificar de está em execução. Num browser, navegue para: `https://<your-site-name>.azurewebsites.net`

Deverá ver o site com nenhuma mensagens atualmente apresentadas.

## <a name="subscribe-to-your-storage-account"></a>Subscrever à sua conta de armazenamento

Subscreva um tópico para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve à conta de armazenamento que criou e transmite o URL da sua aplicação web, como o ponto final da notificação de evento. O ponto final para a sua aplicação web tem de incluir o sufixo `/api/updates/`.

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
$endpoint="https://$sitename.azurewebsites.net/api/updates"

New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $endpoint `
  -ResourceId $storageId
```

Ver a sua aplicação web novamente e tenha em atenção que um evento de validação de subscrição foi enviado ao mesmo. Selecione o ícone de olho para expandir os dados do evento. Grelha de eventos envia o evento de validação para que o ponto final pode Certifique-se de que pretende receber dados de eventos. A aplicação web inclui código para validar a subscrição.

![Ver evento de subscrição](./media/storage-blob-event-quickstart-powershell/view-subscription-event.png)

## <a name="trigger-an-event-from-blob-storage"></a>Acionar um evento a partir do armazenamento de Blobs

Agora, vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Em primeiro lugar, vamos criar um contentor e um objeto. Em seguida, vamos carregar o objeto para o contentor.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Ver a sua aplicação web para ver o evento que acabámos de enviar.

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
