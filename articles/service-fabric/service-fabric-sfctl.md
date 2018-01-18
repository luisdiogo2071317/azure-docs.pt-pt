---
title: Azure Service Fabric CLI - sfctl | Microsoft Docs
description: "Descreve os comandos de sfctl CLI de recursos de infraestrutura de serviço."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 23adc7fa6ee53d1c5edd10b2772f8825dbcef2a8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl"></a>sfctl 
Comandos para a gestão de clusters de Service Fabric e entidades de mensagens em fila. Esta versão é compatível com o tempo de execução do Service Fabric 6.0. Comandos seguem o padrão de verbo substantivo, consulte os subgrupos seguintes para obter mais informações.

## <a name="subgroups"></a>Subgrupos
|Subgrupos|Descrição|
| --- | --- |
| [application](service-fabric-sfctl-application.md)| Criar, eliminar e gerir aplicações e tipos de aplicações.|
| [chaos](service-fabric-sfctl-chaos.md)   | Iniciar, parar e relatórios sobre o serviço de teste chaos.|
| [cluster](service-fabric-sfctl-cluster.md) | Selecione, gerir e operar clusters de Service Fabric.|
| [compose](service-fabric-sfctl-compose.md) | Criar, eliminar e gerir aplicações Docker Compose.|
| [is](service-fabric-sfctl-is.md)      | Consultar e enviar comandos para o serviço de infraestrutura.|
| [node](service-fabric-sfctl-node.md)    | Gerir os nós que formam um cluster.|
| [partition](service-fabric-sfctl-partition.md)  | Consultar e gerir partições para qualquer serviço.|
| [rpm](service-fabric-sfctl-rpm.md)        | Consultar e enviar comandos para o serviço do Gestor de reparação.|
| [replica](service-fabric-sfctl-replica.md) | Gerir as réplicas que pertencem a partições de serviço.|
| [service](service-fabric-sfctl-service.md) | Criar, eliminar e gerir o serviço, tipos de serviço e pacotes de serviços.|
| [store](service-fabric-sfctl-store.md)   | Efetue operações de nível de ficheiro básico no arquivo de imagens de cluster.|

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).