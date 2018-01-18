---
title: Azure Service Fabric CLI - sfctl compor | Microsoft Docs
description: "Descreve a CLI de recursos de infraestrutura de serviço sfctl compor comandos."
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
ms.openlocfilehash: 0e35ac70125bc640114a4492498b12ea96800d42
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-compose"></a>sfctl compose
Criar, eliminar e gerir implementações de Docker Compose.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
|    criar| Implemente uma aplicação de Service Fabric de um ficheiro de Compose.|
|    lista  | Obtém a lista de compose às implementações criadas no cluster de Service Fabric.|
|   remover| Elimina um recurso de infraestrutura de serviço existente compor a implementação do cluster.|
|   status| Obtém informações sobre um recurso de infraestrutura de serviço compor a implementação.|
|atualização       | Inicia a atualizar uma implementação de compose no cluster de Service Fabric.|
|    upgrade-status| Obtém detalhes para a atualização mais recente efetuada em recursos de infraestrutura neste serviço compõem a implementação.|


## <a name="sfctl-compose-create"></a>Componha sfctl criar
Cria um recurso de infraestrutura de serviço compor a implementação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --caminho do ficheiro [necessário]| Caminho para o ficheiro de Docker Compose de destino.|
 |   --nome de implementação [necessário]| O nome da implementação.|
|    --encrypted-pass             | Em vez de a pedir uma palavra-passe de registo do contentor, utilize uma frase de acesso já encriptado.|
|    --has-pass                   | Pede uma palavra-passe para o registo de contentor.|
|    tempo limite – -t                 | Servidor o tempo limite em segundos.  Predefinição: 60.|
 |   --user                       | Nome de utilizador para estabelecer ligação ao registo do contentor.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug                 | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h               | Mostra esta mensagem de ajuda e saída.|
| --o de saída             | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta                 | Cadeia de consulta JMESPath. Para obter mais informações e exemplos, consulte http://jmespath.org/.|
| -verbose               | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-compose-list"></a>sfctl compor lista
Obtém a lista de compose às implementações criadas no cluster de Service Fabric.

Obtém o estado sobre as implementações de compose que foram criados ou que estão a ser criado no cluster de Service Fabric. A resposta inclui o nome, estado e outros detalhes sobre a implementação de compose. Se as implementações não se enquadram numa página, é devolvida uma página de resultados, bem como um token de continuação, que pode ser utilizado para obter a página seguinte.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --continuation-token| O parâmetro de token de continuação é utilizado para obter o seguinte conjunto de resultados. Um token de continuação com um valor não vazio está incluído na resposta da API de quando os resultados do sistema não se enquadram numa única resposta.      Quando este valor é transmitido para a próxima chamada de API, a API devolve o seguinte conjunto de resultados. Se não existirem resultados adicionais, em seguida, o token de continuação não contém um valor. O valor deste parâmetro não deve ser o URL, codificado.|
| --max-results    | O número máximo de resultados a ser devolvido como parte das consultas paginadas.      Este parâmetro define o limite superior no número de resultados devolvidos.      Se não se enquadram numa mensagem de acordo com as restrições de tamanho máximo de mensagem definidas na configuração, os resultados devolvidos podem ser menor que o máximo de resultados especificado. Se este parâmetro for zero ou não especificado, as consultas paginadas incluem resultados tantos possível ajustar na mensagem de retorno.|
| tempo limite – -t     | Servidor o tempo limite em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug          | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h        | Mostra esta mensagem de ajuda e saída.|
| --o de saída      | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta          | Cadeia de consulta JMESPath. Para obter mais informações e exemplos, consulte http://jmespath.org/.|
| -verbose        | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-compose-remove"></a>Remova de compor sfctl
Elimina um recurso de infraestrutura de serviço existente compor a implementação do cluster.

Elimina um recurso de infraestrutura de serviço existente compor a implementação. 

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário]| A identidade da implementação. Isto é, geralmente, o nome completo da aplicação sem a ' recursos de infraestrutura:' esquema de URI.|
| tempo limite – -t            | Servidor o tempo limite em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug                 | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h               | Mostra esta mensagem de ajuda e saída.|
| --o de saída             | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta                 | Cadeia de consulta JMESPath. Para obter mais informações e exemplos, consulte http://jmespath.org/.|
| -verbose               | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-compose-status"></a>sfctl compor Estado
Obtém informações sobre um recurso de infraestrutura de serviço compor a implementação.

Devolve o estado de implementação que foi criado ou no processo a ser criada no Service Fabric do cluster e cujo nome corresponde à especificado como o parâmetro de compose. A resposta inclui o nome, estado e outros detalhes sobre a aplicação.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| --nome de implementação [necessário]| A identidade da implementação. |
| tempo limite – -t            | Servidor o tempo limite em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug                 | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h               | Mostra esta mensagem de ajuda e saída.|
| --o de saída             | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta                 | Cadeia de consulta JMESPath. Para obter mais informações e exemplos, consulte http://jmespath.org/.|
| -verbose               | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-compose-upgrade"></a>sfctl compor atualização
Inicia a atualizar uma implementação de compose no cluster de Service Fabric.

Valida os parâmetros de atualização fornecidos e começa a atualizar a implementação.

### <a name="arguments"></a>Argumentos
|Argumento|Descrição|
| --- | --- |
|    --caminho do ficheiro [necessário]| Caminho para o ficheiro de Docker Compose de destino.|
|    --nome de implementação [necessário]| O nome da implementação.|
|    --default-svc-type-health-map| JSON codificado dicionário que descreve a política de estado de funcionamento utilizada para avaliar o estado de funcionamento dos serviços.|
|    --encrypted-pass             | Em vez de a pedir uma palavra-passe de registo do contentor, utilize uma frase de acesso já encriptado.|
 |   -Falha de ação             | Os valores possíveis incluem: 'Inválido,' 'Reversão', 'Manual'.|
|    --force-restart              | Força o reinício.|
 |   --has-pass                   | Pede uma palavra-passe para o registo de contentor.|
|    --health-check-retry         | Tempo limite de tentativas de verificação do Estado de funcionamento é medido em milissegundos.|
|    --health-check-stable        | Estado de funcionamento Consulte duração estável, medida em milissegundos.|
|    --health-check-wait          | Duração de espera de verificação do Estado de funcionamento é medido em milissegundos.|
|    --replica-set-check          | Réplica atualização definir o limite de tempo de verificação medido em segundos.|
|    --svc-type-health-map        | JSON codificado lista de objetos que descrevem as políticas de estado de funcionamento utilizadas para avaliar o estado de funcionamento dos tipos de serviço diferente.|
|    tempo limite – -t                 | Servidor o tempo limite em segundos.  Predefinição: 60.|
|    --unhealthy-app              | O máximo permitido percentagem das aplicações mau estado de funcionamento antes de o comunicar um erro.        Por exemplo, para permitir a 10% de aplicações para ser mau estado de funcionamento, este valor será 10. A percentagem representa a percentagem máxima de tolerated das aplicações que podem ser mau estado de funcionamento antes do cluster for considerado no erro. Se a percentagem é respeitada, mas existir pelo menos uma aplicação de mau estado de funcionamento, o estado de funcionamento é avaliado como aviso. Esta percentagem é calculada dividindo o número de aplicações mau estado de funcionamento e o número total de instâncias de aplicações no cluster.|
|    --upgrade-domain-timeout     | Tempo limite do domínio de atualização é medido em milissegundos.|
|    -tipo de atualização               | Predefinição: a anular.|
|    -modo de atualização               | Os valores possíveis incluem: 'Inválido', 'UnmonitoredAuto', 'UnmonitoredManual', 'Monitorizado'.  Predefinição: UnmonitoredAuto.|
|    -tempo limite de atualização            | Atualização tempo limite medido em milissegundos.|
|    --user                       | Nome de utilizador para estabelecer ligação ao registo do contentor.|
|    --warning-as-error           | Avisos são tratados com a mesma gravidade como erros.|

### <a name="global-arguments"></a>Argumentos global
 |Argumento|Descrição|
| --- | --- |
|   --debug                      | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
|    -ajudar -h                    | Mostra esta mensagem de ajuda e saída.|
 |   --o de saída                  | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.
                                   Predefinição: json.|
 |   – consulta                      | Cadeia de consulta JMESPath. Consulte http://jmespath.org/ para obter mais informações e exemplos.|
 |   -verbose                    | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).