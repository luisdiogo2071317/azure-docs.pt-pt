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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756122"
---
A tabela seguinte apresenta uma lista de quotas e limites específica [os Hubs de eventos do Azure](https://azure.microsoft.com/services/event-hubs/). Para obter informações sobre os preços de Hubs de eventos, consulte [preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limite | Âmbito | Notas | Valor |
| --- | --- | --- | --- | --- |
| Número de hubs de eventos por espaço de nomes |Espaço de Nomes |Os pedidos subsequentes para a criação de um novo hub de eventos serão rejeitados. |10 |
| Número de partições por hub de eventos |Entidade |- |32 |
| Número de grupos de consumidores por hub de eventos |Entidade |- |20 |
| Número de ligações AMQP por espaço de nomes |Espaço de Nomes |Pedidos subsequentes para ligações adicionais serão rejeitados e uma exceção é recebida pelo código de chamada. |5.000 |
| Tamanho máximo de eventos de Hubs de eventos|Entidade |- |256 KB |
| Tamanho máximo de um nome de hub de eventos |Entidade |- |50 carateres |
| Número de recetores de não "Epoch" por grupo de consumidores |Entidade |- |5 |
| Período máximo de retenção de dados de eventos |Entidade |- |1 a 7 dias |
| Unidades de débito máximas |Espaço de Nomes |Excede o limite de unidade de débito faz com que seus dados para ser otimizados e gera um  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Pode pedir um número maior de unidades de débito para um padrão de camada ao arquivamento uma [pedido de suporte](/azure/azure-supportability/how-to-create-azure-support-request). [Unidades de débito adicionais](../articles/event-hubs/event-hubs-auto-inflate.md) estão disponíveis em blocos de 20 de forma de compra. |20 |
| Número de regras de autorização por espaço de nomes |Espaço de Nomes|Pedidos subsequentes para criação de regras de autorização serão rejeitados.|12 |
