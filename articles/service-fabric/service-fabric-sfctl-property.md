---
title: Propriedade do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de propriedade de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: acade3d828c785af9468baa30086d3b79542f9b7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34764030"
---
# <a name="sfctl-property"></a>propriedade de sfctl
Propriedades de arquivo e a consulta em nomes de Service Fabric.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| eliminar | Elimina a propriedade de serviço recursos de infraestrutura especificada. |
| Introdução | Obtém a propriedade de serviço recursos de infraestrutura especificada. |
| lista | Obtém as informações em todas as propriedades de Service Fabric sob um nome específico. |
| Colocar | Cria ou atualiza uma propriedade de Service Fabric. |

## <a name="sfctl-property-delete"></a>eliminação de propriedade sfctl
Elimina a propriedade de serviço recursos de infraestrutura especificada.

Elimina a propriedade de Service Fabric especificada sob um nome específico. Uma propriedade tem de ser criada antes de ser eliminado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de nome [necessário] | Nome de Service Fabric, sem o ' recursos de infraestrutura\:' esquema de URI. |
| --nome da propriedade [necessário] | Especifica o nome da propriedade a obter. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-property-get"></a>obtenção da propriedade sfctl
Obtém a propriedade de serviço recursos de infraestrutura especificada.

Obtém a propriedade de Service Fabric especificada sob um nome específico. Esta ação irá sempre devolver valor e metadados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de nome [necessário] | Nome de Service Fabric, sem o ' recursos de infraestrutura\:' esquema de URI. |
| --nome da propriedade [necessário] | Especifica o nome da propriedade a obter. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-property-list"></a>lista de propriedades de sfctl
Obtém as informações em todas as propriedades de Service Fabric sob um nome específico.

Um nome de Service Fabric pode ter um ou mais propriedades com nome que armazenam informações personalizadas. Esta operação obtém as informações sobre estas propriedades numa lista paginada. As informações incluem o nome, valor e os metadados sobre cada uma das propriedades.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de nome [necessário] | Nome de Service Fabric, sem o ' recursos de infraestrutura\:' esquema de URI. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -valores incluem | Permite especificar se pretende incluir os valores das propriedades devolvidos. TRUE se os valores devem ser devolvidos com os metadados; FALSO para devolver apenas os metadados da propriedade. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-property-put"></a>sfctl propriedade put
Cria ou atualiza uma propriedade de Service Fabric.

Cria ou atualiza a propriedade de Service Fabric especificada sob um nome específico.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de nome [necessário] | Nome de Service Fabric, sem o ' recursos de infraestrutura\:' esquema de URI. |
| --nome da propriedade [necessário] | O nome da propriedade de Service Fabric. |
| -valor [necessário] | Descreve um valor de propriedade de Service Fabric. Esta é uma cadeia JSON. <br><br> A cadeia json tem dois campos "Kind" dos dados e o 'Valor' dos dados. O valor de "Kind" tem de ser o primeiro item apareça na cadeia de JSON e pode ser valores 'Binary', 'Int64', 'Double', 'String' ou 'Guid'. O valor deve conseguir serializar-os tipos de indicado. Os valores 'Tipo' e 'Data' devem ser fornecidos como cadeias. |
| – o tipo de id personalizada | Tipo de personalizada a propriedade ID. Utilizar esta propriedade, o utilizador é capaz de marcar o tipo do valor da propriedade. |
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