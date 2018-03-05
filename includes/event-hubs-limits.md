---
title: incluir ficheiro
description: incluir ficheiro
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: ab4c5b98ed9f6fcc8c271797db2d81dcc7ec4449
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
A tabela seguinte apresenta uma lista de quotas e limites específica [Event Hubs do Azure](https://azure.microsoft.com/services/event-hubs/). Para obter informações sobre os preços de Hubs de eventos, consulte [preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limite | Âmbito | Notas | Valor |
| --- | --- | --- | --- | --- |
| Número dos event hubs por espaço de nomes |Espaço de Nomes |Os pedidos subsequentes para a criação de um novo hub de eventos serão rejeitados. |10 |
| Número de partições por hub de eventos |Entidade |- |32 |
| Número de grupos de consumidores por hub de eventos |Entidade |- |20 |
| Número de ligações AMQP por espaço de nomes |Espaço de Nomes |Os pedidos subsequentes para ligações adicionais serão rejeitados e uma exceção é recebida pelo código da chamada. |5.000 |
| Tamanho máximo do evento de Event Hubs|Entidade |- |256 KB |
| Tamanho máximo de um nome de hub de eventos |Entidade |- |50 carateres |
| Número de não-época recetores por grupo de consumidores |Entidade |- |5 |
| Período de retenção máximo de dados de eventos |Entidade |- |1-7 dias |
| Unidades de débito máximas |Espaço de Nomes |Exceder o limite de unidade de débito faz com que os dados ser limitada e gera um  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Pode pedir um grande número de unidades de débito para um padrão camada pelo arquivo um [pedido de suporte](/azure/azure-supportability/how-to-create-azure-support-request). [Unidades de débito adicionais](../articles/event-hubs/event-hubs-auto-inflate.md) estão disponíveis em blocos de 20 numa base de compra consolidada. |20 |
| Número de regras de autorização por espaço de nomes |Espaço de Nomes|Os pedidos subsequentes para a criação de regra de autorização serão rejeitados.|12 |
