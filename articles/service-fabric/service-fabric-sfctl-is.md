---
title: O Azure Service Fabric CLI - sfctl é | Documentos da Microsoft
description: Descreve a CLI do Service Fabric sfctl é comandos.
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
ms.openlocfilehash: 53e49099fd3486d51f021528c9354cf32f4952d2
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492794"
---
# <a name="sfctl-is"></a>sfctl is
Consultar e enviar comandos para o serviço de infraestrutura.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| command | Invoca um comando administrativo na instância do serviço de infraestrutura especificada. |
| consulta | Invoca uma consulta só de leitura na instância do serviço de infraestrutura especificada. |

## <a name="sfctl-is-command"></a>o sfctl é o comando
Invoca um comando administrativo na instância do serviço de infraestrutura especificada.

Para os clusters que têm uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma de enviar comandos específicos de infraestrutura para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma do Service Fabric; não se destina a ser utilizada diretamente a partir de seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – comando [necessário] | O texto do comando a ser invocado. O conteúdo do comando é específica da infraestrutura. |
| – id de serviço | A identidade do serviço de infraestrutura. <br><br> Este é o nome completo do serviço de infraestrutura sem o esquema URI de "malha". Este parâmetro necessário apenas para o cluster que tem mais de uma instância de serviço de infraestrutura em execução. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-is-query"></a>o sfctl é a consulta
Invoca uma consulta só de leitura na instância do serviço de infraestrutura especificada.

Para os clusters que têm uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma de enviar consultas de infra-estrutura específicas para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma do Service Fabric; não se destina a ser utilizada diretamente a partir de seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – comando [necessário] | O texto do comando a ser invocado. O conteúdo do comando é específica da infraestrutura. |
| – id de serviço | A identidade do serviço de infraestrutura. <br><br> Este é o nome completo do serviço de infraestrutura sem o "recursos de infraestrutura\:" esquema de URI. Este parâmetro necessário apenas para o cluster que tem mais de uma instância de serviço de infraestrutura em execução. |
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