---
title: Desempenho de recursos de infraestrutura de serviço do Azure monitorização | Microsoft Docs
description: Saiba mais sobre os contadores de desempenho de monitorização e diagnóstico de clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 9e740dd3acce842f888e5994fe8f46222477adc1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208238"
---
# <a name="performance-metrics"></a>Métricas de desempenho

Métricas devem ser recolhidas para compreender o desempenho do seu cluster, bem como as aplicações em execução no mesmo. Para clusters de Service Fabric, recomendamos que recolher os seguintes contadores de desempenho.

## <a name="nodes"></a>Nós

Para as máquinas do cluster, considere os seguintes contadores de desempenho para melhor compreender a carga em cada máquina e fazer cluster adequado dimensionamento decisões a recolher.

| Categoria do contador | Nome do Contador |
| --- | --- |
| PhysicalDisk (por disco) | Média Comprimento de fila de leitura do disco |
| PhysicalDisk (por disco) | Média Comprimento de fila de escrita de disco |
| PhysicalDisk (por disco) | Média Disco seg/leitura |
| PhysicalDisk (por disco) | Média Disco seg/escrita |
| PhysicalDisk (por disco) | Leituras de disco/seg |
| PhysicalDisk (por disco) | Bytes Lidos de Disco/seg |
| PhysicalDisk (por disco) | Escritas de disco/seg |
| PhysicalDisk (por disco) | Bytes Escritos em Disco/seg |
| Memória | MBytes disponíveis |
| PagingFile | % Utilização |
| Processor(total) | % Tempo do processador |
| Processo (para serviço) | % Tempo do processador |
| Processo (para serviço) | ID de processo |
| Processo (para serviço) | Bytes Privados |
| Processo (para serviço) | Número de Threads |
| Processo (para serviço) | Bytes virtuais |
| Processo (para serviço) | Conjunto de trabalho |
| Processo (para serviço) | Conjunto de trabalho - privado |
| Interface(all-instances) de rede | Comprimento da fila de saída |
| Interface(all-instances) de rede | Pacotes de saída rejeitados |
| Interface(all-instances) de rede | Pacotes receberam rejeitados |
| Interface(all-instances) de rede | Erros de saída de pacotes |
| Interface(all-instances) de rede | Erros recebidos pacotes |

## <a name="net-applications-and-services"></a>Serviços e aplicações de .NET

Recolha os seguintes contadores se estiver a implementar os serviços de .NET para o cluster. 

| Categoria do contador | Nome do Contador |
| --- | --- |
| Memória de CLR de .NET (por serviço) | ID de Processo |
| Memória de CLR de .NET (por serviço) | N. º total dos Bytes consolidados |
| Memória de CLR de .NET (por serviço) | N. º total reservado Bytes |
| Memória de CLR de .NET (por serviço) | N. º de bytes em todas as pilhas |
| Memória de CLR de .NET (por serviço) | # Geração 0 |
| Memória de CLR de .NET (por serviço) | # Geração 1 |
| Memória de CLR de .NET (por serviço) | # Geração 2 |
| Memória de CLR de .NET (por serviço) | % De tempo na GC |

### <a name="service-fabrics-custom-performance-counters"></a>Contadores de desempenho personalizado do Service Fabric

Service Fabric gera uma quantidade substancial de contadores de desempenho personalizados. Se tiver o SDK instalado, pode ver a lista completa no seu computador Windows na sua aplicação de Monitor de desempenho (Iniciar > Monitor de desempenho). 

As aplicações está a implementar para o cluster, se estiver a utilizar Reliable Actors, adicione countes de `Service Fabric Actor` e `Service Fabric Actor Method` categorias (consulte [serviço recursos de infraestrutura Reliable Actors diagnóstico](service-fabric-reliable-actors-diagnostics.md)).

Se utilizar Reliable Services, temos de forma semelhante `Service Fabric Service` e `Service Fabric Service Method` categorias de contador deve recolher contadores do. 

Se utilizar coleções fiável, recomendamos adicionar o `Avg. Transaction ms/Commit` do `Service Fabric Transactional Replicator` para recolher a latência média de consolidação por métrica de transação.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [geração de eventos ao nível da plataforma](service-fabric-diagnostics-event-generation-infra.md) no Service Fabric
* Recolher a métrica do desempenho através de [agente do OMS](service-fabric-diagnostics-oms-agent.md)
