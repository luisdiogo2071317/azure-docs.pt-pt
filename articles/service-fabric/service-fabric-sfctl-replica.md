---
title: Réplica do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de réplica de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: cd09fe906f77bb06f0ac7afaa6c6cce326dbfa5c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763583"
---
# <a name="sfctl-replica"></a>sfctl replica
Gerir as réplicas que pertencem a partições de serviço.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| Implementado | Obtém os detalhes da réplica implementada num nó de Service Fabric. |
| lista implementado | Obtém a lista de réplicas implementado num nó de Service Fabric. |
| estado de funcionamento | Obtém o estado de funcionamento de uma réplica de monitorização de estado do serviço do Service Fabric ou instância de serviço sem estado. |
| informações | Obtém as informações sobre uma réplica de uma partição de Service Fabric. |
| lista | Obtém as informações acerca de réplicas de uma partição de serviço do Service Fabric. |
| remover | Remove uma réplica de serviço em execução num nó. |
| report-health | Envia um relatório de estado de funcionamento da réplica de Service Fabric. |
| Reiniciar | Reinicia uma réplica de serviço de um serviço persistente em execução num nó. |

## <a name="sfctl-replica-deployed"></a>réplica sfctl implementada
Obtém os detalhes da réplica implementada num nó de Service Fabric.

Obtém os detalhes da réplica implementada num nó de Service Fabric. As informações incluem o tipo de serviço, o nome do serviço, operação de serviço atual, iniciar a operação de serviço atual data hora, ID de partição, ID de réplica/instância, carga comunicada e outras informações.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | O identificador da réplica. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-deployed-list"></a>réplica sfctl implementado-lista
Obtém a lista de réplicas implementado num nó de Service Fabric.

Obtém a lista que contém as informações acerca de réplicas implementado num nó de Service Fabric. As informações incluem o ID de partição, o ID de réplica, o estado da réplica, o nome do serviço, nome do tipo de serviço e outras informações. Utilize PartitionId ou ServiceManifestName parâmetros de consulta para devolver informações sobre as réplicas implementadas, os valores especificados para esses parâmetros de correspondência.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| – id de partição | A identidade da partição. |
| -nome do manifesto do serviço | O nome de um manifesto de serviço registado como parte de um tipo de aplicação de um cluster do Service Fabric. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-health"></a>o estado de funcionamento do sfctl réplica
Obtém o estado de funcionamento de uma réplica de monitorização de estado do serviço do Service Fabric ou instância de serviço sem estado.

Obtém o estado de funcionamento de uma réplica de Service Fabric. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicadas na réplica com base no estado de funcionamento.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | O identificador da réplica. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-info"></a>informações de réplica sfctl
Obtém as informações sobre uma réplica de uma partição de Service Fabric.

A resposta inclui o ID, função, o estado, estado de funcionamento, o nome do nó, tempo de atividade e outros detalhes sobre a réplica.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | O identificador da réplica. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-list"></a>lista de réplicas sfctl
Obtém as informações acerca de réplicas de uma partição de serviço do Service Fabric.

O ponto final GetReplicas devolve informações sobre as réplicas de partição especificada. A resposta inclui o ID, função, o estado, estado de funcionamento, o nome do nó, tempo de atividade e outros detalhes sobre a réplica.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-remove"></a>remover da réplica de sfctl
Remove uma réplica de serviço em execução num nó.

Esta API simula uma falha de réplica do Service Fabric ao remover uma réplica de um cluster do Service Fabric. A remoção fecha a réplica, passa a réplica para a função None e, em seguida, remove todas as informações de estado da réplica do cluster. Esta API testa o caminho de remoção de estado da réplica e simula o caminho de permanente de falhas de relatório através de APIs do cliente. Aviso - daí serão efetuadas verificações de segurança efetuadas quando esta API é utilizada. Utilização incorreta desta API pode levar a perda de dados para os serviços com monitorização de estado. Além disso, o sinalizador de forceRemove afeta todos os outras réplicas alojadas no mesmo processo.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | O identificador da réplica. |
| -force-remover | Remova uma aplicação de Service Fabric ou o serviço forçadamente sem passar a sequência de encerramento correto. Este parâmetro pode ser utilizado para forçadamente eliminar uma aplicação ou serviço para o eliminar é exceder o tempo limite devido a problemas com o código do serviço que impede o correto fechar das réplicas. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-report-health"></a>sfctl réplica relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento da réplica de Service Fabric.

Relatórios de estado de funcionamento da réplica de Service Fabric especificada. O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um gateway de Service Fabric réplica, que reencaminha para o arquivo de estado de funcionamento. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, verifique se o relatório é apresentada na secção de eventos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -propriedade de estado de funcionamento [necessário] | A propriedade das informações de estado de funcionamento. <br><br> Uma entidade pode ter relatórios de estado de funcionamento para as diferentes propriedades. A propriedade é uma cadeia e não uma enumeração fixa para permitir a flexibilidade de relatório para categorizar a condição de estado que aciona o relatório. Por exemplo, um relatório com SourceId "LocalWatchdog" pode monitorizar o estado dos discos disponíveis no nó, pelo que pode comunicar "AvailableDisk" propriedade nesse nó. O relatório mesmo pode monitorizar a conectividade de nó, pelo que pode reportar uma propriedade "Conectividade" no mesmo nó. No arquivo de estado de funcionamento, estes relatórios são tratados como eventos de estado de funcionamento separada para o nó especificado. Juntamente com o SourceId, a propriedade identifica de forma exclusiva as informações de estado de funcionamento. |
| -Estado de funcionamento [necessário] | Os valores possíveis incluem\: 'Inválido', 'Ok', 'Aviso', 'Error', 'Desconhecido'. |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | A identidade da partição. |
| – id de origem [necessário] | O nome de origem que identifica o componente de sistema/watchdog do/cliente gerado as informações de estado de funcionamento. |
| – Descrição | A descrição das informações de estado de funcionamento. <br><br> Representa texto livre utilizado para adicionar informações legíveis humanos sobre o relatório. O comprimento de máximo de cadeia para a descrição é 4096 carateres. Se a cadeia fornecida é mais longa, este será automaticamente truncado. Quando truncado, os últimos carateres da descrição contêm um marcador "[Truncated]" e o tamanho total da cadeia é 4096 carateres. A presença do marcador indica aos utilizadores que truncagem ocorreu. Tenha em atenção que quando truncado, a descrição tem menos de 4096 carateres da cadeia original. |
| -imediata | Um sinalizador que indica se o relatório deve ser enviado imediatamente. <br><br> Um relatório de estado de funcionamento é enviado para um gateway de Service Fabric aplicação, que reencaminha para o arquivo de estado de funcionamento. Se Immediate estiver definido como VERDADEIRO, o relatório é enviado imediatamente do Gateway de HTTP para o arquivo de estado de funcionamento, independentemente das definições de cliente de recursos de infraestrutura que está a utilizar a aplicação de Gateway de HTTP. Isto é útil para relatórios críticos que devem ser enviados logo que possível. Dependendo de temporização precisa e outras condições, enviar o relatório poderá ainda falhar, por exemplo, se o Gateway de HTTP está fechado ou a mensagem não aceder ao Gateway. Se Immediate estiver definido como false, o relatório é enviado com base nas definições de cliente do Estado de funcionamento do Gateway de HTTP. Por conseguinte, esta será possível criar batch, de acordo com a configuração de HealthReportSendInterval. Esta é a definição recomendada porque permite que o cliente do Estado de funcionamento otimizar o relatório de mensagens para o arquivo de estado de funcionamento, bem como o processamento do relatório de estado de funcionamento do Estado de funcionamento. Por predefinição, relatórios não são enviados imediatamente. |
| -remover quando expirou | Valor que indica se o relatório é removido do arquivo de estado de funcionamento, quando este expirar. <br><br> Se definido como true, o relatório é removido do arquivo de estado de funcionamento, depois de expirar. Se definido como false, o relatório é tratado como um erro quando expirado. O valor desta propriedade é falso por predefinição. Quando os clientes reportem periodicamente, deve definir o RemoveWhenExpired false (predefinição). Desta forma, é o relatório tem problemas (por exemplo, impasse) e não pode reportar a entidade é avaliada em erro quando expira o relatório de estado de funcionamento. Este processo sinaliza a entidade como estando em estado de funcionamento de erro. |
| -número de sequência | O número de sequência para este relatório de estado de funcionamento como uma cadeia de um valor numérico. <br><br> O número de sequência de relatório é utilizado pelo arquivo de estado de funcionamento para detetar relatórios obsoletos. Se não for especificado, um número de sequência é gerada automaticamente pelo cliente do Estado de funcionamento quando é adicionado um relatório. |
| -tipo de serviço | O tipo de réplica de serviço (sem monitorização de estado ou com monitorização de estado) para o qual o estado de funcionamento está a ser reportado. Seguem-se os valores possíveis\: 'Sem monitorização de estado', 'Monitorização de estado'.  Predefinição\: com monitorização de estado. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| – ttl | A duração para o qual este relatório de estado de funcionamento é válido. Este campo está a utilizar o formato ISO8601 para especificar a duração. <br><br> Quando os clientes reportem periodicamente, estes devem enviar relatórios com maior frequência que TTL. Se os clientes comunicam em transição, pode definir o tempo TTL para infinito. Quando expira o TTL, o evento de estado de funcionamento que contém as informações de estado de funcionamento é encontra-se removido do arquivo de estado de funcionamento, se RemoveWhenExpired for true, ou avaliadas em erro, se RemoveWhenExpired false. Se não for especificado, tempo TTL por predefinição, o valor de infinito. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-replica-restart"></a>reinício de réplica sfctl
Reinicia uma réplica de serviço de um serviço persistente em execução num nó.

Reinicia uma réplica de serviço de um serviço persistente em execução num nó. Aviso - daí serão efetuadas verificações de segurança efetuadas quando esta API é utilizada. Utilização incorreta desta API pode levar a perda de disponibilidade para serviços com monitorização de estado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| – id de partição [necessário] | A identidade da partição. |
| – id de réplica [necessário] | O identificador da réplica. |
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
- [A configuração](service-fabric-cli.md) os recursos de infraestrutura do serviço CLI.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).
