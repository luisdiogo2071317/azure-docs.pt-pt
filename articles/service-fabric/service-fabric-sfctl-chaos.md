---
title: Azure Service Fabric CLI - sfctl choas | Microsoft Docs
description: Descreve os comandos de chaos sfctl CLI de recursos de infraestrutura de serviço.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: 34e4d47b1de509c2053996d9d1078733d7055447
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
# <a name="sfctl-chaos"></a>sfctl chaos
Iniciar, parar e relatórios sobre o serviço de teste chaos.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
|    relatório| Obtém o segmento seguinte do relatório Chaos baseado no token de continuação transmitido na ou o transmitido no-intervalo de tempo.|
|    start | Inicia Chaos no cluster.|
|    Parar  | Interrompe Chaos do cluster se já está em execução, caso contrário faz nada.|


## <a name="sfctl-chaos-report"></a>relatório de chaos sfctl
Obtém o segmento seguinte do relatório Chaos baseado no token de continuação transmitido na ou o transmitido no-intervalo de tempo.

Pode especificar o ContinuationToken para obter o segmento seguinte do relatório Chaos ou pode especificar o intervalo de tempo através de StartTimeUtc e EndTimeUtc, mas não é possível especificar o ContinuationToken e o intervalo de tempo na mesma chamada. Quando existe mais do que 100 eventos de Chaos, é devolvido o relatório de Chaos em segmentos em que um segmento contém mais do que 100 eventos de Chaos. 

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -token de continuação| O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta. Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado.|
| – hora de fim-utc   | Tempo que representa a hora de fim do intervalo de tempo para o qual um relatório de Chaos é para ser gerado de ficheiros do Windows. Consulte [DateTime.ToFileTimeUtc método](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) para obter mais detalhes.|
| -utc de hora de início | Tempo que representa a hora de início do intervalo de tempo para o qual um relatório de Chaos é para ser gerado de ficheiros do Windows. Consulte [DateTime.ToFileTimeUtc método](https://msdn.microsoft.com/library/system.datetime.tofiletimeutc(v=vs.110).aspx) para obter mais detalhes.|
| tempo limite – -t     | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug          | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h        | Mostra esta mensagem de ajuda e saída.|
| --o de saída      | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta          | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose        | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-chaos-start"></a>início de chaos sfctl
Inicia Chaos no cluster.

Se Chaos não está já em execução no cluster, começa Chaos pelo transmitido nos parâmetros de Chaos. Se Chaos já está em execução quando esta chamada é efetuada, a chamada falha com o código de erro FABRIC_E_CHAOS_ALREADY_RUNNING.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| aplicações--tipo-estado de funcionamento-política-mapa  | JSON codificado lista com aplicações mau estado de funcionamento da percentagem de máximo para tipos de aplicação específica. Cada entrada especifica como uma chave de nome de tipo de aplicação e como um valor de número inteiro que representa a percentagem de MaxPercentUnhealthyApplications utilizada para avaliar as aplicações do tipo de aplicação especificado. Define um mapa com aplicações mau estado de funcionamento da percentagem de máximo para tipos de aplicação específica. Cada entrada especifica como o nome do tipo de aplicação de chave e como valor de número inteiro que representa a percentagem de MaxPercentUnhealthyApplications utilizada para avaliar as aplicações do tipo de aplicação especificado. O mapa de política de estado de funcionamento de tipo de aplicação pode ser utilizado durante a avaliação de estado de funcionamento do cluster para descrever os tipos de aplicações especiais. Os tipos de aplicações incluídos no mapa são avaliados em relação a percentagem especificada no mapa e não com o MaxPercentUnhealthyApplications global definido na política de estado de funcionamento de cluster. As aplicações de tipos de aplicação especificados no mapa não são contabilizadas contra o conjunto global de aplicações. Por exemplo, se algumas aplicações de um tipo crítico, o administrador do cluster pode adicionar uma entrada para o mapa para esse tipo de aplicação e atribua-lhe um valor de 0% (ou seja, não tolerar eventuais falhas). Todas as outras aplicações podem ser avaliadas com MaxPercentUnhealthyApplications definido como 20% para tolerar algumas falhas fora milhares de instâncias da aplicação. O mapa de política de estado de funcionamento de tipo de aplicação só é utilizado se o manifesto do cluster permite a avaliação de estado de funcionamento de tipo de aplicação com a entrada de configuração para HealthManager/EnableApplicationTypeHealthEvaluation.|
|--chaos-target-filter         | JSON codificado dicionário com duas chaves de tipo de cadeia. As duas chaves são NodeTypeInclusionList e ApplicationInclusionList. Os valores para ambas estas chaves são lista de cadeias. chaos_target_filter define todos os filtros para destino Chaos falhas, por exemplo, originar falha apenas determinados tipos de nó ou originar falha apenas determinadas aplicações. Se não for utilizado chaos_target_filter, Chaos faults todas as entidades de cluster. Se for utilizado chaos_target_filter, Chaos faults apenas as entidades que cumprem a especificação de chaos_target_filter. NodeTypeInclusionList e ApplicationInclusionList permitem apenas uma União semântica. Não é possível especificar uma intersecção de NodeTypeInclusionList e ApplicationInclusionList. Por exemplo, não é possível especificar "falhas desta aplicação apenas quando é nesse tipo de nó." Depois de uma entidade está incluída no NodeTypeInclusionList ou ApplicationInclusionList, essa entidade não é possível excluir ChaosTargetFilter. Mesmo se applicationX não for apresentada ApplicationInclusionList, em algumas iteração Chaos applicationX pode falhar porque e estar num nó de nodeTypeY que está incluído na NodeTypeInclusionList. Se NodeTypeInclusionList e ApplicationInclusionList estiverem vazia, é emitida uma ArgumentException. Todos os tipos de falhas de hardware (reiniciar nó, reinicie o pacote do código, remover a réplica, reiniciar a réplica, mover primário e mover secundário) estão ativadas para os nós dos seguintes tipos de nó. Se um tipo de nó (diga NodeTypeX) não for apresentada NodeTypeInclusionList, em seguida, falhas de nível de nó (como NodeRestart) nunca irão estar ativadas para os nós do NodeTypeX, mas a falhas de pacote e a réplica de código ainda podem ser ativadas para NodeTypeX se uma aplicação no ApplicationInclusionList acontece residam num nó do NodeTypeX. No máximo 100 nó os nomes de tipo podem ser incluídos nesta lista, para aumentar este número, uma atualização da configuração é necessária para a configuração de MaxNumberOfNodeTypesInChaosEntityFilter. Todas as réplicas que pertencem aos serviços destas aplicações são propensos a falhas de réplica (reinício réplica, remova réplica, move, principais e mover secundários) por Chaos. Chaos pode reiniciar um pacote do código apenas se o pacote do código aloja réplicas apenas destas aplicações. Se uma aplicação não for apresentada nesta lista,-lo pode ainda ser falhou em alguns iteração Chaos se a aplicação termine a cópia de segurança num nó de um tipo de nó que está incluído na NodeTypeInclusionList. No entanto se applicationX está associada ao nodeTypeY através de restrições de posicionamento e applicationX está ausente da ApplicationInclusionList e nodeTypeY está ausente da NodeTypeInclusionList, em seguida, applicationX irá nunca falhar. No máximo, os nomes das aplicações de 1000 podem ser incluídas nesta lista, para aumentar este número, uma atualização da configuração é necessária para a configuração de MaxNumberOfApplicationsInChaosEntityFilter.|
|--context                     | Mapa JSON codificado de (cadeia, cadeia) escreva os pares chave-valor. O mapa pode ser utilizado para registar informações sobre a execução de Chaos. Não é possível existir mais do que 100 essas pares e cada cadeia (chave ou valor) pode ter no máximo 4095 carateres de comprimento. Este mapa está definido pelo arranque de Chaos executar armazenar opcionalmente o contexto sobre a execução específico.|
| --disable-move-replica-faults | Desativa o principal de mover e mover falhas secundárias.|
| -estabilização de cluster máx.| A quantidade máxima de tempo de espera para todas as entidades para se tornam estáveis e em bom estado de cluster.  Predefinição: 60. Chaos executa no iterações e no início de cada iteração valida o estado de funcionamento de entidades de cluster. Durante a validação de uma entidade de cluster não está estável e bom estado de funcionamento no MaxClusterStabilizationTimeoutInSeconds, Chaos gera um evento de falha de validação.|
| --max-concurrent-faults    | O número máximo de falhas em simultâneo induzida por iteração.           Predefinição: 1. Chaos executa no iterações e dois iterações consecutivas são separadas por uma fase de validação. Quanto maior for a simultaneidade, mais agressiva a injeção de falhas – inducing série mais complexa de Estados para descobrir erros. A recomendação é começar com um valor de 2 ou 3 e tenha cuidado ao mover a cópia de segurança.|
| --max-percent-unhealthy-apps  | Quando avaliar o estado de funcionamento do cluster durante a Chaos, o máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro. Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster, excluindo as aplicações de tipos de aplicação que estão incluídas no ApplicationTypeHealthPolicyMap. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de aplicações. Percentagem predefinida é zero.|
| --max-percent-unhealthy-nodes | Quando avaliar o estado de funcionamento do cluster durante a Chaos, o máximo permitido percentagem de nós mau estado de funcionamento antes de o comunicar um erro. O máximo permitido de percentagem de nós mau estado de funcionamento antes de o comunicar um erro. Por exemplo, para permitir a 10% de nós incorreto, este valor será 10. A percentagem representa a percentagem máxima de tolerated de nós que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos um nó de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. A percentagem é calculada dividindo o número de nós mau estado de funcionamento e o número total de nós no cluster. O cálculo Arredonda por excesso para tolerar uma falha num pequeno número de nós. Percentagem predefinida é zero. Em grandes clusters, alguns nós sempre será baixo ou horizontalmente para reparações, pelo que esta percentagem deve ser configurada para tolerar que.|
| -tempo para execução              | Total de tempo (em segundos) para o qual Chaos será executado antes de parar automaticamente. O valor máximo permitido é 4.294.967.295 (System.UInt32.MaxValue).  Predefinição: 4294967295.|
| tempo limite – -t               | Tempo limite do servidor em segundos.  Predefinição: 60.|
| -espera tempo entre falhas | De tempo de espera (em segundos) entre falhas consecutivas dentro de uma único iteração.  Predefinição: 20. Quanto maior for o valor, o inferior a sobreposição entre falhas e o mais simples a sequência de estado passa que atravessa o cluster. A recomendação está a começar com um valor entre 1 e 5 e exercício cuidado ao mover a cópia de segurança.|
| -espera tempo entre iterações| Tempo-separação (em segundos) entre duas iterações consecutivas de Chaos. Quanto maior for o valor, inferior a taxa de inserção de falhas. Predefinição: 30.|
| --warning-as-error         | Quando avaliar o estado de funcionamento do cluster durante a Chaos, processe avisos com a mesma gravidade como erros.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug                    | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h                  | Mostra esta mensagem de ajuda e saída.|
| --o de saída                | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.           Predefinição: json.|
| – consulta                    | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose                  | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-chaos-stop"></a>parar de chaos sfctl
Interrompe Chaos do cluster se já está em execução, caso contrário faz nada.

Chaos interrompe o processo de agendamento mais falhas; No entanto, falhas em trânsito não são afetadas.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| tempo limite – -t| Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| -debug  | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h| Mostra esta mensagem de ajuda e saída.|
| --o de saída | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta  | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
| -verbose| Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="next-steps"></a>Passos Seguintes
- [A configuração](service-fabric-cli.md) os recursos de infraestrutura do serviço CLI.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).