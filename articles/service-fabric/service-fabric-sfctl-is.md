---
title: "Azure Service Fabric CLI - sfctl é | Microsoft Docs"
description: "Descreve a CLI de recursos de infraestrutura de serviço sfctl é comandos."
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
ms.openlocfilehash: b611a447dd6669a09ca16c816de74acd7f3e8c7e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl-is"></a>sfctl é
Consultar e enviar comandos para o serviço de infraestrutura.

## <a name="commands"></a>Comandos

|Comando|Descrição|
| --- | --- |
|    Comando| Invoca um comando administrativo na instância do serviço de infraestrutura especificada.|
|    consulta  | Invoca uma consulta de só de leitura na instância do serviço de infraestrutura especificada.|


## <a name="sfctl-is-command"></a>sfctl é comando
Invoca um comando administrativo na instância do serviço de infraestrutura especificada.

Para os clusters que tem uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma para enviar comandos específicos da infraestrutura para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código. .

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -comando [necessário]| O texto do comando para ser invocado. O conteúdo do comando é específica da infraestrutura.  Predefinição: é o comando.|
| --service-id     | A identidade do serviço de infraestrutura. Este é o nome completo do serviço infraestrutura sem o ' recursos de infraestrutura:' esquema de URI. Este parâmetro necessário apenas para os clusters que tem mais do que uma instância do serviço de infraestrutura está em execução.|
| tempo limite – -t     | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug          | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h        | Mostra esta mensagem de ajuda e saída.|
| --o de saída      | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta          | Cadeia de consulta JMESPath. Para obter mais informações e exemplos, consulte http://jmespath.org/.|
| -verbose        | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="sfctl-is-query"></a>sfctl consulta é
Invoca uma consulta de só de leitura na instância do serviço de infraestrutura especificada.

Para os clusters que tem uma ou mais instâncias do serviço de infraestrutura configurado, esta API fornece uma forma para enviar consultas de infraestrutura específico para uma instância específica da infraestrutura de serviço. Os formatos de resposta correspondente e comandos disponíveis variam consoante a infraestrutura em que o cluster está em execução. Esta API suporta a plataforma de Service Fabric; não se destina a ser utilizado diretamente a partir do seu código.

### <a name="arguments"></a>Argumentos

|Argumento|Descrição|
| --- | --- |
| -comando [necessário]| O texto do comando para ser invocado. O conteúdo do comando é específica da infraestrutura.  Predefinição: se a consulta.|
| --service-id     | A identidade do serviço de infraestrutura. Este é o nome completo do serviço infraestrutura sem o ' recursos de infraestrutura:' esquema de URI. Este parâmetro é necessário apenas para os clusters que tem mais do que uma instância do serviço de infraestrutura está em execução.|
| tempo limite – -t     | Tempo limite do servidor em segundos.  Predefinição: 60.|

### <a name="global-arguments"></a>Argumentos global

|Argumento|Descrição|
| --- | --- |
| --debug          | Aumente a verbosidade do registo para mostrar que todos os registos de depuração.|
| -ajudar -h        | Mostra esta mensagem de ajuda e saída.|
| --o de saída      | Formato de saída.  Valores permitidos: json, jsonc, tabela, tsv.  Predefinição: json.|
| – consulta          | Cadeia de consulta JMESPath. Para obter mais informações, consulte http://jmespath.org/.|
| -verbose        | Aumente a verbosidade do registo. Utilize - a depuração para os registos de depuração completas.|

## <a name="next-steps"></a>Passos Seguintes
- [Configurar](service-fabric-cli.md) a CLI de recursos de infraestrutura de serviço.
- Saiba como utilizar a CLI de recursos de infraestrutura de serviço utilizando o [scripts de exemplo](/azure/service-fabric/scripts/sfctl-upgrade-application).