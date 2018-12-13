---
title: O Azure Service Fabric CLI - sfctl compor | Documentos da Microsoft
description: Descreve a CLI do Service Fabric sfctl compor comandos.
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
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: d71b0c020fb9ceb305b56216d466bacb42ad21e8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53278156"
---
# <a name="sfctl-compose"></a>sfctl compose
Criar, eliminar e gerir aplicações do Docker Compose.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
| criar | Cria um recurso de infraestrutura do serviço de aplicação de implementação. |
| list | Obtém a lista de compor implementações criadas no cluster do Service Fabric. |
| remover | Elimina um recursos de infraestrutura existente do serviço de aplicação de implementação do cluster. |
| status | Obtém informações sobre um recurso de infraestrutura do serviço de aplicação de implementação. |
| Atualizar | Começa a atualizar uma implementação de composição no cluster do Service Fabric. |
| reversão de atualização | Atualizar começa a reverter uma implementação de composição no cluster do Service Fabric. |
| Estado de atualização | Obtém detalhes para a atualização mais recente realizada nestes recursos de infraestrutura do serviço de aplicação de implementação. |

## <a name="sfctl-compose-create"></a>o sfctl compor criar
Cria um recurso de infraestrutura do serviço de aplicação de implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | O nome da implementação. |
| – caminho de ficheiro [necessário] | Caminho para o ficheiro de Docker Compose de destino. |
| – pass encriptados | Em vez de pedir uma palavra-passe de registo de contentor, utilize uma frase secreta já encriptada. |
| – pass tem | Solicitará uma palavra-passe para o registo de contentor. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |
| --user | Nome de utilizador para ligar ao registo de contentor. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-list"></a>o sfctl compor a lista
Obtém a lista de compor implementações criadas no cluster do Service Fabric.

Obtém o estado sobre as implementações de composição que foram criadas ou no processo a ser criada no cluster do Service Fabric. A resposta inclui o nome, estado e outros detalhes sobre as implementações de composição. Se a lista de implementações não se encaixam numa página, uma página de resultados é retornada, bem como um token de continuação, que pode ser utilizado para obter a página seguinte.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -token de continuação | O parâmetro de token de continuação é usado para obter o próximo conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API, quando os resultados do sistema não se encaixam numa única resposta. Quando esse valor é passado para a próxima chamada de API, a API devolve o próximo conjunto de resultados. Se não existirem mais resultados, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser codificada com URL. |
| -max-resultados | O número máximo de resultados a serem retornados como parte das consultas paginadas. Este parâmetro define o limite superior no número de resultados devolvidos. Os resultados devolvidos pode ser menor do que os resultados máximos especificados se eles não se encaixam na mensagem de acordo com as restrições de tamanho de mensagem máximo definido na configuração. Se este parâmetro for igual a zero ou não especificado, a consulta paginada inclui resultados tantos possível que se enquadram na mensagem de retorna. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-remove"></a>o sfctl compose remove
Elimina um recursos de infraestrutura existente do serviço de aplicação de implementação do cluster.

Implementação de composição de eliminações um Service Fabric já existente.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | A identidade da implementação. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-status"></a>o sfctl compose status
Obtém informações sobre um recurso de infraestrutura do serviço de aplicação de implementação.

Devolve o estado da implementação de composição que foi criada ou no processo a ser criada no cluster do Service Fabric e cujo nome corresponde ao foi especificado como parâmetro. A resposta inclui o nome, estado e outros detalhes sobre a implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | A identidade da implementação. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-upgrade"></a>o sfctl compor atualização
Começa a atualizar uma implementação de composição no cluster do Service Fabric.

Valida os parâmetros de atualização fornecidos e começa a atualizar a implementação, se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | O nome da implementação. |
| – caminho de ficheiro [necessário] | Caminho para o destino Docker compose ficheiro. |
| --default-svc-type-health-map | JSON com a codificação de dicionário que descrevem a política de estado de funcionamento utilizada para avaliar o estado de funcionamento dos serviços. |
| – pass encriptados | Em vez de pedir uma palavra-passe de registo de contentor, utilize uma frase secreta já encriptada. |
| -Falha de ação | Os valores possíveis incluem\: 'inválido,""Reversão","Manual". |
| -force-reinício | Processos forçadamente são reiniciados durante a atualização, mesmo quando a versão de código não foi alterada. <br><br> A atualização só podem ser alterados ou dados de configuração. |
| – pass tem | Solicitará uma palavra-passe para o registo de contentor. |
| --health-check-retry | O período de tempo entre tentativas para realizar verificações de estado de funcionamento se o aplicativo ou o cluster não está em bom estado. |
| -verificação de estado de funcionamento-estável | A quantidade de tempo que a aplicação ou o cluster deve permanecer em bom estado antes que a atualização prossiga para o domínio de atualização seguinte. <br><br> Em primeiro lugar será interpretado como uma cadeia que representa uma duração ISO 8601. Se isso falhar, ela é interpretada como um número que representa o número total de milissegundos. |
| – espera de verificação de estado de funcionamento | O período de tempo de espera após a conclusão de um domínio de atualização antes de iniciar o estado de funcionamento verifica o processo. |
| --replica-set-check | A quantidade máxima de tempo para bloquear o processamento de um domínio de atualização e evitar a perda de disponibilidade quando surgem problemas inesperados. <br><br> Quando este tempo limite expira, o processamento de domínio de atualização irá continuar, independentemente de problemas de perda de disponibilidade. O tempo limite é reiniciado no início de cada domínio de atualização. Valores válidos são entre 0 e 42949672925, inclusive. |
| -svc-tipo-estado de funcionamento-map | JSON com a codificação de lista de objetos que descrevem as políticas de estado de funcionamento utilizadas para avaliar o estado de funcionamento de diferentes tipos de serviços. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |
| – mau estado de funcionamento da aplicação | O máximo permitido porcentagem de aplicativos de mau estado de funcionamento antes de comunicar um erro. <br><br> Por exemplo, para permitir que 10% dos aplicativos problemático, este valor seria de 10. A percentagem representa a percentagem máxima de tolerado de aplicativos que podem ser mau estado de funcionamento antes do cluster será considerado como erro. Se a percentagem é respeitada, mas há pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Isso é calculado dividindo o número de aplicativos de mau estado de funcionamento e o número total de instâncias da aplicação no cluster. |
| – atualização-domínio-tempo limite | A quantidade de tempo de cada domínio de atualização tem de concluir antes de ser executada FailureAction. <br><br> Em primeiro lugar será interpretado como uma cadeia que representa uma duração ISO 8601. Se isso falhar, ela é interpretada como um número que representa o número total de milissegundos. |
| – tipo de atualização | Predefinido\: sem interrupção. |
| – modo de atualização | Os valores possíveis incluem\: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', "Monitorizado".  Predefinido\: UnmonitoredAuto. |
| – tempo limite da atualização | A quantidade de tempo a atualização global tem de concluir antes de ser executada FailureAction. <br><br> Em primeiro lugar será interpretado como uma cadeia que representa uma duração ISO 8601. Se isso falhar, ela é interpretada como um número que representa o número total de milissegundos. |
| --user | Nome de utilizador para ligar ao registo de contentor. |
| --warning-as-error | Indica se os avisos são tratados com a mesma gravidade como erros. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-upgrade-rollback"></a>o sfctl compor reversão de atualização
Atualizar começa a reverter uma implementação de composição no cluster do Service Fabric.

Atualização da implementação de composição de reversão do service fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | A identidade da implementação. |
| – tempo limite -t | Tempo limite do servidor em segundos.  Predefinido\: 60. |

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| – depuração | Aumenta a verbosidade de registo para mostrar que todos os registos de depuração. |
| – ajudar -h | Mostre esta mensagem de ajuda e saída. |
| --o de saída | Formato de saída.  Valores permitidos\: json, jsonc, tabela, tsv.  Predefinido\: json. |
| – consulta | Cadeia de consulta do JMESPath. Consulte http\://jmespath.org/ para obter mais informações e exemplos. |
| -verbose | Aumenta a verbosidade do registo. Utilize--debug para os registos de depuração completa. |

## <a name="sfctl-compose-upgrade-status"></a>o sfctl compose status de atualização
Obtém detalhes para a atualização mais recente realizada nestes recursos de infraestrutura do serviço de aplicação de implementação.

Devolve as informações sobre o estado da atualização de implementação do compose, juntamente com detalhes para ajudar a depuração problemas de estado de funcionamento da aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -nome de implementação [necessário] | A identidade da implementação. |
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