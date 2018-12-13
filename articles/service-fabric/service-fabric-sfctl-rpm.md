---
title: O Azure Service Fabric CLI sfctl rpm | Documentos da Microsoft
description: Descreve os comandos do CLI do Service Fabric sfctl rpm.
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
ms.openlocfilehash: a8ac3899b6ae80668c4472f24b508ba6ba7e2644
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275317"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Consultar e enviar comandos para o serviço de Gestor de reparação.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| aprovar-force | Força a aprovação da tarefa de reparação de determinado. |
| delete | Elimina uma tarefa de reparação concluída. |
| list | Obtém uma lista de tarefas de reparo correspondência os filtros de determinado. |

## <a name="sfctl-rpm-approve-force"></a>o sfctl rpm aprovar força
Força a aprovação da tarefa de reparação de determinado.

Esta API suporta a plataforma do Service Fabric; não se destina a ser utilizada diretamente a partir de seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de tarefa [necessário] | O ID da tarefa de reparação. |
| – versão | O número da versão atual da tarefa de reparação. Se diferente de zero, o pedido apenas terá êxito se este valor corresponde a versão atual real da tarefa de reparação. Se zero, em seguida, não será efetuada nenhuma verificação de versão. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-rpm-delete"></a>o sfctl rpm delete
Elimina uma tarefa de reparação concluída.

Esta API suporta a plataforma do Service Fabric; não se destina a ser utilizada diretamente a partir de seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de tarefa [necessário] | O ID da tarefa de reparação concluída a ser eliminado. |
| – versão | O número da versão atual da tarefa de reparação. Se diferente de zero, o pedido apenas terá êxito se este valor corresponde a versão atual real da tarefa de reparação. Se zero, em seguida, não será efetuada nenhuma verificação de versão. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-rpm-list"></a>lista de rpm sfctl
Obtém uma lista de tarefas de reparo correspondência os filtros de determinado.

Esta API suporta a plataforma do Service Fabric; não se destina a ser utilizada diretamente a partir de seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – executor filtro | O nome do executor de reparação cujas tarefas solicitadas devem ser incluídas na lista. |
| – filtro de estado | Um bit a bit-OR dos seguintes valores, especificando qual tarefa Estados deve ser incluído na lista de resultados. <br> 1 - criado <br>2 - solicitadas  <br>4 - preparar  <br>8 - aprovado  <br>16 - em execução  <br>32 - restaurar  <br>64 - concluída |
| – filtro de id de tarefa | Corresponder o prefixo de ID de tarefa reparar. |

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