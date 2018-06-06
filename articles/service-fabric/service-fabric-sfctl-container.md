---
title: Contentor do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de contentor de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: cd3725ac547a1ed1fd9207dc48ba3b6227e85ef1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764036"
---
# <a name="sfctl-container"></a>contentor de sfctl
Execute comandos relacionados com o contentor num nó de cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| api de invocação | Invocar a REST API do contentor. |
| registos | Obter os registos do contentor. |

## <a name="sfctl-container-invoke-api"></a>contentor de sfctl invoke-api
Invocar a REST API do contentor.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | Identidade da aplicação. |
| -código-pacote--id de instância [necessário] | ID de instância de pacote do código, que pode ser obtido pelos 'Serviço código--lista do pacote'. |
| -nome de pacote de código [necessário] | Nome do pacote do código. |
| --api-uri-caminho do contentor [necessário] | Caminho de URI de API de REST do contentor, utilize {ID} em vez do nome/id de contentor. |
| --nome do nó [necessário] | O nome do nó. |
| --manifesto-nome do serviço [necessário] | Nome do manifesto do serviço. |
| -corpo da api de contentor | Corpo do pedido HTTP para o contentor de REST API. |
| – contentor-api-tipo de conteúdo | Tipo de conteúdo para o contentor de REST API, será assumida a "application/json". |
| – contentor-api-http-verbo | Verbo de HTTP para o contentor de REST API, será assumida a obter. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-container-logs"></a>registos do contentor de sfctl
Obter os registos do contentor.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | Identidade da aplicação. |
| -código-pacote--id de instância [necessário] | ID de instância de pacote do código, que pode ser obtido pelos 'Serviço código--lista do pacote'. |
| -nome de pacote de código [necessário] | Nome do pacote do código. |
| --nome do nó [necessário] | O nome do nó. |
| --manifesto-nome do serviço [necessário] | Nome do manifesto do serviço. |
| -Cauda | Devolva apenas este número de linhas de registo a partir da extremidade dos registos. Especifique como um número inteiro ou a totalidade para todas as linhas de registo de saída. Predefinições para 'tudo'. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

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