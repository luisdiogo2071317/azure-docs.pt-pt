---
title: Cluster do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de cluster do Service Fabric CLI sfctl.
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
ms.openlocfilehash: 60f3f74778f0fb32677c3b87b3140131ccd37bea
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763634"
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecione, gerir e operar clusters de Service Fabric.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| versões de código | Obtém uma lista dos recursos de infraestrutura versões de código aprovisionados num cluster de Service Fabric. |
| versões de configuração | Obtém uma lista dos recursos de infraestrutura versões de configuração que são aprovisionadas num cluster de Service Fabric. |
| estado de funcionamento | Obtém o estado de funcionamento de um cluster do Service Fabric. |
| Manifesto | Obter o manifesto do cluster de Service Fabric. |
| cancelamento da operação | Cancela uma operação do índice de falhas induzida pelo utilizador. |
| lista de operações | Obtém uma lista de operações do índice de falhas induzida pelo utilizador filtrados pela entrada fornecida. |
| Aprovisionar | Aprovisione os pacotes de código ou de configuração de um cluster do Service Fabric. |
| recover-system | Indica, para o cluster do Service Fabric, o que deve tentar recuperar os serviços de sistema que atualmente estão bloqueados na perda de quórum. |
| report-health | Envia um relatório de estado de funcionamento no cluster de Service Fabric. |
| seleccionar | Liga a um ponto final de cluster de Service Fabric. |
| Não aprovisionamento | Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric. |
| atualização | Comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric. |
| retoma de atualização | Efetuar a atualização do cluster avançar para o domínio de atualização seguinte. |
| reversão de atualização | Reverta a atualização de um cluster do Service Fabric. |
| Estado de atualização | Obtém o progresso da atualização do cluster atual. |
| atualização-atualização | Atualize os parâmetros de atualização de uma atualização de cluster do Service Fabric. |

## <a name="sfctl-cluster-code-versions"></a>versões de código do cluster sfctl
Obtém uma lista dos recursos de infraestrutura versões de código aprovisionados num cluster de Service Fabric.

Obtém uma lista de informações sobre recursos de infraestrutura versões de código que sejam aprovisionadas no cluster. O parâmetro CodeVersion pode ser utilizado para filtrar, opcionalmente, a saída para apenas essa versão específica.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -versão de código | A versão de produto do Service Fabric. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-config-versions"></a>versões de configuração do cluster sfctl
Obtém uma lista dos recursos de infraestrutura versões de configuração que são aprovisionadas num cluster de Service Fabric.

Obtém uma lista de informações sobre recursos de infraestrutura versões de configuração que são aprovisionadas no cluster. O parâmetro ConfigVersion pode ser utilizado para filtrar, opcionalmente, a saída para apenas essa versão específica.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -config-versão | A versão de configuração do Service Fabric. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-health"></a>o estado de funcionamento do sfctl cluster
Obtém o estado de funcionamento de um cluster do Service Fabric.

Obtém o estado de funcionamento de um cluster do Service Fabric. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicado no cluster com base no estado de funcionamento. Da mesma forma, utilize NodesHealthStateFilter e ApplicationsHealthStateFilter para filtrar a coleção de nós e aplicações devolvidas com base no respetivo estado de funcionamento agregada.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – aplicações-estado de funcionamento do Estado do filtro | Permite a filtragem dos objetos de estado de funcionamento da aplicação devolvidos nos resultados da consulta de estado de funcionamento de cluster com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem obtido a partir membros ou operações bit a bit em membros de enumeração de HealthStateFilter de valor de número inteiro. Apenas as aplicações que correspondem ao filtro são devolvidas. Todas as aplicações são utilizadas para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento das aplicações com o valor HealthState OK (2) e de aviso (4) são devolvidas.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| – estatísticas de estado de funcionamento de exclusão | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro. |
| – incluir-sistema--estado de funcionamento-das estatísticas da aplicação | Indica se as estatísticas de estado de funcionamento devem incluir os recursos de infraestrutura\:estatísticas de estado de funcionamento da aplicação de /System. FALSO por predefinição. Se IncludeSystemApplicationHealthStatistics estiver definido como VERDADEIRO, o estado de funcionamento estatísticas incluem as entidades que pertencem aos recursos de infraestrutura\:aplicação /System. Caso contrário, o resultado da consulta inclui estatísticas de estado de funcionamento apenas para aplicações do utilizador. As estatísticas de estado de funcionamento têm de ser incluídas no resultado da consulta para este parâmetro seja aplicada. |
| --nodes-health-state-filter | Permite a filtragem de objetos de estado de funcionamento nó devolvidos nos resultados da consulta de estado de funcionamento de cluster com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas nós que correspondem ao filtro são devolvidos. Todos os nós são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento de nós com o valor HealthState OK (2) e de aviso (4) são devolvidas.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-manifest"></a>manifesto do cluster sfctl
Obter o manifesto do cluster de Service Fabric.

Obter o manifesto do cluster de Service Fabric. O manifesto do cluster contém as propriedades do cluster, que incluem os tipos de outro nó no cluster, configurações de segurança, falhas e topologias de domínio de atualização, etc. Estas propriedades são especificadas como parte do ficheiro Clusterconfig durante a implementação de um cluster autónomo. No entanto, a maioria das informações no manifesto do cluster é gerado internamente pelo recurso de infraestrutura de serviço durante a implementação de cluster nos outros cenários de implementação (por exemplo, quando utilizar o portal do Azure). O conteúdo do manifesto do cluster é apenas para fins informativos e os utilizadores não são esperados para colocar uma dependência no formato do conteúdo do ficheiro ou a interpretação.

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

## <a name="sfctl-cluster-operation-cancel"></a>operação de cluster sfctl-Cancelar
Cancela uma operação do índice de falhas induzida pelo utilizador.

As APIs seguintes iniciar as operações de índice de falhas que podem ser canceladas utilizando CancelOperation: StartDataLoss, StartQuorumLoss, StartPartitionRestart, StartNodeTransition. Se forçar for FALSO, em seguida, a operação especificada induzida pelo utilizador será transições parada e limpa.  Se forçar for VERDADEIRO, o comando será abortado e algumas estado interno pode ficar.  Especificação de imposição como verdadeiro deve ser utilizada com cuidado. Não é permitida a chamar esta API force definido como verdadeiro até que esta API já foi chamada no mesmo comando de teste com o conjunto de imposição primeiro FALSO, ou, a menos que o comando de teste já tem um OperationState OperationState.RollingBack. 

Esclarecimentos\: OperationState.RollingBack significa que o sistema será/limpeza de sistema interno provocado por executar o comando de estado. Não irá restaurar dados se o comando de teste foi causar a perda de dados.  Por exemplo, se chamar StartDataLoss chamar esta API, o sistema irá apenas Limpar estado interno ao executar o comando. Não irá restaurar dados a partição de destino, se o comando progredido suficiente para causar a perda de dados. 

> [!NOTE]
> Se esta API é invocada com force = = true, interno de estado pode ficar.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de operação [necessário] | Um GUID que identifica uma chamada desta API.  Isto é transmitido para a API de GetProgress correspondente. |
| -force | Indica se devem ser corretamente reverter e limpar o estado de sistema interno modificado por ao executar a operação induzida pelo utilizador. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-operation-list"></a>lista de operações do cluster sfctl
Obtém uma lista de operações do índice de falhas induzida pelo utilizador filtrados pela entrada fornecida.

Obtém a lista de operações do índice de falhas induzida pelo utilizador filtrados pela entrada fornecida.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -filtro de estado | Utilizado para filtrar do OperationState para operações de induzida pelo utilizador. <br> 65535 - Selecionar tudo <br> 1 - selecionar em execução <br> 2 - selecionar RollingBack <br>8 - selecionar concluído <br>16 - selecionar falhado <br>32 - selecionar cancelada <br>64 - selecione ForceCancelled.  <br>Predefinição\: 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -tipo de filtro | Utilizado para filtrar OperationType para operações de induzida pelo utilizador. <br> 65535 - Selecionar tudo <br> 1 - Selecione PartitionDataLoss. <br> 2 - selecione PartitionQuorumLoss. <br> 4 - selecione PartitionRestart. <br> 8 - selecione NodeTransition.  <br> Predefinição\: 65535. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-provision"></a>Aprovisionamento de cluster sfctl
Aprovisione os pacotes de código ou de configuração de um cluster do Service Fabric.

Valide e aprovisionar os pacotes de código ou de configuração de um cluster do Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -cluster-manifesto--caminho do ficheiro | O caminho de ficheiro de manifesto do cluster. |
| – o caminho de ficheiro de código | Caminho de ficheiro de pacote do código de cluster. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-recover-system"></a>sistema de recuperação do cluster sfctl
Indica, para o cluster do Service Fabric, o que deve tentar recuperar os serviços de sistema que atualmente estão bloqueados na perda de quórum.

Indica, para o cluster do Service Fabric, o que deve tentar recuperar os serviços de sistema que atualmente estão bloqueados na perda de quórum. Esta operação só deve ser efetuada se sabe-se que não não possível recuperar as réplicas que estão em baixo. Utilização incorreta desta API pode causar a potencial perda de dados.

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

## <a name="sfctl-cluster-report-health"></a>sfctl cluster relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento no cluster de Service Fabric.

O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um nó de gateway do Service Fabric, que reencaminha para o arquivo de estado de funcionamento. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, verifique se o relatório aparece no HealthEvents do cluster.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -propriedade de estado de funcionamento [necessário] | A propriedade das informações de estado de funcionamento. <br><br> Uma entidade pode ter relatórios de estado de funcionamento para as diferentes propriedades. A propriedade é uma cadeia e não uma enumeração fixa para permitir a flexibilidade de relatório para categorizar a condição de estado que aciona o relatório. Por exemplo, um relatório com SourceId "LocalWatchdog" pode monitorizar o estado dos discos disponíveis no nó, pelo que pode comunicar "AvailableDisk" propriedade nesse nó. O relatório mesmo pode monitorizar a conectividade de nó, pelo que pode reportar uma propriedade "Conectividade" no mesmo nó. No arquivo de estado de funcionamento, estes relatórios são tratados como eventos de estado de funcionamento separada para o nó especificado. Juntamente com o SourceId, a propriedade identifica de forma exclusiva as informações de estado de funcionamento. |
| -Estado de funcionamento [necessário] | Os valores possíveis incluem\: 'Inválido', 'Ok', 'Aviso', 'Error', 'Desconhecido'. |
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

## <a name="sfctl-cluster-select"></a>Selecione de cluster sfctl
Liga a um ponto final de cluster de Service Fabric.

Se ligar ao cluster segura, especifique um caminho absoluto para um certificado (. crt) e o ficheiro de chave (.key) ou um único ficheiro com ambos os (. pem). Não especifique ambos. Opcionalmente, de se ligar a um cluster seguro, também especifica um caminho absoluto para um ficheiro de pacote de AC ou o diretório de certificados de AC fidedignos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -ponto final [necessário] | URL de ponto final, incluindo a porta e o prefixo HTTP ou HTTPS de cluster. |
| – aad | Utilize o Azure Active Directory para autenticação. |
| --ca | Caminho absoluto para o diretório de certificados de AC para tratar como válido ou um ficheiro de pacote de AC. |
| -certificados | Caminho absoluto para um ficheiro de certificado de cliente. |
| -chave | Caminho absoluto para o ficheiro de chave do certificado de cliente. |
| -Certifique-se de não | Desativar a verificação de certificados quando utilizar HTTPS, tenha em atenção\: esta é uma opção inseguras e não deve ser utilizada para ambientes de produção. |
| -pem | Caminho absoluto para o certificado de cliente, como um ficheiro. pem. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-unprovision"></a>não aprovisionamento de cluster sfctl
Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric.

Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric. É possível anular o aprovisionamento de código e a configuração em separado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -versão de código | A versão de pacote do código de cluster. |
| -config-versão | O versão do manifesto do cluster. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-upgrade"></a>atualização do cluster sfctl
Comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric.

Valide os parâmetros de atualização fornecidos e comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --app-health-map | JSON codificado dicionário de pares de nome de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro. |
| aplicações--tipo-estado de funcionamento-mapa | JSON codificado dicionário de pares de nome de tipo de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro. |
| -versão de código | A versão de código de cluster. |
| -config-versão | A versão de configuração de cluster. |
| --delta-health-evaluation | Permite a avaliação de estado de funcionamento de diferenças em vez de avaliação de estado de funcionamento absoluto após a conclusão de cada domínio de atualização. |
| --delta-unhealthy-nodes | O máximo permitido percentagem de nós degradação de estado de funcionamento permitida durante as atualizações de cluster.  Predefinição\: 10. <br><br> O delta é medido entre o estado de nós no início da atualização e o estado de nós no momento da avaliação de estado de funcionamento. A verificação é executada após a conclusão de atualização cada domínio de atualização para se certificar de que o estado global do cluster está dentro dos limites tolerated. |
| -Falha de ação | Os valores possíveis incluem\: 'Inválido,' 'Reversão', 'Manual'. |
| -force-reinício | Força o reinício. |
| --health-check-retry | Tempo limite de tentativas de verificação do Estado de funcionamento é medido em milissegundos. |
| -estável estado de funcionamento-verificação | Estado de funcionamento Consulte duração estável, medida em milissegundos. |
| -espera de verificação de estado de funcionamento | Duração de espera de verificação do Estado de funcionamento é medido em milissegundos. |
| --replica-set-check-timeout | Réplica atualização definir um tempo limite de verificação medido em segundos. |
| -implementar atualização-modo | Os valores possíveis incluem\: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorizado'.  Predefinição\: UnmonitoredAuto. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -mau estado de funcionamento de aplicações | O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. <br><br> Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster, excluindo as aplicações de tipos de aplicação que estão incluídas no ApplicationTypeHealthPolicyMap. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de aplicações. |
| -mau estado de funcionamento de nós | O máximo permitido de percentagem de nós mau estado de funcionamento antes de o comunicar um erro. <br><br> Por exemplo, para permitir a 10% de nós incorreto, este valor será 10. A percentagem representa a percentagem máxima de tolerated de nós que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos um nó de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. A percentagem é calculada dividindo o número de nós mau estado de funcionamento e o número total de nós no cluster. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de nós. Em grandes clusters, alguns nós sempre será baixo ou horizontalmente para reparações, pelo que esta percentagem deve ser configurada para tolerar que. |
| --upgrade-domain-delta-unhealthy-nodes | O máximo permitido percentagem de nós de domínio de atualização de degradação de estado de funcionamento permitida durante as atualizações de cluster.  Predefinição\: 15. <br><br> O delta é medido entre o estado de nós do domínio de atualização no início da atualização e o estado de nós do domínio de atualização no momento da avaliação de estado de funcionamento. A verificação é efetuada uma vez concluída cada conclusão de atualização do domínio de atualização para todos os domínios de atualização para se certificar de que o estado dos domínios de atualização está dentro dos limites tolerated. |
| -atualização--tempo limite do domínio | Tempo limite do domínio de atualização é medido em milissegundos. |
| -tempo limite de atualização | Tempo limite de atualização é medido em milissegundos. |
| --warning-as-error | Avisos são tratados com a mesma gravidade como erros. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-upgrade-resume"></a>atualização de cluster sfctl-retomar
Efetuar a atualização do cluster avançar para o domínio de atualização seguinte.

Efetue a cluster configuração ou de código de atualização movimentação no domínio de atualização seguinte se apropriado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -domínio de atualização [necessário] | O domínio de atualização seguinte para esta atualização de cluster. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-cluster-upgrade-rollback"></a>reversão de atualização do cluster sfctl
Reverta a atualização de um cluster do Service Fabric.

Reverta a atualização de código ou de configuração de um cluster do Service Fabric.

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

## <a name="sfctl-cluster-upgrade-status"></a>Estado de atualização do cluster sfctl
Obtém o progresso da atualização do cluster atual.

Obtém o progresso atual da atualização do cluster em curso. Se a atualização não está atualmente em curso, obter o último Estado da atualização do cluster anterior.

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

## <a name="sfctl-cluster-upgrade-update"></a>cluster de sfctl atualização-atualização
Atualize os parâmetros de atualização de uma atualização de cluster do Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --app-health-map | JSON codificado dicionário de pares de nome de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro. |
| aplicações--tipo-estado de funcionamento-mapa | JSON codificado dicionário de pares de nome de tipo de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro. |
| --delta-health-evaluation | Permite a avaliação de estado de funcionamento de diferenças em vez de avaliação de estado de funcionamento absoluto após a conclusão de cada domínio de atualização. |
| --delta-unhealthy-nodes | O máximo permitido percentagem de nós degradação de estado de funcionamento permitida durante as atualizações de cluster.  Predefinição\: 10. <br><br> O delta é medido entre o estado de nós no início da atualização e o estado de nós no momento da avaliação de estado de funcionamento. A verificação é executada após a conclusão de atualização cada domínio de atualização para se certificar de que o estado global do cluster está dentro dos limites tolerated. |
| -Falha de ação | Os valores possíveis incluem\: 'Inválido,' 'Reversão', 'Manual'. |
| -force-reinício | Força o reinício. |
| --health-check-retry | Tempo limite de tentativas de verificação do Estado de funcionamento é medido em milissegundos. |
| -estável estado de funcionamento-verificação | Estado de funcionamento Consulte duração estável, medida em milissegundos. |
| -espera de verificação de estado de funcionamento | Duração de espera de verificação do Estado de funcionamento é medido em milissegundos. |
| --replica-set-check-timeout | Réplica atualização definir um tempo limite de verificação medido em segundos. |
| -implementar atualização-modo | Os valores possíveis incluem\: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorizado'.  Predefinição\: UnmonitoredAuto. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -mau estado de funcionamento de aplicações | O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. <br><br> Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster, excluindo as aplicações de tipos de aplicação que estão incluídas no ApplicationTypeHealthPolicyMap. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de aplicações. |
| -mau estado de funcionamento de nós | O máximo permitido de percentagem de nós mau estado de funcionamento antes de o comunicar um erro. <br><br> Por exemplo, para permitir a 10% de nós incorreto, este valor será 10. A percentagem representa a percentagem máxima de tolerated de nós que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos um nó de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. A percentagem é calculada dividindo o número de nós mau estado de funcionamento e o número total de nós no cluster. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de nós. Em grandes clusters, alguns nós sempre será baixo ou horizontalmente para reparações, pelo que esta percentagem deve ser configurada para tolerar que. |
| --upgrade-domain-delta-unhealthy-nodes | O máximo permitido percentagem de nós de domínio de atualização de degradação de estado de funcionamento permitida durante as atualizações de cluster.  Predefinição\: 15. <br><br> O delta é medido entre o estado de nós do domínio de atualização no início da atualização e o estado de nós do domínio de atualização no momento da avaliação de estado de funcionamento. A verificação é efetuada uma vez concluída cada conclusão de atualização do domínio de atualização para todos os domínios de atualização para se certificar de que o estado dos domínios de atualização está dentro dos limites tolerated. |
| -atualização--tempo limite do domínio | Tempo limite do domínio de atualização é medido em milissegundos. |
| -tipo de atualização | Os valores possíveis incluem\: 'Inválido', 'Sucessiva', 'Rolling_ForceRestart'.  Predefinição\: sucessiva. |
| -tempo limite de atualização | Tempo limite de atualização é medido em milissegundos. |
| --warning-as-error | Avisos são tratados com a mesma gravidade como erros. |

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