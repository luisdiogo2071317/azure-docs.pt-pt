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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 455b2a70568566bff5b1ea4c185568a1758f7db3
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274909"
---
# <a name="sfctl-container"></a>sfctl container
Contentor de execução relacionados com os comandos num nó de cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| api de invocar | Invoca a API de contentor num contêiner implementado num nó do Service Fabric para o pacote de código específico. |
| registos | Obtém os registos de contentor para contentor implementado num nó do Service Fabric para o pacote de código específico. |

## <a name="sfctl-container-invoke-api"></a>contentor de sfctl invocar-api
Invoca a API de contentor num contêiner implementado num nó do Service Fabric para o pacote de código específico.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de aplicação [necessário] | A identidade da aplicação. <br><br> Isso normalmente é o nome completo do aplicativo sem que o "recursos de infraestrutura\:" esquema de URI. A partir da versão 6.0, são delimitados nomes hierárquicos com o "\~" caráter. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:app1/myapp /", a identidade da aplicação seria "myapp\~app1" em 6.0 + e "myapp/app1" nas versões anteriores. |
| – código-pacote--id de instância [necessário] | ID identifica exclusivamente uma instância de pacote de código implementada num nó do service fabric. <br><br> Podem ser obtidos por "serviço código--lista de pacotes". |
| -nome de pacote de código [necessário] | O nome do pacote de código especificado no manifesto de serviço registado como parte de um tipo de aplicação no cluster do Service Fabric. |
| --api-uri-caminho do contentor [necessário] | Caminho de URI da API REST do contentor, utilize "{id}" em vez de nome/id de contentor. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de manifesto de serviço [necessário] | O nome de um manifesto de serviço registado como parte de um tipo de aplicação no cluster do Service Fabric. |
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
Obtém os registos de contentor para contentor implementado num nó do Service Fabric para o pacote de código específico.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de aplicação [necessário] | A identidade da aplicação. <br><br> Isso normalmente é o nome completo do aplicativo sem que o "recursos de infraestrutura\:" esquema de URI. A partir da versão 6.0, são delimitados nomes hierárquicos com o "\~" caráter. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:app1/myapp /", a identidade da aplicação seria "myapp\~app1" em 6.0 + e "myapp/app1" nas versões anteriores. |
| – código-pacote--id de instância [necessário] | ID de instância de pacote do código, que pode ser obtido por "serviço código--lista de pacotes". |
| -nome de pacote de código [necessário] | O nome do pacote de código especificado no manifesto de serviço registado como parte de um tipo de aplicação no cluster do Service Fabric. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de manifesto de serviço [necessário] | O nome de um manifesto de serviço registado como parte de um tipo de aplicação no cluster do Service Fabric. |
| – Cauda | Número de linhas para mostrar a partir do final dos registos. A predefinição é 100. 'tudo' para mostrar os registos completos. |
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