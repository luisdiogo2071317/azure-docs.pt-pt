---
title: incluir ficheiro
description: incluir ficheiro
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/12/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: 74732008b336dc1b95ec96e8550d218105973ca4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
A tabela seguinte lista as informações sobre a quota específico de mensagens do Service Bus. Para obter informações sobre preços e outras quotas para o Service Bus, consulte o [preços do Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) descrição geral.

| Nome de quota | Âmbito | Notas | Valor |
| --- | --- | --- | --- | --- |
| Número máximo de espaços de nomes básicos / padrão por subscrição do Azure |Espaço de Nomes |Os pedidos subsequentes para espaços de nomes básicos / padrão adicionais são rejeitados pelo portal. |100|
| Número máximo de espaços de nomes premium por subscrição do Azure |Espaço de Nomes |Os pedidos subsequentes para espaços de nomes premium adicionais são rejeitados pelo portal. |10 |
| Tamanho da fila/tópico |Entidade |Definido após a criação do tópico/fila. <br/><br/> Receber mensagens subsequentes são rejeitadas e uma exceção é recebida pelo código da chamada. |1, 2, 3, 4 ou 5 GB.<br /><br />Se [criação de partições](../articles/service-bus-messaging/service-bus-partitioning.md) está ativada, o tamanho máximo de fila/tópico é 80 GB. |
| Número de ligações simultâneas num espaço de nomes |Espaço de Nomes |Os pedidos subsequentes para ligações adicionais são rejeitados e uma exceção é recebida pelo código da chamada. As operações REST não contam para ligações de TCP em simultâneo. |NetMessaging: 1000<br /><br />AMQP: 5000 |
| Número de em simultâneo receber pedidos numa entidade tópico/fila/subscrição |Entidade |Subsequentes receber pedidos são rejeitados e uma exceção é recebida pelo código da chamada. Esta quota se aplica a combinado diversas em simultâneo receber operações em todas as subscrições de um tópico. |5.000 |
| Número de tópicos/filas por espaço de nomes de serviço |Espaço de Nomes |Os pedidos subsequentes para a criação de um novo tópico ou uma fila no espaço de nomes de serviço são rejeitados. Como resultado, se configurada por meio de [portal do Azure][Azure portal], é gerada uma mensagem de erro. Se a chamada da management API, uma exceção é recebida pelo código da chamada. |10,000<br /><br />O número total de tópicos e filas de um espaço de nomes de serviço tem de ser menor ou igual a 10.000.<br/>Isto não é aplicável a Premium como todas as entidades são divididos em partições. |
| Número de particionada tópicos por filas por espaço de nomes de serviço |Espaço de Nomes |Os pedidos subsequentes para a criação de um novo tópico particionado ou fila no espaço de nomes de serviço são rejeitados. Como resultado, se configurada por meio de [portal do Azure][Azure portal], é gerada uma mensagem de erro. Se a chamada da gestão de API, um **QuotaExceededException** exceção é recebida pelo código da chamada. |Escalões básicos e Standard - 100<br />[Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) -1000 (por unidade de mensagens)<br/><br />Cada fila particionada ou um tópico conta para a quota de 10 000 entidades por espaço de nomes. |
| Tamanho máximo de qualquer caminho de entidade mensagens: fila ou um tópico |Entidade |- |260 carateres |
| Tamanho máximo de quaisquer mensagens nome da entidade: regra de espaço de nomes, a subscrição ou a subscrição |Entidade |- |50 carateres |
| Tamanho da mensagem para uma entidade de fila/tópico/subscrição |Entidade |Mensagens a receber que excedem destas quotas são rejeitadas e uma exceção é recebida pelo código da chamada. |Tamanho máximo de mensagem: 256 KB ([escalão Standard](../articles/service-bus-messaging/service-bus-premium-messaging.md)) / 1 MB ([escalão Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />**Tenha em atenção** devido a sobrecarga de sistema, este limite é normalmente ligeiramente inferior.<br /><br />Tamanho máximo do cabeçalho: 64 KB<br /><br />Número máximo de propriedades de cabeçalho na matriz de propriedades: **byte/int. MaxValue**<br /><br />Tamanho máximo de propriedade de matriz de propriedades: sem limite explícito. Limitado pelo tamanho do cabeçalho máximo. |
| Tamanho de propriedade da mensagem para uma entidade de fila/tópico/subscrição |Entidade |A **SerializationException** exceção é gerada. |Tamanho de propriedade para cada propriedade da mensagem máximo é 32 k tamanho cumulativo de todas as propriedades não pode exceder 64 k Isto aplica-se o cabeçalho de todo o [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), que tem ambos as propriedades do utilizador, bem como as propriedades do sistema (tais como [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [etiqueta](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid), e assim sucessivamente). |
| Número de subscrições por tópico |Entidade |Os pedidos subsequentes para criar subscrições adicionais para o tópico são rejeitados. Como resultado, se configurado através do portal, é apresentada uma mensagem de erro. Se a chamada a partir da API de gestão uma exceção é recebida pelo código da chamada. |2,000 |
| Número de filtros do SQL Server por tópico |Entidade |Os pedidos subsequentes para a criação de filtros adicionais no tópico são rejeitados e uma exceção é recebida pelo código da chamada. |2,000 |
| Número de filtros de correlação por tópico |Entidade |Os pedidos subsequentes para a criação de filtros adicionais no tópico são rejeitados e uma exceção é recebida pelo código da chamada. |100,000 |
| Tamanho do SQL Server filtros/ações |Espaço de Nomes |Os pedidos subsequentes para a criação de filtros adicionais são rejeitados e uma exceção é recebida pelo código da chamada. |Comprimento máximo da cadeia de condição de filtro: 1024 (1K).<br /><br />Comprimento máximo da cadeia de ação de regra: 1024 (1K).<br /><br />Número máximo de expressões por ação de regra: 32. |
| Número de [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regras por espaço de nomes, fila ou um tópico |Entidade, espaço de nomes |Os pedidos subsequentes para criação de regras adicionais são rejeitados e uma exceção é recebida pelo código da chamada. |Número máximo de regras: 12. <br /><br /> Regras que são configuradas num espaço de nomes do Service Bus são aplicadas a todas as filas e tópicos, esse espaço de nomes. |
| Número de mensagens em fila por transação | Transação | Mensagens de entrada adicionais são rejeitadas e uma exceção a indicar "não é possível enviar mais do que 100 mensagens numa única transação" é recebida pelo código da chamada. | 100 <br /><br /> Para ambos **Send()** e **SendAsync()** operações. |

[Azure portal]: https://portal.azure.com