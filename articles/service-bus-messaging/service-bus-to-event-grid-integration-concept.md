---
title: "Descrição Gera de integração do Azure Service Bus para o Event Grid | Microsoft Docs"
description: "Descrição de mensagens do Service Bus e integração do Event Grid"
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: 17f6c107ea1adfd4463ff4cc205fe11d111acb84
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Descrição Geral de integração do Azure Service Bus para o Azure Event Grid

O Azure Service Bus lançou uma nova integração ao Azure Event Grid. O cenário principal desta funcionalidade é as Filas ou Subscrições do Service Bus que têm um volume baixo de mensagens, não precisarem de ter constantemente uma consulta de recetor para mensagens. O Service Bus agora pode emitir eventos para o Azure Event Grid quando existem mensagens numa Fila ou Subscrição quando não existem recetores. Pode criar subscrições do Azure Event Grid para os seus espaços de nomes do Service Bus e escutam estes eventos e reagem aos eventos ao iniciar um recetor. Com esta funcionalidade, o Service Bus pode ser utilizado em modelos de programação reativa.

Para ativar a funcionalidade, precisa do seguinte:

* Um espaço de nomes Premium do Azure Service Bus com, pelo menos, uma Fila do Service Bus ou um Tópico do Service Bus com, pelo menos, uma Subscrição.
* Acesso do contribuinte ao Espaço de Nomes do Azure Service Bus.
* Além disso, precisa de uma subscrição do Azure Event Grid para o Espaço de Nomes do Service Bus. Esta subscrição recebe a notificação do Azure Event Grid indicando que existem mensagens a ser escolhidas. Os subscritores típicos podem ser Logic Apps, Funções do Azure ou um Web Hook que contacta uma Aplicação Web que depois processa as mensagens. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Verifique se tem acesso de contribuinte

Navegue para o Espaço de Nomes do Service Bus e selecione "Controlo de acesso (IAM)", conforme mostrado abaixo:

![1][]

### <a name="events-and-event-schemas"></a>Eventos e Esquemas de Eventos

O Azure Service Bus envia hoje eventos para dois cenários.

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Além disso, utiliza a Segurança do Azure Event Grid standard e [mecanismos de autenticação](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication).

Para obter mais detalhes sobre Esquemas de Eventos do Event Grid, siga [esta](https://docs.microsoft.com/en-us/azure/event-grid/event-schema) ligação.

#### <a name="active-messages-available-event"></a>Evento Disponível de Mensagens Ativas

Este Evento é gerado se tiver mensagens ativas numa Fila ou Subscrição, sem recetores à escuta.

O esquema para este Evento é o seguinte:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>Evento Disponível de Mensagens não entregues

Obtém, pelo menos, um evento por Fila de Mensagens Não Entregues, que tem mensagens e recetores não ativos.

O esquema para este Evento é o seguinte:

```JSON
[{
     "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Qual a frequência e o número de eventos emitidos?

Se tiver várias Filas e Tópicos/Subscrições no espaço de nomes, obtém, pelo menos, um evento por Fila e um por Subscrição. Os eventos são emitidos imediatamente se não existirem mensagens na entidade do Service Bus e uma nova mensagem chegar, ou a cada dois minutos, a menos que o Azure Service Bus detete um recetor ativo. A procura de mensagem não interrompe os eventos.

Por predefinição, o Azure Service Bus emite eventos para todas as entidades no espaço de nomes. Se pretender receber eventos apenas para entidades específicas, veja a seguinte secção de filtragem.

### <a name="filtering-limiting-from-where-you-get-events"></a>Filtragem e limitação de onde recebe eventos

Se pretender receber eventos apenas para, por exemplo, uma Fila ou uma Subscrição no seu espaço de nomes, pode utilizar os filtros "Começa com" ou "Termina com" fornecidos pelo Azure Event Grid. Em algumas interfaces, os filtros são denominados "Pre" e "Sufixo". Se quiser receber eventos para várias Filas e Subscrições, mas não todas, pode criar várias Subscrições do Azure Event Grid diferentes e fornecer um filtro para cada.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Como criar Subscrições do Azure Event Grid para Espaços de Nomes do Service Bus

Existem três formas diferentes de criar Subscrições do Event Grid para Espaços de Nomes do Service Bus.

* [O portal do Azure](#portal-instructions)
* [CLI do Azure](#azure-cli-instructions)
* [PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Instruções do portal

Para criar uma nova subscrição do Azure Event Grid, navegue para o seu espaço de nomes no portal do Azure e selecione o painel Event Grid. Clicar em "+ Subscrição de Evento" abaixo, mostra um espaço de nomes que já tem algumas subscrições do Event Grid.

![20][]

A captura de ecrã seguinte mostra um exemplo de como subscrever uma Função do Azure ou um Hook de Web sem filtragem específica:

![21][]

## <a name="azure-cli-instructions"></a>Instruções da CLI do Azure

Primeiro, certifique-se de que tem, pelo menos, a versão da CLI do Azure 2.0 instalada. Pode transferir o instalador aqui. Em seguida, prima "Windows + X" e abra uma nova consola do PowerShell com permissões de Administrador. Em alternativa, também pode utilizar um shell de comandos no portal do Azure.

Execute o seguinte código:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>Instruções do PowerShell

Confirme que instalou o Azure PowerShell. Pode encontrá-lo aqui. Em seguida, prima "Windows + X" e abra uma nova consola do PowerShell com permissões de Administrador. Em alternativa, também pode utilizar um shell de comandos no portal do Azure.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

A partir de aqui, pode explorar as outras opções de configuração ou [testar se os eventos estão a fluir](#test-that-events-are-flowing).

## <a name="next-steps"></a>Passos seguintes

* [Exemplos](service-bus-to-event-grid-integration-example.md) do Service Bus e do Event Grid.
* Saiba mais sobre o [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Saiba mais sobre as [Funções do Azure](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Saiba mais sobre o [Azure Logic Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Saiba mais sobre o [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/).

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png