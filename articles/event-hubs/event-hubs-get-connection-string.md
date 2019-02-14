---
title: Obter cadeia de ligação - Event Hubs do Azure | Documentos da Microsoft
description: Este artigo fornece instruções para obter uma cadeia de ligação que os clientes podem utilizar para ligar ao Event Hubs do Azure.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: ee4bd5d2acf1a029486f83ee721b9e1f72347958
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238152"
---
# <a name="get-an-event-hubs-connection-string"></a>Obter uma cadeia de ligação dos Hubs de eventos

Para utilizar os Hubs de eventos, terá de criar um espaço de nomes de Hubs de eventos. Espaço de nomes é um contentor de âmbito que pode alojar vários Hubs de eventos / tópicos do Kafka. Este espaço de nomes fornece um exclusivo [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Depois de criar um espaço de nomes, pode obter a cadeia de ligação necessária para comunicar com os Hubs de eventos.

A cadeia de ligação para os Hubs de eventos do Azure tem os seguintes componentes incorporados dentro da mesma,

* FQDN = o FQDN do espaço de nomes EventHubs que criou (Isto irá incluir o nome de espaço de nomes de EventHubs seguido servicebus.windows.net)
* SharedAccessKeyName = o nome que escolheu para chaves SAS da sua aplicação
* SharedAccessKey = o valor gerado da chave.

O modelo de cadeia de ligação é semelhante a
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Como pode ser uma cadeia de ligação de exemplo `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Este artigo o orienta através de várias formas de obter a cadeia de ligação.

## <a name="get-connection-string-from-the-portal"></a>Obter cadeia de ligação a partir do portal

Assim que tiver o espaço de nomes de Hubs de eventos, a seção de visão geral do portal pode dar-lhe a cadeia de ligação conforme mostrado abaixo:

![Cadeia de ligação de Hubs de eventos](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)

Ao clicar no link de cadeia de ligação na secção Descrição geral, é aberto o separador de políticas SAS, conforme mostrado na figura abaixo:

![As políticas de Hubs de eventos SAS](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)

Pode adicionar uma nova política SAS e obter a cadeia de ligação ou utilizar a política predefinida que já foi criada para. Quando a política é aberta, a cadeia de ligação é obtida como mostra a abaixo figura:

![Os Hubs de eventos-obter cadeia de ligação](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Obter a cadeia de ligação com o Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Pode utilizar o Get-AzEventHubNamespaceKey para obter a cadeia de ligação para o nome de política/regra específica, conforme mostrado abaixo:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

Consulte a [módulo do PowerShell de Hubs de eventos do Azure](https://docs.microsoft.com/powershell/module/az.eventhub/get-azeventhubkey) para obter mais detalhes.

## <a name="getting-the-connection-string-with-azure-cli"></a>Obter a cadeia de ligação com a CLI do Azure
Pode utilizar o seguinte para obter a cadeia de ligação para o espaço de nomes:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Consulte a [CLI do Azure para os Hubs de eventos](https://docs.microsoft.com/cli/azure/eventhubs) para saber mais.

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Criar um Hub de Eventos](event-hubs-create.md)
