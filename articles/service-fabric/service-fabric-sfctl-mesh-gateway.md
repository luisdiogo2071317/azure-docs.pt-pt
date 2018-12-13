---
title: O Azure Service Fabric CLI - sfctl malha gateway | Documentos da Microsoft
description: Descreve os comandos de gateway de malha do CLI do Service Fabric sfctl.
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
ms.openlocfilehash: 7738f838fade47a486b33d808ee2938101406763
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284809"
---
# <a name="sfctl-mesh-gateway"></a>gateway de malha sfctl
Obter e eliminar recursos de gateway de malha.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| delete | Elimina o recurso de Gateway. |
| list | Apresenta uma lista de todos os recursos de gateway. |
| Show | Obtém o recurso de Gateway com o nome fornecido. |

## <a name="sfctl-mesh-gateway-delete"></a>o sfctl malha gateway delete
Elimina o recurso de Gateway.

Elimina o recurso de Gateway identificado pelo nome.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome -n [necessário] | O nome do recurso de gateway. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-mesh-gateway-list"></a>lista de gateways de malha sfctl
Apresenta uma lista de todos os recursos de gateway.

Obtém as informações sobre todos os recursos de gateway num grupo de recursos específico. As informações incluem a descrição e outras propriedades do Gateway.

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-mesh-gateway-show"></a>o sfctl malha gateway show
Obtém o recurso de Gateway com o nome fornecido.

Obtém as informações sobre o recurso de Gateway com o nome fornecido. As informações incluem a descrição e outras propriedades do Gateway.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome -n [necessário] | O nome do recurso de gateway. |

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