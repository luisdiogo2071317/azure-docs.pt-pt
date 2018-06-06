---
title: Serviço do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de serviço do Service Fabric CLI sfctl.
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
ms.openlocfilehash: e027bb7f61d19fc274f7eddd8f28e9e6dac099ce
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763549"
---
# <a name="sfctl-service"></a>sfctl service
Criar, eliminar e gerir o serviço, tipos de serviço e pacotes de serviços.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| nome da aplicação | Obtém o nome da aplicação de Service Fabric para um serviço. |
| code-package-list | Obtém a lista de pacotes de código implementado num nó de Service Fabric. |
| criar | Cria o serviço do Service Fabric especificado. |
| eliminar | Elimina um serviço de recursos de infraestrutura de serviço existente. |
| tipo de implementação | Obtém as informações sobre um tipo de serviço especificado da aplicação implementada num nó num cluster de Service Fabric. |
| implementação tipo-lista | Obtém a lista que contém as informações sobre os tipos de serviço de aplicações implementadas num nó num cluster de Service Fabric. |
| descrição | Obtém a descrição de um serviço de recursos de infraestrutura de serviço existente. |
| get-container-logs | Obtém os registos do contentor para o contentor implementado num nó de Service Fabric. |
| estado de funcionamento | Obtém o estado de funcionamento do serviço do Service Fabric especificado. |
| informações | Obtém as informações sobre o serviço específico que pertencem à aplicação de Service Fabric. |
| lista | Obtém as informações sobre todos os serviços que pertencem à aplicação especificada pelo ID da aplicação. |
| Manifesto | Obtém o manifesto que descreve um tipo de serviço. |
| package-deploy | Transfere os pacotes associados à manifesto do serviço especificado para a cache de imagem no nó especificado. |
| package-health | Obtém as informações sobre o estado de funcionamento de um pacote de serviço para uma aplicação específica implementada para um nó de Service Fabric e da aplicação. |
| package-info | Obtém a lista de pacotes de serviços implementado num nó de Service Fabric correspondente exatamente o nome especificado. |
| package-list | Obtém a lista de pacotes de serviços implementado num nó de Service Fabric. |
| Recuperar | Indica, para o cluster do Service Fabric, o que deve tentar recuperar o serviço especificado que está atualmente bloqueado na perda de quórum. |
| report-health | Envia um relatório de estado de funcionamento do serviço do Service Fabric. |
| resolver | Resolva uma partição de Service Fabric. |
| lista de tipos | Obtém a lista que contém as informações sobre tipos de serviço que são suportados por um tipo de aplicação aprovisionados num cluster de Service Fabric. |
| Atualização | Atualiza o serviço especificado utilizando a descrição da atualização indicado. |

## <a name="sfctl-service-app-name"></a>nome de aplicação do serviço sfctl
Obtém o nome da aplicação de Service Fabric para um serviço.

Obtém o nome da aplicação para o serviço especificado. Um erro FABRIC_E_SERVICE_DOES_NOT_EXIST 404 é devolvido se não existir um serviço com o ID de serviço fornecido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
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

## <a name="sfctl-service-code-package-list"></a>sfctl serviço código--lista de pacotes
Obtém a lista de pacotes de código implementado num nó de Service Fabric.

Obtém a lista de pacotes de código implementado num nó de Service Fabric para a aplicação especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| -nome do pacote do código | O nome do pacote do código especificado no manifesto de serviço registado como parte de um tipo de aplicação de um cluster do Service Fabric. |
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

## <a name="sfctl-service-create"></a>criar serviço sfctl
Cria o serviço do Service Fabric especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| id da aplicação – [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o '\~' carateres. Por exemplo, se o nome da aplicação é ' recursos de infraestrutura\:/myapp/app1 ', a identidade da aplicação deverá ser ' myapp\~app1' 6.0 + e ' myapp/app1' em versões anteriores. |
| – o nome [necessário] | Nome do serviço. Isto deve ser um subordinado do ID de aplicação. Este é o nome, incluindo o `fabric\:` URI. Por exemplo service `fabric\:/A/B` é um elemento subordinado da aplicação `fabric\:/A`. |
| --tipo de serviço [necessário] | Nome do tipo de serviço. |
| -modo de ativação | O modo de ativação para o pacote de serviço. |
| -restrições | As restrições de posicionamento como uma cadeia. Restrições de posicionamento estão expressões nas propriedades de nó e permitem restringir um serviço a nós determinado com base nos requisitos de serviço. Por exemplo, para colocar um serviço em nós em que é azul NodeType especifique o seguinte\:"NodeColor = = blue". |
| --correlated-service | Nome do serviço de destino para correlacionar com. |
| -correlação | Correlacionar o serviço com um serviço existente utilizando uma afinidade de alinhamento. |
| --dns-name | O nome DNS do serviço a ser criado. O serviço de sistema de DNS de recursos de infraestrutura de serviço tem de estar ativado para esta definição. |
| -Contagem de instâncias | A contagem de instâncias. Isto aplica-se apenas a serviços sem monitorização de estado. |
| --int-scheme | Indica que o serviço deve ser particionado uniformemente através de um intervalo de números inteiros não assinados. |
| --int-scheme-count | O número de partições dentro do intervalo de chave de número inteiro para criar, se utilizar um esquema de partição de número inteiro uniforme. |
| – int esquema-alto | O fim do intervalo de número inteiro de chave, se utilizar um esquema de partição de número inteiro uniforme. |
| --int-scheme-low | O início do intervalo de número inteiro de chave, se utilizar um esquema de partição de número inteiro uniforme. |
| -as métricas de carga | Lista JSON codificado de métricas utilizadas quando o balanceamento de carga services entre nós. |
| --min-replica-set-size | A réplica mínimo definir tamanho como um número. Isto aplica-se apenas a serviços com monitorização de estado. |
| --move-cost | Especifica o custo de mover para o serviço. Os valores possíveis são\: 'Zero', 'Baixa', 'Média', 'Alto'. |
| -esquema com o nome | Indica que o serviço deve ter várias partições com nome. |
| -com o nome esquema-lista | JSON codificado lista de nomes para particionar o serviço entre, se utilizar o esquema de partição com nome. |
| – Estado não persistente | Se for VERDADEIRO, isto indica que o serviço não tem nenhum estado persistente armazenado no disco local, ou apenas armazena estado na memória. |
| – lista de políticas de colocação | JSON codificado lista de políticas de colocação para o serviço e quaisquer associados nomes de domínio. As políticas podem ser um ou mais dos\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| -espera de perda de quórum | A duração máxima, em segundos, para que uma partição pode estar num Estado de perda de quórum. Isto aplica-se apenas a serviços com monitorização de estado. |
| -espera de reinício de réplica | A duração, em segundos, entre quando uma réplica fica inativo e quando é criada uma nova réplica. Isto aplica-se apenas a serviços com monitorização de estado. |
| -as políticas de dimensionamento | JSON codificado lista de políticas para este serviço de dimensionamento. |
| --singleton-scheme | Indica o serviço deve ter uma única partição ou ser um serviço não particionado. |
| -modo de espera pela réplica keep | A duração máxima, em segundos, para o modo de espera réplicas serão mantidas antes de a ser removido. Isto aplica-se apenas a serviços com monitorização de estado. |
| -monitorização de estado | Indica que o serviço é um serviço com monitorização de estado. |
| – sem monitorização de estado | Indica que o serviço é um serviço sem estado. |
| --target-replica-set-size | A réplica de destino definir tamanho como um número. Isto aplica-se apenas a serviços com monitorização de estado. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-delete"></a>eliminação do serviço de sfctl
Elimina um serviço de recursos de infraestrutura de serviço existente.

Um serviço tem de ser criado antes de ser eliminado. Por predefinição, o Service Fabric tentará fechar as réplicas do serviço de forma correto e, em seguida, elimine o serviço. No entanto, se o serviço está com dificuldades em Fechar corretamente a réplica, a operação de eliminação pode demorar muito tempo ou ficar bloqueado. Utilize o sinalizador de ForceRemove opcional para ignorar a sequência de fechada correto e forçadamente eliminar o serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
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

## <a name="sfctl-service-deployed-type"></a>sfctl tipo de serviço implementado
Obtém as informações sobre um tipo de serviço especificado da aplicação implementada num nó num cluster de Service Fabric.

Obtém a lista que contém as informações sobre um tipo de serviço específicos das aplicações implementadas num nó num cluster de Service Fabric. A resposta inclui o nome do tipo de serviço, o estado de registo, o pacote do código que registado e ativação ID do pacote de serviço. Cada entrada representa uma ativação de um tipo de serviço, diferenciada pelo ID de ativação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de tipo de serviço [necessário] | Especifica o nome de um tipo de serviço do Service Fabric. |
| -nome do manifesto do serviço | O nome do manifesto de serviço para filtrar a lista de informações de tipo de serviço implementado. Se for especificado, a resposta irá conter apenas as informações sobre tipos de serviço que estão definidas no manifesto serviço. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-deployed-type-list"></a>sfctl serviço implementado-tipo-lista
Obtém a lista que contém as informações sobre os tipos de serviço de aplicações implementadas num nó num cluster de Service Fabric.

Obtém a lista que contém as informações sobre os tipos de serviço de aplicações implementadas num nó num cluster de Service Fabric. A resposta inclui o nome do tipo de serviço, o estado de registo, o pacote do código que registado e ativação ID do pacote de serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| -nome do manifesto do serviço | O nome do manifesto de serviço para filtrar a lista de informações de tipo de serviço implementado. Se for especificado, a resposta irá conter apenas as informações sobre tipos de serviço que estão definidas no manifesto serviço. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-description"></a>Descrição do serviço sfctl
Obtém a descrição de um serviço de recursos de infraestrutura de serviço existente.

Obtém a descrição de um serviço de recursos de infraestrutura de serviço existente. Tem de ser criado um serviço pode ser obtida a respetiva descrição.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
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

## <a name="sfctl-service-get-container-logs"></a>sfctl get-contentor-registos do serviço
Obtém os registos do contentor para o contentor implementado num nó de Service Fabric.

Obtém os registos do contentor para o contentor implementado num nó de Service Fabric para o pacote do código fornecido.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| -nome de pacote de código [necessário] | O nome do pacote do código especificado no manifesto de serviço registado como parte de um tipo de aplicação de um cluster do Service Fabric. |
| --nome do nó [necessário] | O nome do nó. |
| --manifesto-nome do serviço [necessário] | O nome de um manifesto de serviço registado como parte de um tipo de aplicação de um cluster do Service Fabric. |
| -anterior | Especifica se pretende obter registos de contentor de contentores terminado/mensagens não da instância de pacote do código. |
| -Cauda | Número de linhas a mostrar a partir da extremidade dos registos. Predefinido é 100. 'tudo' para mostrar os registos de conclusão. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-health"></a>Estado de funcionamento de serviço de sfctl
Obtém o estado de funcionamento do serviço do Service Fabric especificado.

Obtém as informações de estado de funcionamento do serviço especificado. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicadas falhas no serviço, com base no estado de funcionamento. PartitionsHealthStateFilter de utilização para filtrar a coleção de partições devolvido. Se especificar um serviço que não existe no arquivo de estado de funcionamento, este pedido devolve um erro.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| --events-health-state-filter | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| – estatísticas de estado de funcionamento de exclusão | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro. |
| --partitions-health-state-filter | Permite a filtragem de objetos de estado de funcionamento partições devolvido nos resultados da consulta de estado de funcionamento de serviço com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas as partições que correspondem ao filtro são devolvidas. Todas as partições são utilizadas para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento de partições com o valor HealthState OK (2) e de aviso (4) vai ser devolvido.  <br> -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero.  <br> -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1.  <br> -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2.  <br> -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4.  <br> -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8.  <br> -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-info"></a>informações do serviço de sfctl
Obtém as informações sobre o serviço específico que pertencem à aplicação de Service Fabric.

Devolve as informações sobre o serviço especificado que pertencem à aplicação de Service Fabric especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
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

## <a name="sfctl-service-list"></a>lista de serviço sfctl
Obtém as informações sobre todos os serviços que pertencem à aplicação especificada pelo ID da aplicação.

Devolve as informações sobre todos os serviços que pertencem à aplicação especificada pelo ID da aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| -token de continuação | O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado. |
| -nome do tipo de serviço | O nome do tipo de serviço utilizado para filtrar os serviços a consultar. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-manifest"></a>manifesto de serviço sfctl
Obtém o manifesto que descreve um tipo de serviço.

Obtém o manifesto que descreve um tipo de serviço. A resposta contém o manifesto do serviço XML como uma cadeia.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de tipo de aplicação [necessário] | O nome do tipo de aplicação. |
| --tipo-versão da aplicação [necessário] | A versão do tipo de aplicação. |
| --manifesto-nome do serviço [necessário] | O nome de um manifesto de serviço registado como parte de um tipo de aplicação de um cluster do Service Fabric. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-package-deploy"></a>implementação de pacote de serviço sfctl
Transfere os pacotes associados à manifesto do serviço especificado para a cache de imagem no nó especificado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| nome de tipo de aplicação – [necessário] | O nome do manifesto da aplicação para o manifesto de pedido de serviço correspondentes. |
| versão de tipo de aplicação – [necessário] | A versão do manifesto da aplicação para o manifesto de pedido de serviço correspondentes. |
| --nome do nó [necessário] | O nome do nó. |
| --manifesto-nome do serviço [necessário] | O nome do manifesto de serviço associado aos pacotes que serão transferidos. |
| -partilha de política | Lista JSON codificado de políticas de partilha. Cada elemento de política de partilha é composto por um 'name' e 'âmbito'. O nome corresponde ao nome do pacote do código, de configuração ou de dados que está a ser partilhado. O âmbito pode ser 'None', 'All', 'Código', 'Config' ou 'Data'. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-package-health"></a>sfctl serviço pacote-estado de funcionamento
Obtém as informações sobre o estado de funcionamento de um pacote de serviço para uma aplicação específica implementada para um nó de Service Fabric e da aplicação.

Obtém as informações sobre o estado de funcionamento do pacote de serviço para uma aplicação específica implementada num nó de Service Fabric. Utilize EventsHealthStateFilter para filtrar, opcionalmente, para a coleção de objetos de HealthEvent comunicado no pacote de serviço implementado com base no estado de funcionamento.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de pacote de serviço [necessário] | O nome do pacote de serviço. |
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

## <a name="sfctl-service-package-info"></a>informações de pacote do serviço sfctl
Obtém a lista de pacotes de serviços implementado num nó de Service Fabric correspondente exatamente o nome especificado.

Devolve as informações sobre os pacotes de serviço implementado num nó de Service Fabric para a aplicação especificada. Estes resultados dos pacotes de serviço cujo nome corresponde exatamente o nome do pacote de serviço especificados como parâmetro.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
| --nome do nó [necessário] | O nome do nó. |
| -nome de pacote de serviço [necessário] | O nome do pacote de serviço. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-package-list"></a>lista de pacotes do serviço sfctl
Obtém a lista de pacotes de serviços implementado num nó de Service Fabric.

Devolve as informações sobre os pacotes de serviço implementado num nó de Service Fabric para a aplicação especificada.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --id da aplicação [necessário] | A identidade da aplicação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome da aplicação é "recursos de infraestrutura\:/myapp/app1", a identidade da aplicação seria "myapp\~app1" 6.0 + e "myapp/app1" em versões anteriores. |
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

## <a name="sfctl-service-recover"></a>sfctl recuperar de serviço
Indica, para o cluster do Service Fabric, o que deve tentar recuperar o serviço especificado que está atualmente bloqueado na perda de quórum.

Indica, para o cluster do Service Fabric, o que deve tentar recuperar o serviço especificado que está atualmente bloqueado na perda de quórum. Esta operação só deve ser efetuada se sabe-se que não não possível recuperar as réplicas que estão em baixo. Utilização incorreta desta API pode causar a potencial perda de dados.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
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

## <a name="sfctl-service-report-health"></a>sfctl serviço relatório-Estado de funcionamento
Envia um relatório de estado de funcionamento do serviço do Service Fabric.

Relatórios de estado de funcionamento do serviço do Service Fabric especificado. O relatório tem de conter as informações sobre a origem do relatório de estado de funcionamento e propriedade em que é comunicada. O relatório é enviado para um serviço, que reencaminha para o arquivo de estado de funcionamento do gateway de Service Fabric. O relatório pode ser aceites pelo gateway, mas rejeitado pelo arquivo de estado de funcionamento após a validação adicional. Por exemplo, o arquivo de estado de funcionamento pode rejeitar o relatório devido a um parâmetro inválido, como um número de sequência obsoletos. Para ver se o relatório foi aplicado no arquivo de estado de funcionamento, verifique se o relatório aparece nos eventos de estado de funcionamento do serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -propriedade de estado de funcionamento [necessário] | A propriedade das informações de estado de funcionamento. <br><br> Uma entidade pode ter relatórios de estado de funcionamento para as diferentes propriedades. A propriedade é uma cadeia e não uma enumeração fixa para permitir a flexibilidade de relatório para categorizar a condição de estado que aciona o relatório. Por exemplo, um relatório com SourceId "LocalWatchdog" pode monitorizar o estado dos discos disponíveis no nó, pelo que pode comunicar "AvailableDisk" propriedade nesse nó. O relatório mesmo pode monitorizar a conectividade de nó, pelo que pode reportar uma propriedade "Conectividade" no mesmo nó. No arquivo de estado de funcionamento, estes relatórios são tratados como eventos de estado de funcionamento separada para o nó especificado. Juntamente com o SourceId, a propriedade identifica de forma exclusiva as informações de estado de funcionamento. |
| -Estado de funcionamento [necessário] | Os valores possíveis incluem\: 'Inválido', 'Ok', 'Aviso', 'Error', 'Desconhecido'. |
| – id de serviço [necessário] | A identidade do serviço. <br><br> Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o '\~' carateres. Por exemplo, se o nome de serviço é ' recursos de infraestrutura\:/myapp/app1/svc1', a identidade de serviço seria ' myapp\~app1\~svc1' 6.0 + e ' myapp/app1/svc1' em versões anteriores. |
| – id de origem [necessário] | O nome de origem que identifica o componente de cliente/watchdog do/sistema gera as informações de estado de funcionamento. |
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

## <a name="sfctl-service-resolve"></a>resolução de serviço sfctl
Resolva uma partição de Service Fabric.

Resolva uma partição de serviço do Service Fabric para obter os pontos finais das réplicas do serviço.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de serviço [necessário] | A identidade do serviço. Isto é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é "recursos de infraestrutura\:/myapp/app1/svc1", a identidade de serviço seria "myapp\~app1\~svc1" 6.0 + e "myapp/app1/svc1" em versões anteriores. |
| – o tipo de chave de partição | Tipo de chave para a partição. Este parâmetro é necessário se o esquema de partição para o serviço é Int64Range ou nomeado. Os valores possíveis são seguintes. -Nenhum (1) - indica que o parâmetro PartitionKeyValue não está especificado. Isto é válido para partições com a criação de partições de esquema como Singleton. Este é o valor predefinido. O valor é 1. -Int64Range (2) - indica que o parâmetro de PartitionKeyValue é uma chave de partição int64. Isto é válido para partições com a criação de partições de esquema como Int64Range. O valor é 2. -Com o nome (3) - indica que o parâmetro PartitionKeyValue é um nome da partição. Isto é válido para partições com a criação de partições de esquema como nomeado. O valor é 3. |
| – o valor de chave de partição | Chave de partição. Isto é necessário se o esquema de partição para o serviço é Int64Range ou nomeado. |
| --previous-rsp-version | O valor no campo versão da resposta recebida anteriormente. Isto é necessário se o utilizador sabe que o resultado que foi foi obtido anteriormente está obsoleto. |
| tempo limite – -t | Tempo limite do servidor em segundos.  Predefinição\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração. |
| -ajudar -h | Mostra esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinição\: json. |
| – consulta | Cadeia de consulta JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas. |

## <a name="sfctl-service-type-list"></a>tipo de serviço sfctl-lista
Obtém a lista que contém as informações sobre tipos de serviço que são suportados por um tipo de aplicação aprovisionados num cluster de Service Fabric.

Obtém a lista que contém as informações sobre tipos de serviço que são suportados por um tipo de aplicação aprovisionados num cluster de Service Fabric. O tipo de aplicação fornecido tem de existir. Caso contrário, é devolvido um estado 404.

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

## <a name="sfctl-service-update"></a>atualização do serviço sfctl
Atualiza o serviço especificado utilizando a descrição da atualização indicado.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| – id de serviço [necessário] | O ID do serviço. Este ID é, geralmente, o nome completo do serviço sem o ' recursos de infraestrutura\:' esquema de URI. A partir da versão 6.0, nomes hierárquicos são delimitados por com o "\~" carateres. Por exemplo, se o nome de serviço é ' recursos de infraestrutura\:/myapp/app1/svc1', a identidade de serviço seria ' myapp\~app1\~svc1' 6.0 + e ' myapp/app1/svc1' em versões anteriores. |
| -restrições | As restrições de posicionamento como uma cadeia. Restrições de posicionamento estão expressões nas propriedades de nó e permitem restringir um serviço a nós determinado com base nos requisitos de serviço. Por exemplo, para colocar um serviço em nós em que é azul NodeType especifique o seguinte\: "NodeColor = = blue". |
| --correlated-service | Nome do serviço de destino para correlacionar com. |
| -correlação | Correlacionar o serviço com um serviço existente utilizando uma afinidade de alinhamento. |
| -Contagem de instâncias | A contagem de instâncias. Isto aplica-se apenas a serviços sem monitorização de estado. |
| -as métricas de carga | Lista JSON codificado de métricas utilizado quando balanceamento em nós de carga. |
| --min-replica-set-size | A réplica mínimo definir tamanho como um número. Este tamanho de conjunto aplica-se apenas a serviços com monitorização de estado. |
| --move-cost | Especifica o custo de mover para o serviço. Os valores possíveis são\: 'Zero', 'Baixa', 'Média', 'Alto'. |
| – lista de políticas de colocação | JSON codificado lista de políticas de colocação para o serviço e quaisquer associados nomes de domínio. As políticas podem ser um ou mais dos\: `NonPartiallyPlaceService`, `PreferPrimaryDomain`, `RequireDomain`, `RequireDomainDistribution`. |
| -espera de perda de quórum | A duração máxima, em segundos, para que uma partição pode estar num Estado de perda de quórum. Esta duração aplica-se apenas a serviços com monitorização de estado. |
| -espera de reinício de réplica | A duração, em segundos, entre quando uma réplica fica inativo e quando é criada uma nova réplica. Esta duração aplica-se apenas a serviços com monitorização de estado. |
| -as políticas de dimensionamento | JSON codificado lista de políticas para este serviço de dimensionamento. |
| -modo de espera pela réplica keep | A duração máxima, em segundos, para o modo de espera réplicas serão mantidas antes de a ser removido. Esta duração aplica-se apenas a serviços com monitorização de estado. |
| -monitorização de estado | Indica que o serviço de destino é um serviço com monitorização de estado. |
| – sem monitorização de estado | Indica que o serviço de destino é um serviço sem estado. |
| --target-replica-set-size | A réplica de destino definir tamanho como um número. Este tamanho de conjunto aplica-se apenas a serviços com monitorização de estado. |
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
