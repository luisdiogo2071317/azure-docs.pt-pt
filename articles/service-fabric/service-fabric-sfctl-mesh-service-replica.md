---
title: Serviço de réplica de malha do Azure Service Fabric CLI - sfctl | Documentos da Microsoft
description: Descreve os comandos de réplica do serviço de malha do CLI do Service Fabric sfctl.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: bcf4b8d013783a9fbdb62bcdb8737680bfce7640
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285089"
---
# <a name="sfctl-mesh-service-replica"></a>serviço de malha sfctl-réplica
Obtenha os detalhes de réplica e de réplicas de lista de um determinado serviço num recurso de aplicação.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| list | Apresenta uma lista de todas as réplicas de um serviço. |
| Show | Obtém a réplica especificada do serviço de um aplicativo. |

## <a name="sfctl-mesh-service-replica-list"></a>lista de réplicas do serviço de malha sfctl
Apresenta uma lista de todas as réplicas de um serviço.

Obtém as informações sobre todas as réplicas de um serviço. As informações incluem a descrição e outras propriedades da réplica de serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome da aplicação – nome da aplicação [necessário] | O nome da aplicação. |
| --nome do serviço [necessário] | O nome do serviço. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-mesh-service-replica-show"></a>show de réplica do serviço de malha sfctl
Obtém a réplica especificada do serviço de um aplicativo.

Obtém as informações sobre a réplica de serviço com o nome fornecido. As informações incluem a descrição e outras propriedades da réplica de serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome da aplicação – nome da aplicação [necessário] | O nome da aplicação. |
| -nome -n [necessário] | O nome da réplica de serviço. |
| --nome do serviço [necessário] | O nome do serviço. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |


## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI do Service Fabric.
- Saiba como utilizar a CLI do Service Fabric utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).