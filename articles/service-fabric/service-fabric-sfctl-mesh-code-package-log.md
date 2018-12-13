---
title: Registo de pacote de código de malha do Azure Service Fabric CLI - sfctl | Documentos da Microsoft
description: Descreve os comandos de log de pacote de código de malha do CLI do Service Fabric sfctl.
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
ms.openlocfilehash: 81ddcc8c5685a839afabc1e82ecf4246cb813c21
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285096"
---
# <a name="sfctl-mesh-code-package-log"></a>o sfctl malha código pacote-log
Obter os registos do contentor do pacote de código especificada para a réplica de determinado serviço.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Get | Obtém os registos do contentor. |

## <a name="sfctl-mesh-code-package-log-get"></a>get de log de pacote de código de malha sfctl
Obtém os registos do contentor.

Obtém os registos para o contentor do pacote de código especificada da réplica de serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome da aplicação – nome da aplicação [necessário] | O nome da aplicação. |
| -nome de pacote de código [necessário] | O nome do pacote do código do serviço. |
| -nome-réplica [necessário] | Nome da réplica de Service Fabric. |
| --nome do serviço [necessário] | O nome do serviço. |
| – Cauda | Número de linhas para mostrar a partir do final dos registos. A predefinição é 100. 'tudo' para mostrar os registos completos. |

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