---
title: Migração de métricas de armazenamento do Azure | Microsoft Docs
description: Saiba como migrar métricas antigas para o novo métricas que são geridas pelo Monitor do Azure.
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
ms.openlocfilehash: c64061aee94e8c08a3f6bcae78cffca0b4172d97
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650677"
---
# <a name="azure-storage-metrics-migration"></a>Migração de métricas de armazenamento do Azure

Alinhada com a estratégia de unificá a experiência de monitor no Azure, o Storage do Azure integra métricas da plataforma de Monitor do Azure. No futuro, o serviço das métricas antigos vai terminar com uma notificação antecipada com base na política do Azure. Se baseiam-se nas métricas do armazenamento antigo, terá de migrar antes da data de fim de serviço para manter as informações de métricas.

Este artigo mostra como migrar das métricas antigas para a nova métrica.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Compreender antigas métricas que são geridas pelo armazenamento do Azure

Armazenamento do Azure recolhe valores métricos antigos e agrega e armazena-os em tabelas de $Metric dentro da mesma conta de armazenamento. Pode utilizar o portal do Azure para configurar um gráfico de monitorização. Também pode utilizar os SDKs de armazenamento do Azure para ler os dados de tabelas de $Metric que se baseiam o esquema. Para obter mais informações, consulte [análise de armazenamento](./storage-analytics.md).

Métricas antigas fornecem as métricas de capacidade apenas no Blob storage do Azure. Métricas antigas fornecem as métricas de transação no armazenamento de BLOBs, armazenamento de tabelas, ficheiros do Azure e o armazenamento de filas. 

Métricas antigas são concebidas de modo a um esquema simples. A estrutura resulta num valor métrico de zero quando não tiver os padrões de tráfego acionar a métrica. Por exemplo, o **ServerTimeoutError** valor é definido como 0 nas tabelas de $Metric, mesmo quando não receber quaisquer erros de tempo limite do servidor do tráfego em direto para uma conta de armazenamento.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Compreender a nova métrica gerida pelo Monitor do Azure

Novo armazenamento com base nas métricas, o Storage do Azure emite os dados métricos para o Monitor do Azure de back-end. Monitor do Azure fornece uma experiência de monitorização unificada, incluindo dados a partir do portal, bem como a ingestão de dados. Para obter mais detalhes, pode consultar este [artigo](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Nova métrica fornece as métricas de capacidade e métricas de transação no Blob, tabela, ficheiro, fila e armazenamento premium.

Multi dimensão é uma das funcionalidades que fornece a monitorização do Azure. Armazenamento do Azure adopts a estrutura no novo esquema de métrica de definir. Para dimensões suportadas nas métricas, pode encontrar os detalhes na [métricas do Storage do Azure no Monitor de Azure](./storage-metrics-in-azure-monitor.md). Estrutura de dimensão multi fornece eficiência de custos em ambos os largura de banda da ingestão e a capacidade de armazenar métricas. Por conseguinte, se o tráfego não tem acionadas métricas relacionadas, os dados relacionados de métricos não serão gerados. Por exemplo, se o tráfego não tem acionadas quaisquer erros de tempo limite do servidor, do Azure Monitor não devolve quaisquer dados ao consultar o valor da métrica **transações** com dimensão **ResponseType** igual a **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Mapeamento de métricas entre métricas antigas e nova métrica

Se ler dados métricos programaticamente, tem de adotar o novo esquema de métrico no seus programas. Para melhor compreender as alterações, pode consultar o mapeamento listado na seguinte tabela:

**Métricas de capacidade**

| Métrica antiga | Métrica de novo |
| ------------------- | ----------------- |
| **Capacidade**            | **BlobCapacity** com a dimensão **BlobType** igual a **BlockBlob** ou **PageBlob** |
| **ObjectCount**        | **BlobCount** com a dimensão **BlobType** igual a **BlockBlob** ou **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

As métricas seguintes são novas ofertas de que não suportam as métricas antigas:
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**Métricas de transação**

| Métrica antiga | Métrica de novo |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | As transações com a dimensão **ResponseType** igual a **AuthorizationError** |
| **AnonymousClientOtherError** | As transações com a dimensão **ResponseType** igual a **ClientOtherError** |
| **AnonymousClientTimeoutError** | As transações com a dimensão **ResponseType** igual a **ClientTimeoutError** |
| **AnonymousNetworkError** | As transações com a dimensão **ResponseType** igual a **NetworkError** |
| **AnonymousServerOtherError** | As transações com a dimensão **ResponseType** igual a **ServerOtherError** |
| **AnonymousServerTimeoutError** | As transações com a dimensão **ResponseType** igual a **ServerTimeoutError** |
| **AnonymousSuccess** | As transações com a dimensão **ResponseType** igual a **com êxito** |
| **AnonymousThrottlingError** | As transações com a dimensão **ResponseType** igual a **ClientThrottlingError** ou **ServerBusyError** |
| **AuthorizationError** | As transações com a dimensão **ResponseType** igual a **AuthorizationError** |
| **Disponibilidade** | **Disponibilidade** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | As transações com a dimensão **ResponseType** igual a **ClientOtherError** |
| **ClientTimeoutError** | As transações com a dimensão **ResponseType** igual a **ClientTimeoutError** |
| **NetworkError** | As transações com a dimensão **ResponseType** igual a **NetworkError** |
| **PercentAuthorizationError** | As transações com a dimensão **ResponseType** igual a **AuthorizationError** |
| **PercentClientOtherError** | As transações com a dimensão **ResponseType** igual a **ClientOtherError** |
| **PercentNetworkError** | As transações com a dimensão **ResponseType** igual a **NetworkError** |
| **PercentServerOtherError** | As transações com a dimensão **ResponseType** igual a **ServerOtherError** |
| **PercentSuccess** | As transações com a dimensão **ResponseType** igual a **com êxito** |
| **PercentThrottlingError** | As transações com a dimensão **ResponseType** igual a **ClientThrottlingError** ou **ServerBusyError** |
| **PercentTimeoutError** | As transações com a dimensão **ResponseType** igual a **ServerTimeoutError** ou **ResponseType** igual a **ClientTimeoutError** |
| **SASAuthorizationError** | As transações com a dimensão **ResponseType** igual a **AuthorizationError** |
| **SASClientOtherError** | As transações com a dimensão **ResponseType** igual a **ClientOtherError** |
| **SASClientTimeoutError** | As transações com a dimensão **ResponseType** igual a **ClientTimeoutError** |
| **SASNetworkError** | As transações com a dimensão **ResponseType** igual a **NetworkError** |
| **SASServerOtherError** | As transações com a dimensão **ResponseType** igual a **ServerOtherError** |
| **SASServerTimeoutError** | As transações com a dimensão **ResponseType** igual a **ServerTimeoutError** |
| **SASSuccess** | As transações com a dimensão **ResponseType** igual a **com êxito** |
| **SASThrottlingError** | As transações com a dimensão **ResponseType** igual a **ClientThrottlingError** ou **ServerBusyError** |
| **ServerOtherError** | As transações com a dimensão **ResponseType** igual a **ServerOtherError** |
| **ServerTimeoutError** | As transações com a dimensão **ResponseType** igual a **ServerTimeoutError** |
| **Êxito** | As transações com a dimensão **ResponseType** igual a **com êxito** |
| **ThrottlingError** | **Transações** com a dimensão **ResponseType** igual a **ClientThrottlingError** ou **ServerBusyError**|
| **TotalBillableRequests** | **Transações** |
| **TotalEgress** | **Saída** |
| **TotalIngress** | **Entrada** |
| **TotalRequests** | **Transações** |

## <a name="faq"></a>FAQ

### <a name="how-should-i-migrate-existing-alert-rules"></a>Como posso migrar a regras de alerta existentes?

Se já criou regras de alerta clássicas com base nas métricas do armazenamento antigo, terá de criar regras com base no novo esquema de métrico de alerta de novo.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>É nova métricos os dados armazenados na mesma conta de armazenamento por predefinição?

Não. Para os dados métricos para uma conta de armazenamento de arquivo, utilize o [API de definição de diagnóstico do Azure Monitor](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Passos Seguintes

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Métricas do Storage no Monitor do Azure](./storage-metrics-in-azure-monitor.md)
