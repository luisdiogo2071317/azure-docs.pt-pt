---
title: Azure Service Fabric CLI - sfctl compor | Microsoft Docs
description: Descreve a CLI de recursos de infraestrutura de serviço sfctl compor comandos.
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
ms.openlocfilehash: cc3d3e35ce3dd457d981dfe9420be765cf9fc45a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763413"
---
# <a name="sfctl-compose"></a>sfctl compose
Criar, eliminar e gerir aplicações Docker Compose.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| criar | Cria um recurso de infraestrutura de serviço compor a implementação. |
| lista | Obtém a lista de compose às implementações criadas no cluster de Service Fabric. |
| remover | Elimina um recurso de infraestrutura de serviço existente compor a implementação do cluster. |
| status | Obtém informações sobre um recurso de infraestrutura de serviço compor a implementação. |
| atualização | Inicia a atualizar uma implementação de compose no cluster de Service Fabric. |
| Estado de atualização | Obtém detalhes para a atualização mais recente efetuada em recursos de infraestrutura neste serviço compõem a implementação. |

## <a name="sfctl-compose-create"></a>Componha sfctl criar
Cria um recurso de infraestrutura de serviço compor a implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário] | O nome da implementação. |
| --caminho do ficheiro [necessário] | Caminho para o ficheiro de Docker Compose de destino. |
| -passagem encriptados | Em vez de a pedir uma palavra-passe de registo do contentor, utilize uma frase já encriptado. |
| -passagem tem | Solicitará uma palavra-passe para o registo de contentor. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| --user | Nome de utilizador para estabelecer ligação ao registo do contentor. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-compose-list"></a>sfctl compor lista
Obtém a lista de compose às implementações criadas no cluster de Service Fabric.

Obtém o estado sobre as implementações de compose que foram criadas ou no processo de que está a ser criado no cluster de Service Fabric. A resposta inclui o nome, estado e outros detalhes sobre as implementações de compose. Se a lista de implementações não se enquadram numa página, é devolvida uma página de resultados, bem como um token de continuação que pode ser utilizado para obter a página seguinte.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -resultados máx. | O número máximo de resultados a ser devolvido como parte das consultas paginadas. Este parâmetro define o limite superior no número de resultados devolvidos. Os resultados devolvidos pode ser menor que o especificado máximo de resultados se estes não se enquadram na mensagem de acordo com as restrições de tamanho máximo de mensagem definida na configuração. Se este parâmetro for zero ou não especificado, a consulta paginada inclui resultados tantas quanto possível ajustar na mensagem de retorno. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-compose-remove"></a>Remova de compor sfctl
Elimina um recurso de infraestrutura de serviço existente compor a implementação do cluster.

Elimina um recurso de infraestrutura de serviço existente compor a implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário] | A identidade da implementação. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-compose-status"></a>sfctl compor Estado
Obtém informações sobre um recurso de infraestrutura de serviço compor a implementação.

Devolve o estado da implementação compose que foi criada ou no processo de que está a ser criado no cluster de Service Fabric e cujo nome corresponde à especificado como parâmetro. A resposta inclui o nome, estado e outros detalhes sobre a implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário] | A identidade da implementação. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-compose-upgrade"></a>sfctl compor atualização
Inicia a atualizar uma implementação de compose no cluster de Service Fabric.

Valida os parâmetros de atualização fornecidos e começa a efetuar a atualização da implementação, se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário] | O nome da implementação. |
| --caminho do ficheiro [necessário] | Caminho para o destino Docker compose ficheiro. |
| --default-svc-type-health-map | JSON codificado dicionário que descreve a política de estado de funcionamento utilizada para avaliar o estado de funcionamento dos serviços. |
| -passagem encriptados | Em vez de a pedir uma palavra-passe de registo do contentor, utilize uma frase já encriptado. |
| -Falha de ação | Os valores possíveis incluem\: 'Inválido,' 'Reversão', 'Manual'. |
| -force-reinício | Força o reinício. |
| -passagem tem | Solicitará uma palavra-passe para o registo de contentor. |
| --health-check-retry | Tempo limite de tentativas de verificação do Estado de funcionamento é medido em milissegundos. |
| -estável estado de funcionamento-verificação | Estado de funcionamento Consulte duração estável, medida em milissegundos. |
| -espera de verificação de estado de funcionamento | Duração de espera de verificação do Estado de funcionamento é medido em milissegundos. |
| --replica-set-check | Réplica atualização definir um tempo limite de verificação medido em segundos. |
| -svc-tipo-estado de funcionamento-mapa | JSON codificado lista de objetos que descrevem as políticas de estado de funcionamento utilizadas para avaliar o estado de funcionamento dos tipos de serviço diferente. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -mau estado de funcionamento da aplicação | O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. <br><br> Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster. |
| -atualização--tempo limite do domínio | Tempo limite do domínio de atualização é medido em milissegundos. |
| -tipo de atualização | Predefinição\: sucessiva. |
| -modo de atualização | Os valores possíveis incluem\: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorizado'.  Predefinição\: UnmonitoredAuto. |
| -tempo limite de atualização | Tempo limite de atualização é medido em milissegundos. |
| --user | Nome de utilizador para estabelecer ligação ao registo do contentor. |
| --warning-as-error | Avisos são tratados com a mesma gravidade como erros. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-compose-upgrade-status"></a>sfctl compor o estado de atualização
Obtém detalhes para a atualização mais recente efetuada em recursos de infraestrutura neste serviço compõem a implementação.

Devolve as informações sobre o estado da atualização da implementação do compose juntamente com os detalhes para ajudar a depuração problemas de estado de funcionamento da aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário] | A identidade da implementação. |
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