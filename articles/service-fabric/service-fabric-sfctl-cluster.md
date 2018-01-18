---
title: Cluster do Azure Service Fabric CLI - sfctl | Microsoft Docs
description: Descreve os comandos de cluster do Service Fabric CLI sfctl.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 5eeff271fea67cd859dff598cae0010cf3b8e13f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-cluster"></a>sfctl cluster
Selecione, gerir e operar clusters de Service Fabric.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
|    code-versions| Obtém uma lista dos recursos de infraestrutura versões de código aprovisionados num cluster de Service Fabric.|
|    config-versions | Obtém uma lista dos recursos de infraestrutura versões de configuração que são aprovisionadas num cluster de Service Fabric.|
|    estado de funcionamento       | Obtém o estado de funcionamento de um cluster do Service Fabric.|
|    Manifesto     | Obter o manifesto do cluster de Service Fabric.|
|    cancelamento da operação| Cancela uma operação do índice de falhas induzida pelo utilizador.|
|    operationgit | Obtém uma lista de operações do índice de falhas induzida pelo utilizador filtrados pela entrada fornecida.|
|    Aprovisionar     | Aprovisione os pacotes de código ou de configuração de um cluster do Service Fabric.|
|    recover-system  | Indica, para o cluster do Service Fabric, o que deve tentar recuperar os serviços do sistema que atualmente estão bloqueados na perda de quórum.|
|report-health   | Envia um relatório de estado de funcionamento no cluster de Service Fabric.|
|    seleccionar       | Liga a um ponto final de cluster de Service Fabric.|
| Não aprovisionamento     | Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric.|
|    atualização         | Comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric.|
|    retoma de atualização  | Efetuar a atualização do cluster avançar para o domínio de atualização seguinte.|
|    upgrade-rollback| Reverta a atualização de um cluster do Service Fabric.|
|    upgrade-status  | Obtém o progresso da atualização do cluster atual.|
|upgrade-update  | Atualize os parâmetros de atualização de uma atualização de cluster do Service Fabric.|


## <a name="sfctl-cluster-health"></a>o estado de funcionamento do sfctl cluster
Obtém o estado de funcionamento de um cluster do Service Fabric.

Obtém o estado de funcionamento de um cluster do Service Fabric. Utilize EventsHealthStateFilter para filtrar a recolha de eventos de estado de funcionamento comunicado no cluster com base no estado de funcionamento. Da mesma forma, utilize NodesHealthStateFilter e ApplicationsHealthStateFilter para filtrar a coleção de nós e aplicações devolvidas com base no respetivo estado de funcionamento agregada. 

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --applications-health-state-filter| Permite a filtragem dos objetos de estado de funcionamento da aplicação devolvidos nos resultados da consulta de estado de funcionamento de cluster com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem obtido a partir membros ou operações bit a bit em membros de enumeração de HealthStateFilter de valor de número inteiro. Apenas as aplicações que correspondem ao filtro são devolvidas.  Todas as aplicações são utilizadas para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são sinalizador em enumeração, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, estado de funcionamento das aplicações com o valor HealthState OK (2) e de aviso (4) são devolvidas. -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero. -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1. -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2. -Aviso - filtro que corresponde à entrada com HealthState valor aviso. O valor é 4. -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8. -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535.|
| --events-health-state-filter   | Permite a filtragem na coleção de objetos de HealthEvent devolvido com base no estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas os eventos que correspondem ao filtro são devolvidos. Todos os eventos são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são sinalizador com base na enumeração, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é 6, em seguida, todos os eventos com o valor HealthState OK (2) e de aviso (4) são devolvidos. -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero. -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1. -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2. -Aviso - filtro que corresponde à entrada com HealthState valor aviso.  O valor é 4. -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8. -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535.|
|--exclude-health-statistics                   | Indica se as estatísticas de estado de funcionamento devem ser devolvidas como parte do resultado da consulta. FALSO por predefinição. As estatísticas mostram o número de elementos subordinados entidades no estado de funcionamento Ok, aviso e erro.|
 |   --include-system-application-health-statistics| Indica se as estatísticas de estado de funcionamento devem incluir o fabric: / estatísticas de estado de funcionamento da aplicação de sistema. FALSO por predefinição. Se IncludeSystemApplicationHealthStatistics estiver definido como VERDADEIRO, o estado de funcionamento estatísticas incluem as entidades que pertencem aos recursos de infraestrutura: / aplicação de sistema. Caso contrário, o resultado da consulta inclui estatísticas de estado de funcionamento apenas para aplicações do utilizador. As estatísticas de estado de funcionamento têm de ser incluídas no resultado da consulta para este parâmetro seja aplicada.|
| --nodes-health-state-filter    | Permite a filtragem de objetos de estado de funcionamento nó devolvidos nos resultados da consulta de estado de funcionamento de cluster com base no respetivo estado de funcionamento. Os valores possíveis para este parâmetro incluem o valor de número inteiro de um dos seguintes Estados de funcionamento. Apenas nós que correspondem ao filtro são devolvidos. Todos os nós são utilizados para avaliar o estado de funcionamento agregada. Se não for especificado, são devolvidas todas as entradas. Os valores de estado são enumeração baseada no sinalizador de, pelo que o valor pode ser uma combinação destes valores obtidos através de operador de 'Ou' bit a bit. Por exemplo, se o valor fornecido é "6", em seguida, estado de funcionamento de nós com o valor HealthState OK (2) e de aviso (4) são devolvidas. -Predefinição - valor predefinido. Corresponde a qualquer HealthState. O valor é zero. -Nenhuma - filtro que não corresponde a qualquer valor HealthState. Utilizado para não devolver resultados numa coleção especificada dos Estados. O valor é 1. -Ok - filtre que corresponde de entrada com o valor de HealthState Ok. O valor é 2. -Aviso - filtro que corresponde à entrada com HealthState valor aviso.  O valor é 4. -Erro - filtro que corresponda à entrada com o valor de HealthState erro. O valor é 8. -Todos os - filtro que corresponda à entrada com qualquer valor HealthState. O valor é a 65535.|
| tempo limite – -t                   | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug                        | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h                      | Mostra esta mensagem de ajuda e saída.|
| --o de saída                    | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.                    Predefinição: json.|
| – consulta                        | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose                      | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-cluster-manifest"></a>manifesto do cluster sfctl
Obter o manifesto do cluster de Service Fabric.

Obter o manifesto do cluster de Service Fabric. O manifesto do cluster contém as propriedades do cluster, que incluem os tipos de outro nó no cluster, configurações de segurança, falhas e topologias de domínio de atualização etc. Estas propriedades são especificadas como parte do ficheiro Clusterconfig durante a implementação de um cluster autónomo. No entanto, a maioria das informações no manifesto do cluster é gerado internamente pelo recurso de infraestrutura de serviço durante a implementação de cluster nos outros cenários de implementação (por exemplo, quando utilizar o [portal do Azure](https://portal.azure.com)). O conteúdo do manifesto do cluster é apenas para fins informativos e os utilizadores não são esperados para colocar uma dependência no formato do conteúdo do ficheiro ou a interpretação. 

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| tempo limite – -t| Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug  | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h| Mostra esta mensagem de ajuda e saída.|
| --o de saída | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta  | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose| Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-cluster-provision"></a>Aprovisionamento de cluster sfctl
Aprovisione os pacotes de código ou de configuração de um cluster do Service Fabric.
Valide e aprovisionar os pacotes de código ou de configuração de um cluster do Service Fabric.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
|--cluster-manifest-file-path| O caminho de ficheiro de manifesto do cluster.|
|    --code-file-path            | Caminho de ficheiro de pacote do código de cluster.|
|    tempo limite – -t                | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h  | Mostra esta mensagem de ajuda e saída.|
| --o de saída| Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose  | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-cluster-select"></a>Selecione de cluster sfctl
Liga a um ponto final de cluster de Service Fabric.

Se ligar ao cluster segura, especifique um certificado (. crt) e o ficheiro de chave (.key) ou um único ficheiro com ambos os (. pem). Não especifique ambos. Opcionalmente, de se ligar a um cluster seguro, também especificar um caminho para um ficheiro de pacote de AC ou o diretório de certificados de AC fidedignos.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -ponto final [necessário]| URL de ponto final, incluindo a porta e o prefixo HTTP ou HTTPS de cluster.|
| – aad             | Utilize o Azure Active Directory para autenticação.|
| --ca              | Caminho para o diretório de certificados de AC para tratar como válido ou um ficheiro de pacote de AC.|
| -certificados            | Caminho para um ficheiro de certificado de cliente.|
| -chave             | Caminho do ficheiro de chave do certificado de cliente.|
| -Certifique-se de não       | Desativar a verificação de certificados quando utilizar HTTPS, tenha em atenção: Esta é uma opção inseguras e não deve ser utilizada para ambientes de produção.|
| -pem             | Caminho do certificado de cliente, como um ficheiro. pem.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug           | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h         | Mostra esta mensagem de ajuda e saída.|
| --o de saída       | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta           | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose         | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-cluster-unprovision"></a>não aprovisionamento de cluster sfctl
Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric.

Anular o provisionamento os pacotes de código ou de configuração de um cluster do Service Fabric.

### <a name="arguments"></a>Argumentos
|Argumento|Descrição|
| --- | --- |
|--code-version  | A versão de pacote do código de cluster.|
|    --config-version| O versão do manifesto do cluster.|
|    tempo limite – -t    | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global
|Argumento|Descrição|
| --- | --- |
|--debug         | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
 |   -ajudar -h       | Mostra esta mensagem de ajuda e saída.|
 |   --o de saída     | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
 |   – consulta         | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
 |   -verbose       | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|


## <a name="sfctl-cluster-upgrade"></a>atualização do cluster sfctl
Comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric.
Valide os parâmetros de atualização fornecidos e comece a atualizar a versão de código ou de configuração de um cluster do Service Fabric se os parâmetros são válidos.

### <a name="arguments"></a>Argumentos
|Argumento|Descrição|
| --- | --- |
|    --app-health-map                      | JSON codificado dicionário de pares de nome de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro.|
 |   --app-type-health-map                 | JSON codificado dicionário de pares de nome de tipo de aplicação e a percentagem máxima mau estado de funcionamento antes de gerar o erro.|
 |   --code-version                        | A versão de código de cluster.|
 |   --config-version                      | A versão de configuração de cluster.|
 |   --delta-health-evaluation             | Permite a avaliação de estado de funcionamento de diferenças em vez de avaliação de estado de funcionamento absoluto após a conclusão de cada domínio de atualização.|
 |   --delta-unhealthy-nodes               | O máximo permitido percentagem de nós degradação de estado de funcionamento permitida durante as atualizações de cluster.  Predefinição: 10. O delta é medido entre o estado de nós no início da atualização e o estado de nós no momento da avaliação de estado de funcionamento. A verificação é executada após a conclusão de atualização cada domínio de atualização para se certificar de que o estado global do cluster está dentro dos limites tolerated.|
 |   -Falha de ação                      | Os valores possíveis incluem: 'Inválido,' 'Reversão', 'Manual'.|
 |   --force-restart                       | Força o reinício.|
 |   --health-check-retry                  | Tempo limite de tentativas de verificação do Estado de funcionamento é medido em milissegundos.|
 |   --health-check-stable                 | Estado de funcionamento Consulte duração estável, medida em milissegundos.|
  |  --health-check-wait                   | Duração de espera de verificação do Estado de funcionamento é medido em milissegundos.|
  |  --replica-set-check-timeout           | Réplica atualização definir um tempo limite de verificação medido em segundos.|
 |   --rolling-upgrade-mode                | Os valores possíveis incluem: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorizado'.  Predefinição: UnmonitoredAuto.|
  |  tempo limite – -t                          | Tempo limite do servidor em segundos.  Predefinição: 60.|
  |  --unhealthy-applications              | O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster, excluindo as aplicações de tipos de aplicação que estão incluídas no ApplicationTypeHealthPolicyMap. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de aplicações.|
 |   -mau estado de funcionamento de nós                     | O máximo permitido de percentagem de nós mau estado de funcionamento antes de o comunicar um erro. Por exemplo, para permitir a 10% de nós incorreto, este valor será 10. A percentagem representa a percentagem máxima de tolerated de nós que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos um nó de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. A percentagem é calculada dividindo o número de nós mau estado de funcionamento e o número total de nós no cluster. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de nós. Em grandes clusters, alguns nós sempre será baixo ou horizontalmente para reparações, pelo que esta percentagem deve ser configurada para tolerar que.|
 |   --upgrade-domain-delta-unhealthy-nodes| O máximo permitido percentagem de nós de domínio de atualização de degradação de estado de funcionamento permitida durante as atualizações de cluster. Predefinição: 15. O delta é medido entre o estado de nós do domínio de atualização no início da atualização e o estado de nós do domínio de atualização no momento da avaliação de estado de funcionamento. A verificação é efetuada uma vez concluída cada conclusão de atualização do domínio de atualização para todos os domínios de atualização para se certificar de que o estado dos domínios de atualização está dentro dos limites tolerated.|
 |   --upgrade-domain-timeout              | Tempo limite do domínio de atualização é medido em milissegundos.|
 |   -tempo limite de atualização                     | Tempo limite de atualização é medido em milissegundos.|
 |   --warning-as-error                    | Avisos são tratados com a mesma gravidade como erros.|

### <a name="global-arguments"></a>Argumentos global
|Argumento|Descrição|
| --- | --- |
|--debug                               | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
|    -ajudar -h                             | Mostra esta mensagem de ajuda e saída.|
|    --o de saída                           | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv. Predefinição: json.|
|    – consulta                               | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
|    -verbose                             | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="next-steps"></a>Passos Seguintes
- [A configuração](service-fabric-cli.md) os recursos de infraestrutura do serviço CLI.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).