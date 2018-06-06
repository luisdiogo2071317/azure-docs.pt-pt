---
title: Cluster do Azure Service Fabric CLI - sfctl sa-| Microsoft Docs
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
ms.date: 05/23/2018
ms.author: bikang
ms.openlocfilehash: ffdbff7edc5af187071615c8b1e61790b3a38429
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764041"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Gerir clusters autónomos do Service Fabric.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Configuração | Obter a configuração de cluster do Service Fabric autónoma. |
| atualização da configuração | Comece a atualizar a configuração de um cluster do Service Fabric autónomo. |
| Estado de atualização | Obter o estado de atualização de configuração de cluster de um cluster do Service Fabric autónomo. |

## <a name="sfctl-sa-cluster-config"></a>configuração de sa cluster sfctl
Obter a configuração de cluster do Service Fabric autónoma.

Obter a configuração de cluster do Service Fabric autónoma. A configuração de cluster contém propriedades do cluster, que incluem os tipos de outro nó no cluster, configurações de segurança, falhas e topologias de domínio de atualização, etc.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -versão de api de configuração [necessária] | A versão de API a autónomo json da configuração do cluster. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>configuração de sa cluster sfctl-atualização
Comece a atualizar a configuração de um cluster do Service Fabric autónomo.

Valide os parâmetros de atualização da configuração especificada e comece a atualizar a configuração do cluster, se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --configuração do cluster [necessário] | A configuração do cluster. |
| --delta-unhealthy-nodes | O máximo permitido de percentagem de degradação de estado de funcionamento de diferenças durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| --health-check-retry | O período de tempo entre tentativas para efetuar um Estado de funcionamento verifica se a aplicação ou o cluster não está em bom estado.  Predefinição\: PT0H0M0S. |
| -estável estado de funcionamento-verificação | O período de tempo que a aplicação ou o cluster tem de permanecer bom estado de funcionamento.  Predefinição\: PT0H0M0S. |
| -espera de verificação de estado de funcionamento | O período de tempo de espera após a conclusão de um domínio de atualização antes de iniciar o estado de funcionamento verifica processo.  Predefinição\: PT0H0M0S. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -mau estado de funcionamento de aplicações | O máximo permitido percentagem das aplicações danificadas durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| -mau estado de funcionamento de nós | O máximo permitido de percentagem de nós mau estado de funcionamento durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| --upgrade-domain-delta-unhealthy-nodes | O máximo permitido de percentagem de degradação de estado de funcionamento de delta do domínio de atualização durante a atualização. Valores permitidos são valores de número inteiro entre zero e 100. |
| -atualização--tempo limite do domínio | O tempo limite para o domínio de atualização.  Predefinição\: PT0H0M0S. |
| -tempo limite de atualização | O tempo limite de atualização.  Predefinição\: PT0H0M0S. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-sa-cluster-upgrade-status"></a>Estado de atualização do cluster de sa sfctl
Obter o estado de atualização de configuração de cluster de um cluster do Service Fabric autónomo.

Obter a configuração de cluster detalhes de estado de atualização de um cluster do Service Fabric autónomo.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
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