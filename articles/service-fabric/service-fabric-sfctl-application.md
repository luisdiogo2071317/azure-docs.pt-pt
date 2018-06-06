---
title: Aplicação Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de aplicação de sfctl CLI de recursos de infraestrutura de serviço.
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
ms.openlocfilehash: 3ecc5a03ff1847dc11c5a5047e35566a4e68fec2
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763702"
---
# <a name="sfctl-application"></a>sfctl application
Criar, eliminar e gerir aplicações e tipos de aplicações.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| criar | Cria uma aplicação de Service Fabric utilizando a descrição especificada. |
| eliminar | Elimina uma aplicação de Service Fabric existente. |
| Implementado | Obtém as informações sobre uma aplicação implementada num nó de Service Fabric. |
| Estado de funcionamento implementado | Obtém as informações sobre o estado de funcionamento de uma aplicação implementada num nó de Service Fabric. |
| lista implementado | Obtém a lista de aplicações implementadas num nó de Service Fabric. |
| estado de funcionamento | Obtém o estado de funcionamento da aplicação de recursos de infraestrutura de serviço. |
| informações | Obtém informações sobre uma aplicação de Service Fabric. |
| lista | Obtém a lista de aplicações criado no cluster de Service Fabric correspondentes os filtros especificados. |
| carregar | Obtém informações sobre uma aplicação de Service Fabric de carga. |
| Manifesto | Obtém o manifesto que descreve um tipo de aplicação. |
| Aprovisionar | Aprovisiona ou regista escrever uma aplicação de Service Fabric com o cluster utilizando o pacote. sfpkg no arquivo de externo ou o pacote de aplicações no arquivo de imagem. |
| report-health | Envia um relatório de estado de funcionamento da aplicação de Service Fabric. |
| tipo | Obtém a lista de tipos de aplicações no cluster de Service Fabric correspondente exatamente o nome especificado. |
| lista de tipos | Obtém a lista de tipos de aplicações no cluster de Service Fabric. |
| Não aprovisionamento | Remove ou anula o registo de um tipo de aplicação de Service Fabric do cluster. |
| atualização | Inicia a atualizar uma aplicação no cluster de Service Fabric. |
| retoma de atualização | Retoma a atualizar uma aplicação no cluster de Service Fabric. |
| reversão de atualização | Inicia a reverter a atualização atualmente em curso de uma aplicação no cluster de Service Fabric. |
| Estado de atualização | Obtém os detalhes para a atualização mais recente efetuada nesta aplicação. |
| carregar | Copie um pacote de aplicação de Service Fabric para o arquivo de imagens. |

## <a name="sfctl-application-create"></a>Criar aplicação sfctl
Cria uma aplicação de Service Fabric utilizando a descrição especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome da aplicação [necessário] | O nome da aplicação, incluindo o ' recursos de infraestrutura\:' esquema de URI. |
| --tipo de aplicação [necessário] | O nome do tipo de aplicação foi encontrado no manifesto da aplicação. |
| versão de – aplicação [necessária] | A versão do tipo de aplicação conforme definido no manifesto da aplicação. |
| --max-node-count | O número máximo de nós em que Service Fabric será capacidade de reserva para esta aplicação. Tenha em atenção que isto não significa que os serviços desta aplicação devem ser colocados em todos os nós. |
| -as métricas | Um JSON codificado lista de descrições de métrica de capacidade de aplicação. Uma métrica é definida como um nome, associado um conjunto das capacidades para cada nó que a aplicação existe na. |
| número de nós – mín. | O número mínimo de nós em que Service Fabric será capacidade de reserva para esta aplicação. Tenha em atenção que isto não significa que os serviços desta aplicação devem ser colocados em todos os nós. |
| -parâmetros | Uma lista JSON codificado de parâmetro de aplicação as substituições aplicadas ao criar a aplicação. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-delete"></a>sfctl aplicações eliminar
Elimina uma aplicação de Service Fabric existente.

Elimina uma aplicação de Service Fabric existente. Tem de ser criada uma aplicação poderá ser eliminado. Eliminar uma aplicação, irá eliminar todos os serviços que fazem parte dessa aplicação. Por predefinição, o Service Fabric tentará fechar as réplicas do serviço de forma correto e, em seguida, elimine o serviço. No entanto, se um serviço está com dificuldades em Fechar corretamente a réplica, a operação de eliminação pode demorar muito tempo ou ficar bloqueado. Utilize o sinalizador de ForceRemove opcional para ignorar a sequência de fechada correto e forçadamente eliminar a aplicação e todos os respetivos serviços.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
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

## <a name="sfctl-application-deployed"></a>sfctl aplicação implementada
Obtém as informações sobre uma aplicação implementada num nó de Service Fabric.

Obtém as informações sobre uma aplicação implementada num nó de Service Fabric.  Esta consulta devolve informações da aplicação de sistema se o ID de aplicação fornecido para a aplicação de sistema. Resultados abranger aplicações implementadas no Active Directory, ativação e a transferência Estados. Esta consulta requer que o nome de nó corresponde a um nó no cluster. A consulta falha se o nome de nó fornecido não aponta para os nós de recursos de infraestrutura de serviço ativo no cluster.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| – incluir--estado de funcionamento | Incluem o estado de funcionamento de uma entidade. Se este parâmetro for false ou não foi especificado, em seguida, o estado de funcionamento devolvido é "Desconhecido". Quando definido como VERDADEIRO, a consulta passa em paralelo para o nó e o serviço de sistema de estado de funcionamento antes dos resultados são intercalados. Como resultado, a consulta é mais dispendiosa e pode demorar mais tempo. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-deployed-health"></a>sfctl aplicação implementada-estado de funcionamento
Obtém as informações sobre o estado de funcionamento de uma aplicação implementada num nó de Service Fabric.

Obtém as informações sobre o estado de funcionamento de uma aplicação implementada num nó de Service Fabric. Utilize EventsHealthStateFilter para filtrar, opcionalmente, para a coleção de objetos de HealthEvent reportados aplicação implementada com base no estado de funcionamento. Utilize DeployedServicePackagesHealthStateFilter para filtrar opcionalmente para crianças DeployedServicePackageHealth com base no estado de funcionamento.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| --deployed-Service-Packages-Health-State-Filter | Permite a filtragem dos objetos de estado do Estado de funcionamento do serviço implementado pacote devolvidos nos resultados da consulta de estado de funcionamento da aplicação implementada com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas implementar serviço pacotes que correspondem ao filtro são devolvidos. Todos os pacotes de serviço implementado são utilizados para avaliar o estado de funcionamento de agregados de uma aplicação implementada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento de pacotes de serviços com o valor HealthState OK (2) e de aviso (4) são devolvidas.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| – estatísticas de estado de funcionamento de exclusão | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-deployed-list"></a>sfctl implementado-lista de aplicações
Obtém a lista de aplicações implementadas num nó de Service Fabric.

Obtém a lista de aplicações implementadas num nó de Service Fabric. Os resultados não incluem informações sobre aplicações de sistema implementado, a menos que explicitamente solicitadas pelo ID. Resultados abranger aplicações implementadas no Active Directory, ativação e a transferência Estados. Esta consulta requer que o nome de nó corresponde a um nó no cluster. A consulta falha se o nome de nó fornecido não aponta para os nós de recursos de infraestrutura de serviço ativo no cluster.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome do nó [necessário] | O nome do nó. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| – incluir--estado de funcionamento | Incluem o estado de funcionamento de uma entidade. Se este parâmetro for false ou não foi especificado, o estado de funcionamento devolvido é "Desconhecido". Quando definido como VERDADEIRO, a consulta passa em paralelo para o nó e o serviço de sistema de estado de funcionamento antes dos resultados são intercalados. Como resultado, a consulta é mais dispendiosa e pode demorar mais tempo. |
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

## <a name="sfctl-application-health"></a>o estado de funcionamento do sfctl aplicação
Obtém o estado de funcionamento da aplicação de recursos de infraestrutura de serviço.

Devolve o estado de estado de funcionamento da aplicação de recursos de infraestrutura de serviço. A resposta relatórios de estado de funcionamento Ok, erro ou de aviso. Se a entidade não foi encontrada no arquivo de estado de funcionamento, devolverá erro.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --deployed-applications-health-state-filter | Permite a filtragem dos objetos de estado de funcionamento de aplicações implementadas devolvido nos resultados da consulta de estado de funcionamento de aplicação com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas as aplicações implementadas que correspondem ao filtro vai ser devolvidas. Todas as aplicações implementadas são utilizadas para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento de aplicações implementadas com o valor HealthState OK (2) e de aviso (4) são devolvidas.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| – estatísticas de estado de funcionamento de exclusão | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro. |
| --services-health-state-filter | Permite a filtragem de objetos de estado de funcionamento dos serviços devolvido nos resultados da consulta de estado de funcionamento de serviços com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os serviços que correspondem ao filtro são devolvidos. Todos os serviços são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento de serviços com o valor HealthState OK (2) e de aviso (4) vai ser devolvido.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-info"></a>informações de aplicação sfctl
Obtém informações sobre uma aplicação de Service Fabric.

Devolve as informações sobre a aplicação que foi criada ou no processo de que está a ser criado no cluster de Service Fabric e cujo nome corresponde à especificado como parâmetro. A resposta inclui o nome, tipo, estado, parâmetros e outros detalhes sobre a aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| -parâmetros da aplicação de exclusão | O sinalizador que especifica se os parâmetros da aplicação serão excluídos do resultado. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-list"></a>lista de aplicações sfctl
Obtém a lista de aplicações criado no cluster de Service Fabric correspondentes os filtros especificados.

Obtém as informações sobre as aplicações que foram criados ou no processo de ser criados nos recursos de infraestrutura de serviço de cluster e corresponder os filtros especificados. A resposta inclui o nome, tipo, estado, parâmetros e outros detalhes sobre a aplicação. Se as aplicações não se enquadram numa página, é devolvida uma página de resultados, bem como um token de continuação que pode ser utilizado para obter a página seguinte. Filtros ApplicationTypeName e ApplicationDefinitionKindFilter não podem ser especificados ao mesmo tempo.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --application-definition-kind-filter | Utilizado para filtrar ApplicationDefinitionKind, que é o mecanismo utilizado para definir uma aplicação de Service Fabric.  <br> -Predefinição - o valor predefinido, que executa a mesma função que selecionar "Tudo". O valor é 0.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor ApplicationDefinitionKind. O valor é a 65535.  <br> -ServiceFabricApplicationDescription - filtro que corresponda à entrada com o valor de ApplicationDefinitionKind ServiceFabricApplicationDescription. O valor é 1.  <br> -Compose - filtro que corresponda à entrada com o valor de ApplicationDefinitionKind Compose. O valor é 2. |
| – o nome de tipo de aplicação | O nome do tipo de aplicação utilizado para filtrar as aplicações a consultar. Este valor não deve conter a versão de tipo de aplicação. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -parâmetros da aplicação de exclusão | O sinalizador que especifica se os parâmetros da aplicação serão excluídos do resultado. |
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

## <a name="sfctl-application-load"></a>sfctl carga da aplicação
Obtém informações sobre uma aplicação de Service Fabric de carga.

Devolve as carregar as informações sobre a aplicação que foi criada ou no processo de que está a ser criado no cluster de Service Fabric e cujo nome corresponde à especificado como parâmetro. A resposta inclui o nome, nós mínimo, máximo de nós, o número de nós que a aplicação está atualmente occupying e informações de métrica de carga de aplicação sobre a aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-manifest"></a>manifesto da aplicação sfctl
Obtém o manifesto que descreve um tipo de aplicação.

Obtém o manifesto que descreve um tipo de aplicação. A resposta contém o manifesto da aplicação XML como uma cadeia.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de tipo de aplicação [necessário] | O nome do tipo de aplicação. |
| --tipo-versão da aplicação [necessário] | A versão do tipo de aplicação. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-provision"></a>Aprovisionamento de aplicações sfctl
Aprovisiona ou regista escrever uma aplicação de Service Fabric com o cluster utilizando o pacote. sfpkg no arquivo de externo ou o pacote de aplicações no arquivo de imagem.

Aprovisiona um tipo de aplicação de Service Fabric com o cluster. Isto é necessário antes de quaisquer novas aplicações podem ser instanciadas. A operação de aprovisionamento pode ser efetuada no pacote de aplicação especificada pelo relativePathInImageStore ou utilizando o URI da. sfpkg externo. A menos que – externo-provision estiver definido, este comando irá esperar aprovisionar de arquivo de imagem.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --application-package-download-uri | O caminho para o pacote de aplicação '. sfpkg' a partir de onde o pacote de aplicações pode ser transferido através de protocolos HTTP ou HTTPS. <br><br> Para o tipo de aprovisionamento externo armazene apenas. O pacote de aplicações pode ser armazenado num arquivo externo que fornece a operação GET para transferir o ficheiro. Protocolos suportados são HTTP e HTTPS e o caminho tem de permitir acesso de leitura. |
| --tipo-compilação-caminho da aplicação | Para aprovisionar kind arquivo de imagens apenas. O caminho relativo para o pacote de aplicação no arquivo de imagem especificado durante a operação de carregamento anteriores. |
| – o nome de tipo de aplicação | Para o tipo de aprovisionamento externo armazene apenas. O nome do tipo de aplicação representa o nome do tipo de aplicação foi encontrado no manifesto da aplicação. |
| --tipo-versão da aplicação | Para o tipo de aprovisionamento externo armazene apenas. A versão de tipo de aplicação representa a versão do tipo de aplicação foi encontrada no manifesto da aplicação. |
| -Aprovisionar externo | A localização de onde o pacote de aplicação pode ser registado ou aprovisionado. Indica que o aprovisionamento é para um pacote de aplicação que foi carregado anteriormente para um arquivo de externo. O pacote de aplicação termina com a extensão *.sfpkg. |
| -não espera | Indica se é ou não aprovisionamento deve ocorrer no modo assíncrono. <br><br> Quando definido como VERDADEIRO, a operação de aprovisionamento devolve quando o pedido é aceite pelo sistema e a operação de aprovisionamento continua sem qualquer tempo limite. O valor predefinido é falso. Para pacotes de aplicações grandes, recomendamos que defina o valor como verdadeiro. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-report-health"></a>sfctl aplicação relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento da aplicação de Service Fabric.

Relatórios de estado de funcionamento da aplicação de Service Fabric especificada. O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um aplicação que encaminha para o arquivo de estado de funcionamento de gateway do Service Fabric. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, obter o estado de funcionamento da aplicação e verifique se o relatório é apresentado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. <br><br> Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o '\~' carateres. Por exemplo, se o nome da aplicação é ' recursos de infraestrutura\:/myapp/app1 ', a identidade da aplicação deverá ser ' myapp\~app1' 6.0 + e ' myapp/app1' em versões anteriores. |
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

## <a name="sfctl-application-type"></a>tipo de aplicação sfctl
Obtém a lista de tipos de aplicações no cluster de Service Fabric correspondente exatamente o nome especificado.

Devolve as informações sobre os tipos de aplicação que estão aprovisionados ou no processo de aprovisionamento do cluster do Service Fabric. Estes resultados são dos tipos de aplicação cujo nome corresponde ao exatamente especificado como o parâmetro e que estão em conformidade com os parâmetros de consulta especificada. Todas as versões do tipo de aplicação correspondente ao nome de tipo de aplicação são devolvidas, com cada versão devolvida como um tipo de aplicação. A resposta inclui o nome, versão, estado e outros detalhes sobre o tipo de aplicação. Esta é uma consulta paginada, o que significa que se não for todos os tipos de aplicação cabem numa página, é devolvida uma página de resultados, bem como um token de continuação que pode ser utilizado para obter a página seguinte. Por exemplo, se existirem 10 tipos de aplicação, mas uma página só se adequa os tipos de aplicação de três primeiro, ou se o resultado máximo é definido para 3, em seguida, 3 é devolvido. Para aceder ao resto dos resultados, obter as páginas subsequentes, utilizando o token de continuação devolvido na consulta seguinte. Um token de continuação vazio é devolvido se não houver nenhuma das páginas subsequente.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de tipo de aplicação [necessário] | O nome do tipo de aplicação. |
| --tipo-versão da aplicação | A versão do tipo de aplicação. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -parâmetros da aplicação de exclusão | O sinalizador que especifica se os parâmetros da aplicação serão excluídos do resultado. |
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

## <a name="sfctl-application-type-list"></a>tipo de aplicação sfctl-lista
Obtém a lista de tipos de aplicações no cluster de Service Fabric.

Devolve as informações sobre os tipos de aplicação que estão aprovisionados ou no processo de aprovisionamento do cluster do Service Fabric. Cada versão de um tipo de aplicação é devolvido como tipo de uma aplicação. A resposta inclui o nome, versão, estado e outros detalhes sobre o tipo de aplicação. Esta é uma consulta paginada, o que significa que se não for todos os tipos de aplicação cabem numa página, é devolvida uma página de resultados, bem como um token de continuação que pode ser utilizado para obter a página seguinte. Por exemplo, se existirem 10 tipos de aplicação, mas uma página só se adequa os tipos de aplicação de três primeiro, ou se o resultado máximo é definido para 3, em seguida, 3 é devolvido. Para aceder ao resto dos resultados, obter as páginas subsequentes, utilizando o token de continuação devolvido na consulta seguinte. Um token de continuação vazio é devolvido se não houver nenhuma das páginas subsequente.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --tipo-definição-tipo-filtro da aplicação | Utilizado para filtrar ApplicationTypeDefinitionKind, que é o mecanismo utilizado para definir um tipo de aplicação de Service Fabric.  <br> -Predefinição - o valor predefinido, que executa a mesma função que selecionar "Tudo". O valor é 0.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor ApplicationTypeDefinitionKind. O valor é a 65535.  <br> -ServiceFabricApplicationPackage - filtro que corresponda à entrada com o valor de ApplicationTypeDefinitionKind ServiceFabricApplicationPackage. O valor é 1.  <br> -Compose - filtro que corresponda à entrada com o valor de ApplicationTypeDefinitionKind Compose. O valor é 2. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -parâmetros da aplicação de exclusão | O sinalizador que especifica se os parâmetros da aplicação serão excluídos do resultado. |
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

## <a name="sfctl-application-unprovision"></a>não aprovisionamento de aplicações sfctl
Remove ou anula o registo de um tipo de aplicação de Service Fabric do cluster.

Remove ou anula o registo de um tipo de aplicação de Service Fabric do cluster. Esta operação só pode ser efetuada se todas as instâncias da aplicação do tipo de aplicação tem sido eliminadas. Depois do tipo de aplicação não está registado, não existem novas instâncias de aplicações podem ser criadas para este tipo de aplicação em particular.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de tipo de aplicação [necessário] | O nome do tipo de aplicação. |
| --tipo-versão da aplicação [necessário] | A versão do tipo de aplicação conforme definido no manifesto da aplicação. |
| parâmetro – assíncrona | O sinalizador que indica se é ou não não aprovisionamento deve ocorrer no modo assíncrono. Quando definido como VERDADEIRO, a operação de não aprovisionamento devolve quando o pedido é aceite pelo sistema e a operação de não aprovisionamento continua sem qualquer tempo limite. O valor predefinido é falso. No entanto, recomendamos defini-la como verdadeiro para os pacotes de aplicações de grandes dimensões que tenham sido aprovisionados. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-upgrade"></a>atualização da aplicação sfctl
Inicia a atualizar uma aplicação no cluster de Service Fabric.

Valida os parâmetros de atualização da aplicação fornecida e começa a efetuar a atualização da aplicação, se os parâmetros são válidos. Tenha em atenção que a atualização da descrição substitui a descrição da aplicação existente. Isto significa que, se os parâmetros não forem especificados, os parâmetros nas aplicações existentes serão substituídos com a lista de parâmetros vazio. Isto resulta na aplicação com o valor predefinido dos parâmetros do manifesto da aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. <br><br> Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o '\~' carateres. Por exemplo, se o nome da aplicação é ' recursos de infraestrutura\:/myapp/app1 ', a identidade da aplicação deverá ser ' myapp\~app1' 6.0 + e ' myapp/app1' em versões anteriores. |
| -[necessário] versão da aplicação | Versão da aplicação de destino. |
| -parâmetros [necessários] | Uma lista JSON codificado de parâmetro de aplicação as substituições aplicadas ao atualizar a aplicação. |
| -predefinição serviço-estado de funcionamento da política | JSON codificado especificação da política de estado de funcionamento utilizada por predefinição para avaliar o estado de funcionamento de um tipo de serviço. |
| -Falha de ação | A ação a executar quando uma atualização monitorizada encontra monitorização violações de políticas ou de estado de funcionamento da política. |
| -force-reinício | Reinicie forçadamente processos durante a atualização, mesmo quando a versão de código não foi alterada. |
| --health-check-retry-timeout | A quantidade de tempo para repetir avaliações do Estado de funcionamento quando a aplicação ou o cluster mau estado de funcionamento antes da ação de falha é executada. Medido em milissegundos.  Predefinição\: PT0H10M0S. |
| --health-check-stable-duration | A quantidade de tempo que a aplicação ou o cluster tem de permanecer bom estado de funcionamento antes de continua a atualização para o domínio de atualização seguinte. Medido em milissegundos.  Predefinição\: PT0H2M0S. |
| --health-check-wait-duration | A quantidade de tempo de espera após a conclusão de um domínio de atualização antes de aplicar políticas de estado de funcionamento. Medido em milissegundos.  Predefinição\: 0. |
| --max-unhealthy-apps | O máximo permitido de percentagem de mau estado de funcionamento aplicações implementadas. Representado como um número entre 0 e 100. |
| -modo | O modo utilizado para monitorizar o estado de funcionamento durante uma atualização sem interrupção.  Predefinição\: UnmonitoredAuto. |
| --replica-set-check-timeout | A quantidade máxima de tempo para bloquear o processamento de um domínio de atualização e evitar a perda de disponibilidade quando existirem problemas inesperados. Medido em segundos. |
| – política de estado de funcionamento de serviço | JSON codificado mapa com a política de estado de funcionamento do tipo de serviço por nome de tipo de serviço. O mapa está vazio ser predefinido. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |
| -atualização--tempo limite do domínio | A quantidade de tempo cada domínio de atualização tem de concluir antes de FailureAction é executada. Medido em milissegundos.  Predefinição\: P10675199DT02H48M05.4775807S. |
| -tempo limite de atualização | A quantidade de tempo de atualização global tem de concluir antes de FailureAction é executada. Medido em milissegundos.  Predefinição\: P10675199DT02H48M05.4775807S. |
| --warning-as-error | Processe avisos de avaliação do Estado de funcionamento com o mesmo gravidade como erros. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-upgrade-resume"></a>aplicação de sfctl atualização-retomar
Retoma a atualizar uma aplicação no cluster de Service Fabric.

Atualizar de uma aplicação de Service Fabric manual não monitorizada de retoma. Service Fabric atualiza um domínio de atualização de cada vez. Para atualizações manuais não monitorizadas, após a conclusão de Service Fabric um domínio de atualização, Esta aguarda para que possa invocar esta API antes de prosseguir para o domínio de atualização seguinte.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| -nome de domínio de atualização [necessário] | O nome do domínio de atualização na qual pretende retomar a atualização. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-upgrade-rollback"></a>aplicação de sfctl atualização-reversão
Inicia a reverter a atualização atualmente em curso de uma aplicação no cluster de Service Fabric.

Inicia a reverter a aplicação atual atualizar para a versão anterior. Esta API só pode ser utilizada para reverter a atualização em curso atual que está a implementar reencaminhar para a nova versão. Se a aplicação atualmente não está a ser atualizada utilize StartApplicationUpgrade API para atualizá-lo para a versão pretendida, incluindo a reverter para uma versão anterior.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-upgrade-status"></a>sfctl atualização-estado da aplicação
Obtém os detalhes para a atualização mais recente efetuada nesta aplicação.

Devolve informações sobre o estado da atualização mais recente aplicação juntamente com os detalhes para ajudar a depuração problemas de estado de funcionamento da aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-application-upload"></a>carregamento de aplicação sfctl
Copie um pacote de aplicação de Service Fabric para o arquivo de imagens.

Apresenta opcionalmente o progresso do carregamento para cada ficheiro no pacote. Carregar progresso é enviado para `stderr`.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – o caminho [necessário] | Caminho para o pacote de aplicação local. |
| cadeia de – o arquivo de imagens | Armazenar a imagem de destino para carregar o pacote de aplicação.  Predefinição\: recursos de infraestrutura\:arquivo de imagens. |
| – Mostrar progresso | Mostra progresso de carregamento de ficheiros para pacotes grandes. |

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
