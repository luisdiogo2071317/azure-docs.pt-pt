---
title: O Azure Service Fabric CLI sfctl malha volume | Documentos da Microsoft
description: Descreve os comandos de volume de malha do CLI do Service Fabric sfctl.
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
ms.openlocfilehash: cbd16ca66002446b3907c1ecaa6c0b334b35dc57
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285222"
---
# <a name="sfctl-mesh-volume"></a>volume de malha sfctl
Obter e eliminar recursos de volume.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| delete | Elimina o recurso de Volume. |
| list | Apresenta uma lista de todos os recursos de volume. |
| Show | Obtém o recurso de Volume com o nome fornecido. |

## <a name="sfctl-mesh-volume-delete"></a>o sfctl malha volume delete
Elimina o recurso de Volume.

Elimina o recurso de Volume identificado pelo nome.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome -n [necessário] | O nome do volume. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-mesh-volume-list"></a>lista de volumes de malha sfctl
Apresenta uma lista de todos os recursos de volume.

Obtém as informações sobre todos os recursos de volume num grupo de recursos específico. As informações incluem a descrição e outras propriedades do Volume.

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-mesh-volume-show"></a>o sfctl malha volume show
Obtém o recurso de Volume com o nome fornecido.

Obtém as informações sobre o recurso de Volume com o nome fornecido. As informações incluem a descrição e outras propriedades do Volume.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome -n [necessário] | O nome do volume. |

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