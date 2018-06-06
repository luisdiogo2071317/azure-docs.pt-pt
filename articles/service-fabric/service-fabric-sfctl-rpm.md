---
title: Azure Service Fabric CLI sfctl rpm | Microsoft Docs
description: Descreve os comandos de rpm sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: 5e7cf1b28e8f38f0539e0084e1f745e0dab4dec3
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763617"
---
# <a name="sfctl-rpm"></a>sfctl rpm
Consultar e enviar comandos para o serviço do Gestor de reparação.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| force aprovar | Força a aprovação da tarefa reparar indicado. |
| eliminar | Elimina uma tarefa de reparação foi concluída. |
| lista | Obtém uma lista de tarefas de reparação que correspondam aos filtros de indicado. |

## <a name="sfctl-rpm-approve-force"></a>sfctl rpm aprovar bruta
Força a aprovação da tarefa reparar indicado.

Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de tarefa [necessário] | O ID da tarefa de reparação. |
| -versão | O número da versão atual da tarefa de reparação. Se diferente de zero, o pedido apenas terá êxito se este valor corresponde à versão atual real da tarefa de reparação. Se zero, em seguida, não será efetuada nenhuma verificação da versão. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-rpm-delete"></a>Eliminar sfctl rpm
Elimina uma tarefa de reparação foi concluída.

Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de tarefa [necessário] | O ID da tarefa de reparação foi concluída a eliminar. |
| -versão | O número da versão atual da tarefa de reparação. Se diferente de zero, o pedido apenas terá êxito se este valor corresponde à versão atual real da tarefa de reparação. Se zero, em seguida, não será efetuada nenhuma verificação da versão. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-rpm-list"></a>lista de rpm sfctl
Obtém uma lista de tarefas de reparação que correspondam aos filtros de indicado.

Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -executor filtro | O nome do executor a reparação cujas tarefas pedidas devem ser incluídas na lista. |
| -filtro de estado | Um bit a bit-OR dos seguintes valores, especificando que tarefas Estados deve ser incluída na lista de resultados. <br> 1 - criado <br>2 - reclamado  <br>4 - preparação  <br>8 - aprovado  <br>16 - execução  <br>32 - restaurar  <br>64 - concluída |
| – o filtro de id de tarefa | Corresponder o prefixo de ID de tarefa reparar. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).