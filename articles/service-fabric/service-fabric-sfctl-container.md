---
title: Contentor do Azure Service Fabric CLI - sfctl | Documentos da Microsoft
description: Descreve os comandos de contentor do Service Fabric CLI sfctl.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 27108d27ee27346e4cba44e6778faff56df70a36
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495133"
---
# <a name="sfctl-container"></a>sfctl container
Contentor de execução relacionados com os comandos num nó de cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| api de invocar | Invoca a REST API do contentor. |
| registos | Ao obter registos de contentor. |

## <a name="sfctl-container-invoke-api"></a>contentor de sfctl invocar-api
Invoca a REST API do contentor.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de aplicação [necessário] | Identidade da aplicação. |
| – código-pacote--id de instância [necessário] | ID de instância de pacote do código, que pode ser obtido por "serviço código--lista de pacotes". |
| -nome de pacote de código [necessário] | Nome do pacote de código. |
| --api-uri-caminho do contentor [necessário] | Caminho de URI da API REST do contentor, utilize "{id}" em vez de nome/id de contentor. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de manifesto de serviço [necessário] | Nome do manifesto do serviço. |
| – contentores-api-corpo | Corpo do pedido HTTP para o contentor REST API. |
| – contentores-api-tipo de conteúdo | Tipo de conteúdo para o contentor REST API, a predefinição é "application/json". |
| – contentores-api-http-verbo | Verbo HTTP para o contentor REST API, o padrão é GET. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-container-logs"></a>registos de contentor sfctl
Ao obter registos de contentor.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de aplicação [necessário] | Identidade da aplicação. |
| – código-pacote--id de instância [necessário] | ID de instância de pacote do código, que pode ser obtido por "serviço código--lista de pacotes". |
| -nome de pacote de código [necessário] | Nome do pacote de código. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de manifesto de serviço [necessário] | Nome do manifesto do serviço. |
| – Cauda | Só devolva este número de linhas de registo do fim dos registos. Especifica como um número inteiro ou todas para todas as linhas de log de saída. Predefinições para "todos". |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

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