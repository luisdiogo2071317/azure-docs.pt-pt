---
title: Início Rápido do Azure - processar fluxos de eventos com o PowerShell | Microsoft Docs
description: Este início rápido descreve como enviar e receber eventos dos Hubs de Eventos do Azure com o PowerShell e uma aplicação .NET de exemplo.
services: event-hubs
author: sethmanheim
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/26/2018
ms.author: sethm
ms.openlocfilehash: 9216372038db7a6f97cfc8034f715b34de08d83c
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132449"
---
# <a name="quickstart-process-event-streams-using-powershell-and-net-standard"></a>Início Rápido: processar fluxos de eventos com o PowerShell e o .NET Standard

Os Hubs de Eventos do Azure são uma plataforma de fluxo de dados altamente dimensionável e um serviço de ingestão capaz de receber e processar milhões de eventos por segundo. Este início rápido mostra como criar um hub de eventos com o Azure PowerShell e enviar e receber a partir de um hub de eventos com o SDK .NET Standard.

Para concluir este início rápido, precisa de uma subscrição do Azure. Se não tiver uma, [crie uma conta gratuita][] antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que tem:

- [Visual Studio 2017 Atualização 3 (versão 15.3, 26730.01)](http://www.visualstudio.com/vs) ou posterior.
- [SDK .NET Standard](https://www.microsoft.com/net/download/windows), versão 2.0 ou posterior.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se estiver a utilizar o PowerShell localmente, tem de executar a versão mais recente do PowerShell para concluir este início rápido. Se precisar de instalar ou atualizar, veja [Instalar e Configurar o Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="provision-resources"></a>Recursos de aprovisionamento

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é uma coleção lógica dos recursos do Azure e precisa de um grupo de recursos para criar um hub de eventos. 

O exemplo seguinte cria um grupo de recursos na região E.U.A Leste. Substitua `myResourceGroup` pelo nome do grupo de recursos que pretende utilizar:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

### <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes dos Hubs de Eventos

Assim que o seu grupo de recursos é feito, crie um espaço de nomes dos Hubs de Eventos dentro desse grupo de recursos. Um espaço de nomes dos Hubs de Eventos oferece um nome de domínio completamente qualificado e exclusivo, no qual pode criar o seu hub de eventos. Substitua `namespace_name` por um nome exclusivo para o seu espaço de nomes:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

### <a name="create-an-event-hub"></a>Criar um hub de eventos

Agora que tem um espaço de nomes dos Hubs de Eventos, crie um hub de eventos nesse espaço de nomes:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
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

### <a name="get-the-connection-string"></a>Obter a cadeia de ligação

É precisa uma cadeia de ligação para ligar ao seu hub de eventos e processar eventos. Para obter a cadeia de ligação, execute:

```azurepowershell-interactive
Get-AzureRmEventHubKey -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Name RootManageSharedAccessKey
```

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

## <a name="clean-up-resources"></a>Limpar recursos

Quando concluir este início rápido, pode eliminar o seu grupo de recursos e o espaço de nomes, a conta de armazenamento e os hubs de eventos dentro do mesmo. Substitua `myResourceGroup` pelo nome do grupo de recursos que criou. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste artigo, criou o espaço de nomes dos Hubs de Eventos e outros recursos necessários para enviar e receber eventos do hub de eventos. Para obter mais informações, avance para o próximo tutorial:

> [!div class="nextstepaction"]
> [Visualizar anomalias de dados nas transmissões de dados dos Hubs de Eventos](event-hubs-tutorial-visualize-anomalies.md)

[crie uma conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
