---
title: Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de sfctl CLI de recursos de infraestrutura de serviço.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: 456257e54da83ac629039b714cc74f9dafda2174
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763277"
---
# <a name="sfctl"></a>sfctl
Comandos para a gestão de clusters de Service Fabric e entidades de mensagens em fila. Esta versão é compatível com o tempo de execução do Service Fabric 6.2.

Comandos seguem o padrão de substantivo verbo. Para obter mais informações, consulte os subgrupos.

## <a name="subgroups"></a>Subgrupos
|Subgrupos|Descrição|
| --- | --- |
| [application](service-fabric-sfctl-application.md) | Criar, eliminar e gerir aplicações e tipos de aplicações. |
| [chaos](service-fabric-sfctl-chaos.md) | Iniciar, parar e relatórios sobre o serviço de teste chaos. |
| [cluster](service-fabric-sfctl-cluster.md) | Selecione, gerir e operar clusters de Service Fabric. |
| [Componha](service-fabric-sfctl-compose.md) | Criar, eliminar e gerir aplicações Docker Compose. |
| [contentor](service-fabric-sfctl-container.md) | Execute comandos relacionados com o contentor num nó de cluster. |
| [is](service-fabric-sfctl-is.md) | Consultar e enviar comandos para o serviço de infraestrutura. |
| [Nó](service-fabric-sfctl-node.md) | Gerir os nós que formam um cluster. |
| [partition](service-fabric-sfctl-partition.md) | Consultar e gerir partições para qualquer serviço. |
| [Propriedade](service-fabric-sfctl-property.md) | Propriedades de arquivo e a consulta em nomes de Service Fabric. |
| [replica](service-fabric-sfctl-replica.md) | Gerir as réplicas que pertencem a partições de serviço. |
| [rpm](service-fabric-sfctl-rpm.md) | Consultar e enviar comandos para o serviço do Gestor de reparação. |
| [cluster de SA](service-fabric-sfctl-sa-cluster.md) | Gerir clusters autónomos do Service Fabric. |
| [service](service-fabric-sfctl-service.md) | Criar, eliminar e gerir o serviço, tipos de serviço e pacotes de serviços. |
| [store](service-fabric-sfctl-store.md) | Efetue operações de nível de ficheiro básico no arquivo de imagens de cluster. |

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).