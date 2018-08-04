---
title: Cluster do Azure Service Fabric CLI - sfctl sa-| Documentos da Microsoft
description: Descreve os comandos de cluster do Service Fabric CLI sfctl autónomo.
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
ms.openlocfilehash: d7f33bf0657ca2a6888387b7651706f9de537bb4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494361"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Gerir clusters autónomos do Service Fabric.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Config | Obter a configuração de cluster autónomo do Service Fabric. |
| atualização de configuração | Começar a atualizar a configuração de um cluster autónomo do Service Fabric. |
| Estado de atualização | Obter o estado de atualização de configuração de cluster de um cluster autónomo do Service Fabric. |

## <a name="sfctl-sa-cluster-config"></a>configuração de cluster de sa de sfctl
Obter a configuração de cluster autónomo do Service Fabric.

A configuração de cluster contém as propriedades do cluster, que incluem os tipos de nó diferente no cluster, configurações de segurança, falhas e topologias de domínio de atualização, etc.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – versão de api de configuração [necessário] | A versão de API de configuração do json de cluster autónomo. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>atualização de configuração do sa-cluster sfctl
Começar a atualizar a configuração de um cluster autónomo do Service Fabric.

Valide os parâmetros de atualização de configuração fornecidos e começar a atualizar a configuração do cluster, se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -cluster-config [necessário] | A configuração de cluster que será aplicada ao cluster. |
| – políticas de estado de funcionamento de aplicações | JSON com a codificação de dicionário de pares de nome de tipo de aplicação e a percentagem máxima em mau estado de funcionamento antes de gerar o erro. |
| --delta-unhealthy-nodes | O máximo permitido de percentagem de degradação de estado de funcionamento de delta durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| --health-check-retry | O período de tempo entre tentativas para efetuar um Estado de funcionamento verifica se o aplicativo ou o cluster não está em bom estado.  Predefinido\: PT0H0M0S. |
| -verificação de estado de funcionamento-estável | O período de tempo que a aplicação ou o cluster tem de permanecer em bom estado.  Predefinido\: PT0H0M0S. |
| – espera de verificação de estado de funcionamento | O período de tempo de espera após a conclusão de um domínio de atualização antes de iniciar o estado de funcionamento verifica o processo.  Predefinido\: PT0H0M0S. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |
| – mau estado de funcionamento de aplicações | O máximo permitido porcentagem de aplicativos com problemas durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| – mau estado de funcionamento de nós | O máximo permitido de percentagem de nós de mau estado de funcionamento durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| --upgrade-domain-delta-unhealthy-nodes | O máximo permitido de percentagem de degradação de estado de funcionamento de delta do domínio de atualização durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| – atualização-domínio-tempo limite | O tempo limite para o domínio de atualização.  Predefinido\: PT0H0M0S. |
| – tempo limite da atualização | O tempo limite da atualização.  Predefinido\: PT0H0M0S. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

### <a name="examples"></a>Exemplos

Iniciar uma cluster configuração atualização sfctl sa-config-atualização do cluster – cluster-config <YOUR CLUSTER CONFIG> – políticas de estado de funcionamento de aplicações "{" fabric: / System ": {"ConsiderWarningAsError": true}}"

## <a name="sfctl-sa-cluster-upgrade-status"></a>Estado de atualização do cluster de sa sfctl
Obter o estado de atualização de configuração de cluster de um cluster autónomo do Service Fabric.

Obter a configuração de cluster detalhes de estado de atualização de um cluster autónomo do Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
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