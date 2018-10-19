---
title: Criar um grupo de espaço de nomes e os consumidores de Event Hubs do Azure através de um modelo | Documentos da Microsoft
description: Criar um espaço de nomes de Hubs de eventos com um hub de eventos e um grupo de consumidores através de modelos do Azure Resource Manager
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: e3a7b2a7ad866fe6c70c638dc5203b9a31c6b5b3
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426639"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Criar um espaço de nomes de Hubs de eventos com o grupo de hub e o consumidor de eventos com um modelo Azure Resource Manager
Os Hubs de Eventos do Azure são uma plataforma de fluxo de dados altamente dimensionável e um serviço de ingestão capaz de receber e processar milhões de eventos por segundo. Este início rápido mostra como criar um hub de eventos com um modelo Azure Resource Manager.

Utilize um modelo Azure Resource Manager para criar um espaço de nomes do tipo [os Hubs de eventos](event-hubs-what-is-event-hubs.md), com um hub de eventos e um grupo de consumidores. O artigo mostra como definir quais recursos são implementados e como definir os parâmetros que são especificados quando a implementação é executada. Pode utilizar este modelo para as suas próprias implementações ou personalizá-lo para satisfazer as suas necessidades. Para obter informações sobre a criação de modelos, consulte [modelos Authoring Azure Resource Manager][Authoring Azure Resource Manager templates].

Para o modelo completo, consulte a [modelo de grupo de hub e o consumidor de eventos] [ Event Hub and consumer group template] no GitHub.

> [!NOTE]
> Para verificar os modelos mais recentes, visite a galeria [Modelos de Início Rápido do Azure][Azure Quickstart Templates] e procure Hubs de Eventos.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este início rápido, precisa de uma subscrição do Azure. Se não tiver uma, [criar uma conta gratuita] [] antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se estiver a utilizar **do Azure PowerShell** para implementar o modelo do Resource Manager localmente, tem de executar a versão mais recente do PowerShell para concluir este início rápido. Se precisar de instalar ou atualizar, veja [Instalar e Configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Se optar por instalar e utilizar **CLI do Azure** para implementar o modelo do Resource Manager localmente, este tutorial requer a execução da CLI do Azure versão 2.0.4 ou posterior. Execute `az --version` para verificar a sua versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli).

## <a name="what-will-you-deploy"></a>O que irá implementar?

Com este modelo, implementa um espaço de nomes de Hubs de eventos com um hub de eventos e um grupo de consumidores.

Para executar automaticamente a implementação, clique no seguinte botão:

[![Implementar no Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parâmetros

Com o Azure Resource Manager, define parâmetros para os valores que pretende especificar quando o modelo é implementado. O modelo inclui uma secção denominada `Parameters` que contém todos os valores de parâmetro. Deve definir um parâmetro para valores que irá variar com base no projeto que está a implementar ou com base no ambiente que pretende implementar. Não defina parâmetros para valores que permanecem sempre iguais. Cada valor de parâmetro no modelo define os recursos que são implementados.

O modelo define os seguintes parâmetros:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

O nome do espaço de nomes dos Hubs de Eventos que serão criados.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

O nome do hub de eventos no espaço de nomes dos Hubs de Eventos.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

O nome do grupo de consumidores criado para o hub de eventos.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

A versão da API do modelo.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Recursos a implementar

Cria um espaço de nomes do tipo **EventHubs**, com um hub de eventos e um grupo de consumidores:

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Comandos para executar a implementação

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```
### <a name="create-a-storage-account-for-event-processor-host"></a>Criar uma conta de armazenamento para o Anfitrião do Processador de Eventos

O Anfitrião do Processador de Eventos simplifica a receção de eventos provenientes dos Hubs de Eventos ao gerir pontos de verificação e receções em paralelo. Para o ponto de verificação, o Anfitrião do Processador de Eventos precisa de uma conta de armazenamento. Para criar uma conta de armazenamento e obter as chaves, execute os seguintes comandos:

```azurepowershell-interactive
# Create a standard general purpose storage account 
New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name storage_account_name -Location eastus -SkuName Standard_LRS 
e
# Retrieve the storage account key for accessing it
Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name storage_account_name
```

É precisa uma cadeia de ligação para ligar ao seu hub de eventos e processar eventos. Para obter a cadeia de ligação, execute:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```


## <a name="azure-cli"></a>CLI do Azure

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

### <a name="create-a-storage-account-for-event-processor-host"></a>Criar uma conta de armazenamento para o Anfitrião do Processador de Eventos
```azurecli-interactive
# Create a general purpose standard storage account for Event Processor Host
az storage account create --name storageAccountName --resource-group myResourceGroup --location eastus2 --sku Standard_RAGRS --encryption blob

# List the storage account access keys
az storage account keys list --resource-group myResourceGroup --account-name storageAccountName

# Get namespace connection string
az eventhubs namespace authorization-rule keys list --resource-group myResourceGroup --namespace-name namespaceName --name RootManageSharedAccessKey
```

Copie e cole a cadeia de ligação numa localização temporária, como o Bloco de Notas, para utilizar mais tarde.

## <a name="stream-into-event-hubs"></a>Transmitir em fluxo para os Hubs de Eventos

Agora pode começar a transmissão em fluxo para os seus Hubs de Eventos. Os exemplos podem ser transferidos ou clonados do Git a partir do [repositório de Hubs de Eventos](https://github.com/Azure/azure-event-hubs)

### <a name="ingest-events"></a>Ingerir eventos

Para começar a transmitir em fluxo eventos, transfira o [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) do GitHub ou clone o [repositório do GitHub dos Hubs de Eventos](https://github.com/Azure/azure-event-hubs), ao emitir o comando seguinte:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navegue até à pasta \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleSender e carregue o ficheiro SampleSender.sln para o Visual Studio.

Em seguida, adicione o pacote Nuget [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ao projeto.

No ficheiro Program.cs, substitua os marcadores de posição seguintes pelo seu nome do hub de eventos e cadeia de ligação:

```C#
private const string EhConnectionString = "Event Hubs connection string";
private const string EhEntityPath = "Event Hub name";

```

Agora, crie e execute o exemplo. Pode ver os eventos que estão a ser ingeridos no seu hub de eventos:

![][3]

### <a name="receive-and-process-events"></a>Receber e processar eventos

Transfira agora o exemplo do recetor do Anfitrião do Processador de Eventos, que recebe as mensagens que acabou de enviar. Transfira o [SampleEphReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) do GitHub ou clone o [repositório do GitHub dos Hubs de Eventos](https://github.com/Azure/azure-event-hubs), ao emitir o comando seguinte:

```bash
git clone https://github.com/Azure/azure-event-hubs.git
```

Navegue até à pasta \azure-event-hubs\samples\DotNet\Microsoft.Azure.EventHubs\SampleEphReceiver e carregue o ficheiro de solução SampleEphReceiver.sln para o Visual Studio.

Em seguida, adicione os pacotes NuGet [Microsoft.Azure.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) e [Microsoft.Azure.EventHubs.Processor](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) ao projeto.

No ficheiro Program.cs, substitua as seguintes constantes pelos valores correspondentes:

```C#
private const string EventHubConnectionString = "Event Hubs connection string";
private const string EventHubName = "Event Hub name";
private const string StorageContainerName = "Storage account container name";
private const string StorageAccountName = "Storage account name";
private const string StorageAccountKey = "Storage account key";
```

Agora, crie e execute o exemplo. Pode ver os eventos que são recebidos no seu exemplo de aplicação:

![][4]

No portal do Azure, pode ver a taxa em que os eventos são processados para um determinado espaço de nomes de Hubs de Eventos, conforme mostrado:

![][5]


## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou o espaço de nomes de Hubs de eventos e utilizado aplicativos de exemplo para enviar e receber eventos a partir do seu hub de eventos. Para obter instruções passo a passo enviar eventos para (ou) receber eventos de um hub de eventos, consulte os seguintes tutoriais: 

1. **Enviar eventos para um hub de eventos**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [node. js ](event-hubs-node-get-started-send.md), [Vá](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
2. **Receber eventos de um hub de eventos**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [ NODE. js](event-hubs-node-get-started-receive.md), [vá](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
