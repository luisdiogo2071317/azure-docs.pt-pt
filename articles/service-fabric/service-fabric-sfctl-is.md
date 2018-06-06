---
title: Azure Service Fabric CLI - sfctl é | Microsoft Docs
description: Descreve a CLI de recursos de infraestrutura de serviço sfctl é comandos.
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
ms.openlocfilehash: aa1e4d588f367351f0bed10370c7d67d50dd9927
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763056"
---
# <a name="sfctl-is"></a>sfctl is
Consultar e enviar comandos para o serviço de infraestrutura.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Comando | Invoca um comando administrativo na instância do serviço de infraestrutura especificada. |
| consulta | Invoca uma consulta de só de leitura na instância do serviço de infraestrutura especificada. |

## <a name="sfctl-is-command"></a>sfctl é comando
Invoca um comando administrativo na instância do serviço de infraestrutura especificada.

Para os clusters que tem uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma para enviar comandos específicos da infraestrutura para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -comando [necessário] | O texto do comando para ser invocado. O conteúdo do comando é específica da infraestrutura. |
| – id do serviço | A identidade do serviço de infraestrutura. <br><br> Este é o nome completo do serviço infraestrutura sem o esquema URI 'recursos de infraestrutura'. Este parâmetro necessário apenas para o cluster que tenha mais do que uma instância do serviço de infraestrutura está em execução. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-is-query"></a>sfctl consulta é
Invoca uma consulta de só de leitura na instância do serviço de infraestrutura especificada.

Para os clusters que tem uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma para enviar consultas de infraestrutura específico para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -comando [necessário] | O texto do comando para ser invocado. O conteúdo do comando é específica da infraestrutura. |
| – id do serviço | A identidade do serviço de infraestrutura. <br><br> Este é o nome completo do serviço infraestrutura sem o ' recursos de infraestrutura\:' esquema de URI. Este parâmetro necessário apenas para o cluster que tenha mais do que uma instância do serviço de infraestrutura está em execução. |
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