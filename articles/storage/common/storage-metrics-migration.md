---
title: Migração de métricas de armazenamento do Azure | Microsoft Docs
description: Saiba mais sobre como migrar métricas legadas para a nova métrica gerida pelo Monitor do Azure.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 03/30/2018
ms.author: fryu
ms.openlocfilehash: c4dc9b231668315af16c625314c737fee99d672d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="azure-storage-metrics-migration"></a>Migração de métricas de armazenamento do Azure

Alinhada com a estratégia de experiência de Monitor unifying no Azure, do armazenamento do Azure no-quadros métricas à plataforma de Monitor do Azure. No futuro, o serviço de métricas legados será terminado com o aviso antecipado com base na política do Azure. Se dependem as métricas do storage legado, terá de migrar antes da data de fim de serviço para manter as informações de métricas.

Este artigo fornece orientações sobre como migrar a partir de métricas legadas para a nova métrica.

## <a name="understand-legacy-metrics-managed-by-azure-storage"></a>Compreender as métricas legadas geridas pelo armazenamento do Azure

Armazenamento do Azure recolhe valores métricos legados, agrega e armazena-os em tabelas de $Metric dentro da mesma conta de armazenamento. Pode utilizar o portal do Azure para configurar a monitorização do gráfico e também pode utilizar os SDKs de armazenamento do Azure para ler os dados das tabelas de $Metric com base no esquema. Para obter mais detalhes, pode consultar este [análise de armazenamento](./storage-analytics.md).

Métricas de legado fornecem as métricas de capacidade no Blob apenas e métricas de transação no Blob, tabela, ficheiros e fila.

Métricas de legado foram concebidas no esquema simples. A estrutura resulta num valor métrico de zero quando não tiver os padrões de tráfego acionar a métrica. Por exemplo, **ServerTimeoutError** está definido para 0 nas tabelas de $Metric, mesmo quando não receber quaisquer erros de tempo limite do servidor do tráfego em direto para a conta de armazenamento.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Compreender a nova métrica gerida pelo Monitor do Azure

Novo armazenamento com base nas métricas, o Storage do Azure emite os dados métricos ao back-end de Monitor do Azure. Monitor do Azure fornece uma experiência de monitorização unificada, incluindo dados a partir do portal, bem como a ingestão de dados. Para obter mais detalhes, pode consultar este [artigo](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Nova métrica fornece as métricas de capacidade e métricas de transação no Blob, tabela, ficheiro, fila e armazenamento premium.

Multi dimensão é uma das funcionalidades fornecidas pelo Monitor do Azure. Armazenamento do Azure adopts a estrutura no novo esquema de métrica de definir. Para dimensões suportadas nas métricas, pode encontrar os detalhes deste [métricas do Storage do Azure no Monitor de Azure](./storage-metrics-in-azure-monitor.md). Estrutura de dimensão multi fornece eficiência de custos em ambos os largura de banda da ingestão e a capacidade de armazenar métricas. Por conseguinte, se o tráfego não tem acionadas métricas relacionadas, os dados relacionados de métricos não serão gerados. Por exemplo, se o tráfego não tem acionadas quaisquer erros de tempo limite do servidor, Azure Monitor devolve sem dados ao consultar o valor da métrica **transações** com dimensão **ResponseType** igual a **ServerTimeoutError**.

## <a name="metrics-mapping-between-legacy-metrics-and-new-metrics"></a>Mapeamento de métricas entre as métricas de legado e métricas de novo

Se ler dados métricos programaticamente, tem de adotar o novo esquema de métrico no seus programas. Para melhor compreender as alterações, pode consultar o mapeamento listados a tabela seguinte:

**Métricas de capacidade**

| Métrica legada | Métrica de novo |
| ------------------- | ----------------- |
| Capacidade            | BlobCapacity com dimensão BlobType igual a BlockBlob ou PageBlob |
| ObjectCount         | BlobCount com dimensão BlobType igual a BlockBlob ou PageBlob |
| ContainerCount      | ContainerCount |

As métricas seguintes são novas ofertas de métricas legadas não suportam:
* TableCapacity
* TableCount
* TableEntityCount
* QueueCapacity
* QueueCount
* QueueMessageCount
* FileCapacity
* FileCount
* FileShareCount
* UsedCapacity

**Métricas de transação**

| Métrica legada | Métrica de novo |
| ------------------- | ----------------- |
| AnonymousAuthorizationError | Transações com dimensão ResponseType igual a AuthorizationError |
| AnonymousClientOtherError | Transações com dimensão ResponseType igual a ClientOtherError |
| AnonymousClientTimeoutError | Transações com dimensão ResponseType igual a ClientTimeoutError |
| AnonymousNetworkError | Transações com dimensão ResponseType igual a NetworkError |
| AnonymousServerOtherError | Transações com dimensão ResponseType igual a ServerOtherError |
| AnonymousServerTimeoutError | Transações com dimensão ResponseType igual a ServerTimeoutError |
| AnonymousSuccess | Transações com dimensão ResponseType igual para êxito |
| AnonymousThrottlingError | Transações com dimensão ResponseType igual a ClientThrottlingError ou ServerBusyError |
| AuthorizationError | Transações com dimensão ResponseType igual a AuthorizationError |
| Disponibilidade | Disponibilidade |
| AverageE2ELatency | SuccessE2ELatency |
| AverageServerLatency | SuccessServerLatency |
| ClientOtherError | Transações com dimensão ResponseType igual a ClientOtherError |
| ClientTimeoutError | Transações com dimensão ResponseType igual a ClientTimeoutError |
| NetworkError | Transações com dimensão ResponseType igual a NetworkError |
| PercentAuthorizationError | Transações com dimensão ResponseType igual a AuthorizationError |
| PercentClientOtherError | Transações com dimensão ResponseType igual a ClientOtherError |
| PercentNetworkError | Transações com dimensão ResponseType igual a NetworkError |
| PercentServerOtherError | Transações com dimensão ResponseType igual a ServerOtherError |
| PercentSuccess | Transações com dimensão ResponseType igual para êxito |
| PercentThrottlingError | Transações com dimensão ResponseType igual a ClientThrottlingError ou ServerBusyError |
| PercentTimeoutError | Transações com dimensão ResponseType igual a ServerTimeoutError ou igual a ClientTimeoutError ResponseType|
| SASAuthorizationError | Transações com dimensão ResponseType igual a AuthorizationError |
| SASClientOtherError | Transações com dimensão ResponseType igual a ClientOtherError |
| SASClientTimeoutError | Transações com dimensão ResponseType igual a ClientTimeoutError |
| SASNetworkError | Transações com dimensão ResponseType igual a NetworkError |
| SASServerOtherError | Transações com dimensão ResponseType igual a ServerOtherError |
| SASServerTimeoutError | Transações com dimensão ResponseType igual a ServerTimeoutError |
| SASSuccess | Transações com dimensão ResponseType igual para êxito |
| SASThrottlingError | Transações com dimensão ResponseType igual a ClientThrottlingError ou ServerBusyError |
| ServerOtherError | Transações com dimensão ResponseType igual a ServerOtherError |
| ServerTimeoutError | Transações com dimensão ResponseType igual a ServerTimeoutError |
| Êxito | Transações com dimensão ResponseType igual para êxito |
| ThrottlingError | Transações com dimensão ResponseType igual a ClientThrottlingError ou ServerBusyError |
| TotalBillableRequests | Transações |
| TotalEgress | Saída |
| TotalIngress | Entrada |
| TotalRequests | Transações |

## <a name="faq"></a>FAQ

* Como posso migrar a regras de alerta existentes?

R: Se tiver criado o alerta clássica regras com base nas métricas do armazenamento legado, terá de criar regras com base no novo esquema de métrica de alerta de novo.

* Irá novos dados métricos armazenados na mesma conta de armazenamento por predefinição?

R: um número de Se precisar de arquivar os dados métricos à conta de armazenamento, pode utilizar [definição de diagnóstico no Monitor do Azure](https://azure.microsoft.com/blog/azure-monitor-multiple-diagnostic-settings/)

## <a name="next-steps"></a>Passos Seguintes

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Métricas do Storage no Monitor do Azure](./storage-metrics-in-azure-monitor.md)
