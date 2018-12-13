---
title: O Azure Service Fabric CLI sfctl definições telemetria | Documentos da Microsoft
description: Descreve os comandos de telemetria de definições do CLI do Service Fabric sfctl.
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
ms.openlocfilehash: e0e229f0edb078fe9ce289d0089f34d7cbf9dbf8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285397"
---
# <a name="sfctl-settings-telemetry"></a>telemetria de definições de sfctl
Configure definições de telemetria locais para esta instância de sfctl.

Telemetria de Sfctl recolhe o nome do comando sem parâmetros fornecidos ou os respetivos valores, sfctl versão, tipo de SO, versão do python, o sucesso ou falha do comando, a mensagem de erro devolvido.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| telemetria do conjunto | Ativar ou desativar a telemetria. |

## <a name="sfctl-settings-telemetry-set-telemetry"></a>conjunto de telemetria de definições de sfctl-telemetria
Ativar ou desativar a telemetria.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – desligar | Desative a telemetria. |
| – no | Ative a telemetria. Este é o valor predefinido. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

### <a name="examples"></a>Exemplos

Desative a telemetria.

```
sfctl settings telemetry set_telemetry --off
```

Ative a telemetria.

```
sfctl settings telemetry set_telemetry --on
```


## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI do Service Fabric.
- Saiba como utilizar a CLI do Service Fabric utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).