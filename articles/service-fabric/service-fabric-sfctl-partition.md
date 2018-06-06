---
title: Azure Service Fabric CLI sfctl partição | Microsoft Docs
description: Descreve os comandos de partição de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: a9455683c5fad7fad4dda62fd967da617d8a8496
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763651"
---
# <a name="sfctl-partition"></a>sfctl partition
Consultar e gerir partições para qualquer serviço.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| perda de dados | Esta API será induce perda de dados para a partição especificada. |
| data-loss-status | Obtém o progresso de uma operação de perda de dados de partição iniciado utilizando a API de StartDataLoss. |
| estado de funcionamento | Obtém o estado de funcionamento a partição de serviço recursos de infraestrutura especificada. |
| informações | Obtém as informações sobre uma partição de Service Fabric. |
| lista | Obtém a lista de partições de um serviço do Service Fabric. |
| carregar | Obtém as informações de carga da partição de serviço recursos de infraestrutura especificada. |
| reposição de carga | Repõe a atual carga de uma partição de Service Fabric. |
| perda de quórum | Induces perda de quórum para uma partição de serviço com estado indicado. |
| quorum-loss-status | Obtém o progresso de uma operação de perda de quórum numa partição iniciada utilizando a API de StartQuorumLoss. |
| Recuperar | Indica, para o cluster do Service Fabric, o que deve tentar recuperar uma partição específica que está atualmente bloqueada na perda de quórum. |
| recuperar-all | Indica, para o cluster do Service Fabric, o que deve tentar recuperar quaisquer serviços (incluindo os serviços do sistema), que atualmente estão bloqueados na perda de quórum. |
| report-health | Envia um relatório de estado de funcionamento na partição de Service Fabric. |
| Reiniciar | Esta API reiniciará algumas ou todas as réplicas ou instâncias da partição especificada. |
| restart-status | Obtém o progresso de uma operação de PartitionRestart iniciado utilizando StartPartitionRestart. |
| nome-SVC | Obtém o nome do serviço Service Fabric para uma partição. |

## <a name="sfctl-partition-data-loss"></a>sfctl partição-perda de dados
Esta API será induce perda de dados para a partição especificada.

Aciona uma chamada à API do OnDataLossAsync da partição.  Esta API será induce perda de dados para a partição especificada. Aciona uma chamada à API do OnDataLoss da partição. Perda de dados real depende o DataLossMode especificado. <br> PartialDataLoss - apenas um quórum de réplicas são removidos e é acionado OnDataLoss para a partição mas perda de dados real depende da presença de replicação em trânsito. <br>FullDataLoss - todas as réplicas são removidas, por conseguinte, todos os dados são perdidas e OnDataLoss é acionada. <br>Esta API só deve ser chamada com um serviço com monitorização de estado como destino. Não é aconselhada a chamar esta API com um serviço de sistema como destino. 
> [!NOTE]
> Depois de ter sido chamada esta API, não pode ser anulada. Chamar CancelOperation apenas parar a execução e limpar o estado de sistema interno. Não irá restaurar dados se o comando tem progredido suficiente para causar a perda de dados. Chame a API de GetDataLossProgress com o mesmo OperationId para devolver informações sobre a operação foi iniciada com esta API.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -modo de perda de dados [necessário] | Esta enumeração é transmitida para a API de StartDataLoss para indicar o tipo de perda de dados para induce. |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-data-loss-status"></a>sfctl partição-perda-estado de dados
Obtém o progresso de uma operação de perda de dados de partição iniciado utilizando a API de StartDataLoss.

Obtém o progresso de uma operação de perda de dados iniciada com StartDataLoss, utilizando o OperationId.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-health"></a>o estado de funcionamento do sfctl partição
Obtém o estado de funcionamento a partição de serviço recursos de infraestrutura especificada.

Obtém as informações de estado de funcionamento da partição especificada. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicadas falhas no serviço, com base no estado de funcionamento. Utilize ReplicasHealthStateFilter para filtrar a coleção de objetos de ReplicaHealthState na partição. Se especificar uma partição que não existe no arquivo de estado de funcionamento, este pedido devolve um erro.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| – estatísticas de estado de funcionamento de exclusão | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro. |
| --replicas-health-state-filter | Permite a filtragem da coleção de objetos de ReplicaHealthState na partição. Os membros ou operações bit a bit em membros de HealthStateFilter pode obter o valor. Apenas as réplicas que correspondem ao filtro vai ser devolvidas. Todas as réplicas serão utilizadas para avaliar o estado de funcionamento agregada. Se não for especificado, todas as entradas vai ser devolvidas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) vai ser devolvidos. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-info"></a>informações de partição sfctl
Obtém as informações sobre uma partição de Service Fabric.

Obtém as informações sobre a partição especificada. A resposta inclui o ID de partição, criação de partições informações de esquema, suportadas por partição, o estado, o estado de funcionamento e outros detalhes sobre a partição de chaves.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-list"></a>lista de partição sfctl
Obtém a lista de partições de um serviço do Service Fabric.

Obtém a lista de partições de um serviço do Service Fabric. A resposta inclui o ID de partição, criação de partições informações de esquema, suportadas por partição, o estado, o estado de funcionamento e outros detalhes sobre a partição de chaves.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
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

## <a name="sfctl-partition-load"></a>sfctl carga da partição
Obtém as informações de carga da partição de serviço recursos de infraestrutura especificada.

Devolve informações sobre a carga de uma partição especificada. A resposta inclui uma lista de relatórios de carga para uma partição de Service Fabric. Cada relatório inclui o nome da métrica de carga, o valor e a última vez que relatados em UTC.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-load-reset"></a>partição de sfctl reposição de carga
Repõe a atual carga de uma partição de Service Fabric.

Repõe a atual carga de uma partição de Service Fabric para a carga predefinido para o serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-quorum-loss"></a>sfctl partição-perda de quórum
Induces perda de quórum para uma partição de serviço com estado indicado.

Induces perda de quórum para uma partição de serviço com estado indicado.  Esta API é útil para uma situação de perda de quórum temporária no seu serviço. Chame a API de GetQuorumLossProgress com o mesmo OperationId para devolver informações sobre a operação foi iniciada com esta API. Só pode ser chamado na monitorização de estado persistente (HasPersistedState = = true) dos serviços.  Não utilize esta API serviços sem monitorização de estado ou com monitorização de estado de memória apenas os serviços.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| -duração de perda de quórum [necessário] | A quantidade de tempo para que a partição será mantida na perda de quórum.  Isto deve ser especificado em segundos. |
| -modo de perda de quórum [necessário] | Esta enumeração é transmitida para a API de StartQuorumLoss para indicar o tipo de perda de quórum para induce. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-quorum-loss-status"></a>partição de sfctl quórum-perda-status
Obtém o progresso de uma operação de perda de quórum numa partição iniciada utilizando a API de StartQuorumLoss.

Obtém o progresso de uma operação de perda de quórum começar StartQuorumLoss, utilizando o OperationId fornecido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-recover"></a>recuperar sfctl partição
Indica, para o cluster do Service Fabric, o que deve tentar recuperar uma partição específica que está atualmente bloqueada na perda de quórum.

Indica, para o cluster do Service Fabric, o que deve tentar recuperar uma partição específica que está atualmente bloqueada na perda de quórum. Esta operação só deve ser efetuada se sabe-se que não não possível recuperar as réplicas que estão em baixo. Utilização incorreta desta API pode causar a potencial perda de dados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-recover-all"></a>partição de sfctl recuperar-all
Indica, para o cluster do Service Fabric, o que deve tentar recuperar quaisquer serviços (incluindo os serviços do sistema), que atualmente estão bloqueados na perda de quórum.

Indica, para o cluster do Service Fabric, o que deve tentar recuperar quaisquer serviços (incluindo os serviços do sistema), que atualmente estão bloqueados na perda de quórum. Esta operação só deve ser efetuada se sabe-se que não não possível recuperar as réplicas que estão em baixo. Utilização incorreta desta API pode causar a potencial perda de dados.

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

## <a name="sfctl-partition-report-health"></a>sfctl partição relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento na partição de Service Fabric.

Relatórios de estado de funcionamento da partição de serviço recursos de infraestrutura especificada. O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um gateway de Service Fabric partição, o que encaminha para o arquivo de estado de funcionamento. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, verifique se o relatório é apresentada na secção de eventos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -propriedade de estado de funcionamento [necessário] | A propriedade das informações de estado de funcionamento. <br><br> Uma entidade pode ter relatórios de estado de funcionamento para as diferentes propriedades. A propriedade é uma cadeia e não uma enumeração fixa para permitir a flexibilidade de relatório para categorizar a condição de estado que aciona o relatório. Por exemplo, um relatório com SourceId "LocalWatchdog" pode monitorizar o estado dos discos disponíveis no nó, pelo que pode comunicar "AvailableDisk" propriedade nesse nó. O relatório mesmo pode monitorizar a conectividade de nó, pelo que pode reportar uma propriedade "Conectividade" no mesmo nó. No arquivo de estado de funcionamento, estes relatórios são tratados como eventos de estado de funcionamento separada para o nó especificado. Juntamente com o SourceId, a propriedade identifica de forma exclusiva as informações de estado de funcionamento. |
| -Estado de funcionamento [necessário] | Os valores possíveis incluem\: 'Inválido', 'Ok', 'Aviso', 'Error', 'Desconhecido'. |
| – id de partição [necessário] | A identidade da partição. |
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

## <a name="sfctl-partition-restart"></a>reinício de partição sfctl
Esta API reiniciará algumas ou todas as réplicas ou instâncias da partição especificada.

Esta API é útil para testar a ativação pós-falha. Se utilizados para visar uma partição de serviço sem monitorização de estado, RestartPartitionMode tem de ser AllReplicasOrInstances. Chame a API de GetPartitionRestartProgress utilizando o mesmo OperationId para obter o progresso.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| -modo de partição de reinício [necessário] | Descreva as partições a reiniciar. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-restart-status"></a>Estado de reinício do partição sfctl
Obtém o progresso de uma operação de PartitionRestart iniciado utilizando StartPartitionRestart.

Obtém o progresso de uma PartitionRestart começar StartPartitionRestart utilizando o OperationId fornecido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| – id de partição [necessário] | A identidade da partição. |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-partition-svc-name"></a>sfctl svc-nome de partição
Obtém o nome do serviço Service Fabric para uma partição.

Obtém o nome do serviço para a partição especificada. Um erro 404 é devolvido se o ID de partição não existe no cluster.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de partição [necessário] | A identidade da partição. |
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
