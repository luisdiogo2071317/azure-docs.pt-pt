---
title: Azure Service Fabric CLI sfctl nós | Microsoft Docs
description: Descreve os comandos de nó de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: fb8a310a131938e95f3d21b3962dbbd1944a57ed
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763430"
---
# <a name="sfctl-node"></a>sfctl node
Gerir os nós que formam um cluster.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| desactivar | Desative um nó de cluster do Service Fabric com a intenção de Desativação especificada. |
| ativar | Ative um nó de cluster do Service Fabric está atualmente desativado. |
| estado de funcionamento | Obtém o estado de funcionamento de um nó de Service Fabric. |
| informações | Obtém as informações sobre um nó específico no cluster de Service Fabric. |
| lista | Obtém a lista de nós no cluster de Service Fabric. |
| carregar | Obtém as informações de carga de um nó de Service Fabric. |
| remoção de estado | Notifica o Service Fabric que o estado persistente num nó foi permanentemente removido ou perdido. |
| report-health | Envia um relatório de estado de funcionamento no nó de Service Fabric. |
| Reiniciar | Reinicia um nó de cluster do Service Fabric. |
| transição | Inicia ou para um nó de cluster. |
| Estado de transição | Obtém o progresso de uma operação iniciado utilizando StartNodeTransition. |

## <a name="sfctl-node-disable"></a>desativação do nó de sfctl
Desative um nó de cluster do Service Fabric com a intenção de Desativação especificada.

Desative um nó de cluster do Service Fabric com a intenção de Desativação especificada. Assim que a desativação estiver em curso, a intenção de Desativação pode ser aumentada, mas não diminuída (por exemplo, um nó que está desativado com a tentativa de colocar em pausa pode ser desativado ainda mais com reinício, mas não o inverso. Podem ser reativados nós utilizando a ativar uma operação de nó qualquer altura depois de estes são desativadas. Se a desativação não estiver concluída, isto cancelará a desativação. Um nó que fica inativo e volta a funcionar enquanto desativada têm de ser reativados antes de serviços serão colocados nesse nó.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| --deactivation-intent | Descreve o motivo para desativar o nó ou intenção. Os valores possíveis são seguintes. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-enable"></a>Ativar de nó sfctl
Ative um nó de cluster do Service Fabric está atualmente desativado.

Ativa um nó de cluster do Service Fabric está atualmente desativado. Depois de ativado, o nó novamente ficará um destino viável para colocação de novo réplicas, e irão ser reativadas qualquer réplicas desativadas restante no nó.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-health"></a>Estado de funcionamento de nó de sfctl
Obtém o estado de funcionamento de um nó de Service Fabric.

Obtém o estado de funcionamento de um nó de Service Fabric. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicado no nó com base no estado de funcionamento. Se o nó que especificou, o nome não existe no arquivo de estado de funcionamento, esta ação devolve um erro.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
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

## <a name="sfctl-node-info"></a>informações do nó de sfctl
Obtém as informações sobre um nó específico no cluster de Service Fabric.

Obtém as informações sobre um nó específico num Cluster do Service Fabric. A resposta inclui o nome, estado, ID, estado de funcionamento, tempo de atividade e outros detalhes sobre o nó.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-list"></a>lista de nós sfctl
Obtém a lista de nós no cluster de Service Fabric.

Obtém a lista de nós no cluster de Service Fabric. A resposta inclui o nome, estado, ID, estado de funcionamento, tempo de atividade e outros detalhes sobre o nó.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| --node-status-filter | Permite a filtragem de nós com base na NodeStatus. Apenas os nós que são de correspondência do valor de filtro especificado serão devolvidos. O valor do filtro pode ser um dos seguintes procedimentos.  Predefinição\: predefinido. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-load"></a>carga de nó sfctl
Obtém as informações de carga de um nó de Service Fabric.

Obtém as informações de carga de um nó de Service Fabric para todas as métricas que têm de carga ou a capacidade definido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-remove-state"></a>sfctl remove-estado do nó
Notifica o Service Fabric que o estado persistente num nó foi permanentemente removido ou perdido.

Notifica o Service Fabric que o estado persistente num nó foi permanentemente removido ou perdido.  Isto implica que não é possível recuperar o estado persistente desse nó. Isto acontece normalmente se um disco rígido foi limpo limpo ou se a falhas de um disco rígido. O nó tem de ser desativado para esta operação seja concluída com êxito. Esta operação permite saber que as réplicas nesse nó já não existem e que o Service Fabric deverá ser interrompida aguardar esses réplicas voltar atrás cópias de segurança do Service Fabric. Não execute este cmdlet se o estado no nó não foi removido e o nó pode voltar atrás cópias de segurança com o respetivo estado intacto.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-report-health"></a>sfctl nó relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento no nó de Service Fabric.

Relatórios de estado de funcionamento do nó de Service Fabric especificado. O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um nó de gateway do Service Fabric, que reencaminha para o arquivo de estado de funcionamento. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, verifique se o relatório é apresentada na secção HealthEvents.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -propriedade de estado de funcionamento [necessário] | A propriedade das informações de estado de funcionamento. <br><br> Uma entidade pode ter relatórios de estado de funcionamento para as diferentes propriedades. A propriedade é uma cadeia e não uma enumeração fixa para permitir a flexibilidade de relatório para categorizar a condição de estado que aciona o relatório. Por exemplo, um relatório com SourceId "LocalWatchdog" pode monitorizar o estado dos discos disponíveis no nó, pelo que pode comunicar "AvailableDisk" propriedade nesse nó. O relatório mesmo pode monitorizar a conectividade de nó, pelo que pode reportar uma propriedade "Conectividade" no mesmo nó. No arquivo de estado de funcionamento, estes relatórios são tratados como eventos de estado de funcionamento separada para o nó especificado. Juntamente com o SourceId, a propriedade identifica de forma exclusiva as informações de estado de funcionamento. |
| -Estado de funcionamento [necessário] | Os valores possíveis incluem\: 'Inválido', 'Ok', 'Aviso', 'Error', 'Desconhecido'. |
| --nome do nó [necessário] | Nome do nó para elaborar relatórios sobre. |
| – id de origem [necessário] | O nome de origem que identifica o componente de sistema/watchdog do/cliente gerado as informações de estado de funcionamento. |
| – Descrição | A descrição das informações de estado de funcionamento. <br><br> Representa texto livre utilizado para adicionar informações legíveis humanos sobre o relatório. O comprimento de máximo de cadeia para a descrição é 4096 carateres. Se a cadeia fornecida é mais longa, este será automaticamente truncado. Quando truncado, os últimos carateres da descrição contêm um marcador "[Truncated]" e o tamanho total da cadeia é 4096 carateres. A presença do marcador indica aos utilizadores que truncagem ocorreu. Tenha em atenção que quando truncado, a descrição tem menos de 4096 carateres da cadeia original. |
| -imediata | Um sinalizador que indica se o relatório deve ser enviado imediatamente. <br><br> Um relatório de estado de funcionamento é enviado para um gateway de Service Fabric aplicação, que reencaminha para o arquivo de estado de funcionamento. Se Immediate estiver definido como VERDADEIRO, o relatório é enviado imediatamente do Gateway de HTTP para o arquivo de estado de funcionamento, independentemente das definições de cliente de recursos de infraestrutura que está a utilizar a aplicação de Gateway de HTTP. Isto é útil para relatórios críticos que devem ser enviados logo que possível. Dependendo de temporização precisa e outras condições, enviar o relatório poderá ainda falhar, por exemplo, se o Gateway de HTTP está fechado ou a mensagem não aceder ao Gateway. Se Immediate estiver definido como false, o relatório é enviado com base nas definições de cliente do Estado de funcionamento do Gateway de HTTP. Por conseguinte, esta será possível criar batch, de acordo com a configuração de HealthReportSendInterval. Esta é a definição recomendada porque permite que o cliente do Estado de funcionamento otimizar o relatório de mensagens para o arquivo de estado de funcionamento, bem como o processamento do relatório de estado de funcionamento do Estado de funcionamento. Por predefinição, relatórios não são enviados imediatamente. |
| -remover quando expirou | Valor que indica se o relatório é removido do arquivo de estado de funcionamento, quando este expirar. <br><br> Se definido como true, o relatório é removido do arquivo de estado de funcionamento, depois de expirar. Se definido como false, o relatório é tratado como um erro quando expirado. O valor desta propriedade é falso por predefinição. Quando os clientes reportem periodicamente, deve definir o RemoveWhenExpired false (predefinição). Desta forma, é o relatório tem problemas (por exemplo, impasse) e não pode reportar a entidade é avaliada em erro quando expira o relatório de estado de funcionamento. Este processo sinaliza a entidade como estando em estado de funcionamento de erro. |
| -número de sequência | O número de sequência para este relatório de estado de funcionamento como uma cadeia de um valor numérico. <br><br> O número de sequência de relatório é utilizado pelo arquivo de estado de funcionamento para detetar relatórios obsoletos. Se não for especificado, um número de sequência é gerada automaticamente pelo cliente do Estado de funcionamento quando é adicionado um relatório. |
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

## <a name="sfctl-node-restart"></a>reiniciar o nó sfctl
Reinicia um nó de cluster do Service Fabric.

Reinicia um nó de cluster do Service Fabric já foi iniciado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| -criar-recursos de infraestrutura-captura | Especifica verdadeiro para criar uma captura do processo de nó de recursos de infraestrutura. Esta é a maiúsculas e minúsculas.  Predefinição\: FALSO. |
| --node-instance-id | O ID de instância do nó de destino. Se o ID de instância for especificado o nó é reiniciado apenas se corresponder com a instância atual do nó. Um valor predefinido de "0" corresponderia a qualquer ID de instância. O ID de instância pode ser obtido utilizando a consulta do nó de get.  Predefinição\: 0. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-transition"></a>transição de nó sfctl
Inicia ou para um nó de cluster.

Inicia ou para um nó de cluster.  Um nó de cluster é um processo, não a instância de SO em si.  Para iniciar um nó, transmita "Start" para o parâmetro NodeTransitionType. Para parar um nó, transmita "Parar de" para o parâmetro NodeTransitionType. Esta API começa a operação - quando a API devolve que o nó poderá não terminar em transição ainda. Chame GetNodeTransitionProgress com o mesmo OperationId para obter o progresso da operação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de instância de nó [necessário] | O ID de instância de nó do nó de destino. Isto pode ser determinado através da API de GetNodeInfo. |
| --nome do nó [necessário] | O nome do nó. |
| -tipo de transição de nó [necessário] | Indica o tipo de transição para executar.  NodeTransitionType.Start irá iniciar um nó de paragem. NodeTransitionType.Stop irá parar um nó que está a funcionar. |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| -stop-duração-na-segundos [necessário] | A duração, em segundos, para manter o nó parada.  O valor mínimo é 600, o número máximo é 14400.  Após este período de tempo expira, o nó automaticamente regressará cópias de segurança. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-node-transition-status"></a>Estado de transição do nó sfctl
Obtém o progresso de uma operação iniciado utilizando StartNodeTransition.

Obtém o progresso de uma operação foi iniciada com StartNodeTransition utilizando o OperationId fornecido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
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