---
title: Serviço de porta de entrada do Azure - métricas e registo | Documentos da Microsoft
description: Este artigo ajuda-o a compreender as diferentes métricas e registos de acesso que suporta o serviço de porta de entrada do Azure
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 643ae03a9350868b172d99b2af7ce23e34fedf47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998003"
---
# <a name="monitoring-metrics-for-front-door"></a>Métricas de monitorização para a porta de entrada

Ao utilizar o serviço de porta de entrada do Azure, pode monitorizar as métricas-chave para validar a configuração de porta de entrada, juntamente com a utilização, o estado de funcionamento e o desempenho ou o sua porta de entrada.

## <a name="metrics"></a>Métricas

As métricas são uma funcionalidade para determinados recursos do Azure, onde pode ver contadores de desempenho no portal. Para a porta da frente, as métricas seguintes estão disponíveis:

| Métrica | Nome a apresentar de métrica | Unidade | Dimensões | Descrição |
| --- | --- | --- | --- | --- |
| RequestCount | Contagem de pedidos | Contagem | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | O número de pedidos de cliente servido por porta de entrada.  |
| RequestSize | Tamanho do pedido | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | O número de bytes enviados como pedidos de clientes para a porta de entrada. |
| ResponseSize | Tamanho de resposta | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | O número de bytes enviados como respostas da porta de entrada para os clientes. |
| TotalLatency | Latência total | Milissegundos | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | O tempo calculado a partir do quando o pedido de cliente foi recebido por porta da frente até que o cliente confirmados o último byte de resposta de porta de entrada. |
| BackendRequestCount | Contagem de pedidos de back-end | Contagem | HttpStatus</br>HttpStatusGroup</br>Back-end | O número de pedidos enviados do porta de entrada para o back-ends. |
| BackendRequestLatency | Latência de pedido de back-end | Milissegundos | Back-end | O tempo calculado a partir do quando o pedido foi enviado pela porta de entrada para o back-end até que a porta da frente recebido o último byte de resposta de back-end. |
| BackendHealthPercentage | Percentagem de estado de funcionamento do back-end | Percentagem | Back-end</br>BackendPool | A percentagem de estado de funcionamento com êxito as sondas de porta de entrada para o back-ends. |
| WebApplicationFirewallRequestCount | Contagem de pedidos de Firewall de aplicação Web | Contagem | PolicyName</br>RuleName</br>Ação | O número de pedidos de clientes processados pela segurança de camada de aplicativo de porta de entrada. |


## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar uma porta de entrada](quickstart-create-front-door.md).
- Saiba mais [como funciona a porta da frente](front-door-routing-architecture.md).
