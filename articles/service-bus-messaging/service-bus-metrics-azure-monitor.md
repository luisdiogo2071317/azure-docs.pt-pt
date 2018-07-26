---
title: Métricas de barramento de serviço do Azure no Azure Monitor (pré-visualização) | Documentos da Microsoft
description: Utilizar a monitorização do Azure para monitorizar as entidades do Service Bus
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 05/31/2018
ms.author: sethm
ms.openlocfilehash: 517c1376415466f5ce0e3c854f6c91f60cdab9f8
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247628"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Métricas de barramento de serviço do Azure no Azure Monitor (pré-visualização)

Métricas de barramento de serviço dá-lhe o estado dos recursos na sua subscrição do Azure. Com um vasto conjunto de dados de métricas, pode avaliar o estado de funcionamento geral dos seus recursos do Service Bus, não apenas ao nível do espaço de nomes, mas também no nível de entidade. Essas estatísticas podem ser importantes como eles ajudam-na monitorizar o estado do Service Bus. Métricas também podem ajudar a resolver problemas de causa raiz sem a necessidade de contactar o suporte do Azure.

O Azure Monitor fornece interfaces do usuário unificada para monitoramento em vários serviços do Azure. Para obter mais informações, consulte [monitorização no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) e o [métricas de obter o Azure Monitor com o .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) exemplo no GitHub.

## <a name="access-metrics"></a>Métricas de acesso

O Azure Monitor proporciona várias formas de métricas de acesso. Pode qualquer uma das métricas de acesso através da [portal do Azure](https://portal.azure.com), ou utilize o Azure Monitor APIs (REST e .NET) e soluções de análise de como o Log Analytics e Hubs de eventos. Para obter mais informações, consulte [métricas do Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

As métricas estão ativadas por predefinição e pode aceder os mais recentes 30 dias de dados. Se precisar de reter dados durante um período mais longo do tempo, pode arquivar dados de métricas para uma conta de armazenamento do Azure. Este é configurado no [das definições de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) no Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Métricas de acesso no portal

Pode monitorizar as métricas ao longo do tempo na [portal do Azure](https://portal.azure.com). O exemplo seguinte mostra como ver pedidos com êxito e solicitações de entrada ao nível da conta:

![][1]

Também pode aceder a métricas diretamente através do espaço de nomes. Para tal, selecione o seu espaço de nomes e, em seguida, clique em **métricas (Peview)**. Para apresentar métricas filtradas para o âmbito da entidade, selecione a entidade e, em seguida, clique em **métricas (pré-visualização)**.

![][2]

Para as métricas que suporta dimensões, tem de filtrar com o valor de dimensão pretendida.

## <a name="billing"></a>Faturação

Uso de métricas no Azure Monitor é gratuita em pré-visualização. No entanto, se utilizar soluções adicionais que ingerir dados de métricas, pode ser cobrado por estas soluções. Por exemplo, é cobrado pelo armazenamento do Azure se arquivar dados de métricas para uma conta de armazenamento do Azure. Também são cobradas pelo Log Analytics se os transmitir dados de métricas para o Log Analytics para análise avançada.

As métricas seguintes dão-lhe uma descrição geral do Estado de funcionamento do seu serviço. 

> [!NOTE]
> Podemos são descontinuar várias métricas, como eles são movidos sob um nome diferente. Isto pode requerer a atualização de suas referências. Métricas marcadas com a palavra-chave "preterido" não serão suportadas no futuro.

Todos os valores de métricas são enviados para o Azure Monitor, a cada minuto. A granularidade de tempo define o intervalo de tempo para o qual os valores das métricas são apresentados. O intervalo de tempo suportado para todas as métricas do Service Bus é de 1 minuto.

## <a name="request-metrics"></a>Métricas de pedidos

Conta o número de pedidos de operações de gestão e de dados.

| Nome da Métrica | Descrição |
| ------------------- | ----------------- |
| Pedidos de entrada (pré-visualização) | O número de pedidos efetuados para o serviço do Service Bus num determinado período. <br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Pedidos com êxito (pré-visualização)|O número de pedidos com êxito efetuados para o serviço do Service Bus num determinado período.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Erros de servidor (pré-visualização)|O número de pedidos não processadas devido a um erro no serviço do Service Bus num determinado período.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Erros de utilizador (pré-visualizar - veja a subsecção seguinte)|O número de pedidos não processadas devido a erros de utilizador num determinado período.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Pedidos limitados (pré-visualização)|O número de pedidos que foram limitado porque a utilização foi excedida.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|

### <a name="user-errors"></a>Erros de utilizador

Os seguintes dois tipos de erros são classificados como erros de utilizador:

1. Erros de lado do cliente (em HTTP que seriam 400 erros).
2. Erros que ocorrem durante o processamento das mensagens, tal como [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>Métricas de mensagem

| Nome da Métrica | Descrição |
| ------------------- | ----------------- |
|Mensagens de entrada (pré-visualização)|O número de eventos ou as mensagens enviadas para o Service Bus num determinado período.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Mensagens de saída (pré-visualização)|O número de eventos ou mensagens recebidas do Service Bus num determinado período.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|

## <a name="connection-metrics"></a>Métricas de ligação

| Nome da Métrica | Descrição |
| ------------------- | ----------------- |
|ActiveConnections (pré-visualização)|O número de ligações ativas num espaço de nomes, bem como numa entidade.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Ligações aberto (pré-visualização)|O número de conexões abertas.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName|
|Ligações fechado (pré-visualização)|O número de conexões encerradas.<br/><br/> Unidade: contagem <br/> Tipo de agregação: Total <br/> Dimensão: EntityName |

## <a name="resource-usage-metrics"></a>Métricas de utilização de recursos

| Nome da Métrica | Descrição |
| ------------------- | ----------------- |
|Utilização da CPU por espaço de nomes (pré-visualização)|A percentagem de utilização da CPU do espaço de nomes.<br/><br/> Unidade: por cento <br/> Tipo de agregação: máximo <br/> Dimensão: EntityName|
|Utilização de tamanho de memória por espaço de nomes (pré-visualização)|A utilização de memória de percentagem do espaço de nomes.<br/><br/> Unidade: por cento <br/> Tipo de agregação: máximo <br/> Dimensão: EntityName|

## <a name="metrics-dimensions"></a>Dimensões de métricas

O Azure Service Bus suporta as seguintes dimensões de métricas no Azure Monitor. Adicionar dimensões para as suas métricas é opcional. Se não adicionar dimensões, as métricas são especificadas no nível do espaço de nomes. 

|Nome da dimensão|Descrição|
| ------------------- | ----------------- |
|EntityName| Service Bus suporta entidades de mensagens sob o espaço de nomes.|

## <a name="next-steps"></a>Passos Seguintes

Consulte a [descrição geral da monitorização do Azure](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


