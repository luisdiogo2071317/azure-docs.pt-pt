---
title: Encaminhar eventos de armazenamento de Blobs do Azure para um ponto de extremidade de web personalizados - Powershell | Documentos da Microsoft
description: Utilize a Azure Event Grid para subscrever a eventos de armazenamento de Blobs.
services: storage,event-grid
keywords: ''
author: david-stanford
ms.author: dastanfo
ms.date: 07/05/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 2c61c58398b8c095002db4bc59afed1c95e3550f
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865425"
---
# <a name="route-blob-storage-events-to-a-custom-web-endpoint-with-powershell"></a>Encaminhar eventos de armazenamento de BLOBs para um ponto final web personalizado com o PowerShell

O Azure Event Grid é um serviço de eventos para a cloud. Neste artigo, vai utilizar o Azure PowerShell para subscrever a eventos de armazenamento de BLOBs, acionar um evento e ver o resultado. 

Normalmente, envia eventos para um ponto final que processa os dados de eventos e efetua ações. No entanto, para simplificar este artigo, vai enviar eventos para uma aplicação Web que recolhe e apresenta as mensagens.

Quando tiver terminado, verá que os dados do evento foram enviados para a aplicação Web.

![Ver resultados](./media/storage-blob-event-quickstart-powershell/view-results.png)

## <a name="setup"></a>Configurar

Este artigo requer a utilização da versão mais recente do Azure PowerShell. Se precisar de instalar ou atualizar, veja [Instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã para autenticar.

```powershell
Connect-AzureRmAccount
```

> [!NOTE]
> Disponibilidade para eventos de armazenamento está associada ao Event Grid [disponibilidade](../../event-grid/overview.md) e ficará disponível noutras regiões como o Event Grid.

Este exemplo utiliza **westus2** e armazena a seleção numa variável para uso.

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

Para utilizar eventos de armazenamento de Blobs, precisa de uma [conta de armazenamento de Blobs](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) ou uma [conta de armazenamento de Fins Gerais v2](../common/storage-account-options.md#general-purpose-v2). As contas de **Fins Gerais v2 (GPv2)** são contas de armazenamento que suportam todas as funcionalidades de todos os serviços de armazenamento, incluindo Blobs, Ficheiros, Filas e Tabelas. Uma **conta de armazenamento de Blobs** é uma conta de armazenamento especializada para armazenar os seus dados não estruturados como blobs (objetos) no Armazenamento do Azure. As contas de armazenamento de Blobs são semelhantes às contas de armazenamento para fins gerais e partilham todas as excelentes características de durabilidade, disponibilidade, escalabilidade e desempenho que utiliza atualmente, incluindo 100% de consistência com a API dos blobs de blocos e dos blobs de acréscimo. Para aplicações que requerem apenas armazenamento de blobs de blocos ou de blobs de acréscimo, recomendamos a utilização das contas de armazenamento de Blobs.  

Criar uma conta de armazenamento de Blobs com replicação LRS através de [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), em seguida, obter o contexto de conta de armazenamento que define a conta de armazenamento a ser utilizado. Ao efetuar ações em contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais. Este exemplo cria uma conta de armazenamento denominada **gridstorage** com armazenamento localmente redundante (LRS). 

> [!NOTE]
> Nomes de conta de armazenamento estão num espaço de nomes global, por isso terá de acrescentar alguns caracteres aleatórios para o nome fornecido neste script.

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

Antes de subscrever o tópico, vamos criar o ponto final para a mensagem de evento. Normalmente, o ponto final executa as ações com base nos dados do evento. Para simplificar este início rápido, vai implementar uma [aplicação Web pré-criada](https://github.com/dbarkol/azure-event-grid-viewer) para apresentar as mensagens de evento. A solução implementada inclui um plano do Serviço de Aplicações, uma aplicação Web do Serviço de Aplicações e o código de origem do GitHub.

Substitua `<your-site-name>` por um nome exclusivo para a aplicação Web. O nome da aplicação Web deve ser exclusivo, porque faz parte da entrada DNS.

```powershell
$sitename="<your-site-name>"

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateUri "https://raw.githubusercontent.com/dbarkol/azure-event-grid-viewer/master/azuredeploy.json" `
  -siteName $sitename `
  -hostingPlanName viewerhost
```

A implementação pode demorar alguns minutos. Após a implementação ter sido concluída com êxito, verifique a aplicação Web para verificar se está em execução. Num browser, navegue para: `https://<your-site-name>.azurewebsites.net`

Deverá ver o site sem mensagens atualmente apresentadas.

[!INCLUDE [event-grid-register-provider-powershell.md](../../../includes/event-grid-register-provider-powershell.md)]

## <a name="subscribe-to-your-storage-account"></a>Subscrever a sua conta de armazenamento

Subscreva um tópico para comunicar ao Event Grid os eventos que pretende controlar. O exemplo seguinte subscreve à conta de armazenamento que criou e transmite o URL da sua aplicação web como o ponto final para notificação de eventos. O ponto final para a aplicação Web tem de incluir o sufixo `/api/updates/`.

```powershell
$storageId = (Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup -AccountName $storageName).Id
$endpoint="https://$sitename.azurewebsites.net/api/updates"

New-AzureRmEventGridSubscription `
  -EventSubscriptionName gridBlobQuickStart `
  -Endpoint $endpoint `
  -ResourceId $storageId
```

Verifique a aplicação Web novamente e repare que um evento de validação de subscrição foi enviado para a mesma. Selecione o ícone do olho para expandir os dados do evento. O Event Grid envia o evento de validação para que o ponto final possa verificar que pretende receber dados de eventos. A aplicação Web inclui código para validar a subscrição.

![Ver evento da subscrição](./media/storage-blob-event-quickstart-powershell/view-subscription-event.png)

## <a name="trigger-an-event-from-blob-storage"></a>Acionar um evento a partir do armazenamento de Blobs

Agora, vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Em primeiro lugar, vamos criar um contentor e um objeto. Em seguida, vamos carregar o objeto para o contentor.

```powershell
$containerName = "gridcontainer"
New-AzureStorageContainer -Name $containerName -Context $ctx

echo $null >> gridTestFile.txt

Set-AzureStorageBlobContent -File gridTestFile.txt -Container $containerName -Context $ctx -Blob gridTestFile.txt
```

Acionou o evento e o Event Grid enviou a mensagem para o ponto final que configurou ao subscrever. Verifique a aplicação Web para ver o evento que acabámos de enviar.

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
