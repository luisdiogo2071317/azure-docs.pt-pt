---
title: "Erro e excepção a processar para Logic Apps no Azure | Microsoft Docs"
description: "Padrões de erro e excepção a processar a Logic Apps."
services: logic-apps
documentationcenter: 
author: dereklee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 01/31/2018
ms.author: deli; LADocs
ms.openlocfilehash: 91819d0fba30dd2ada981435fa13b8ae0a7fcc45
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Processar os erros e exceções em aplicações lógicas

Adequadamente o período de indisponibilidade de processamento ou problemas de sistemas dependentes podem constituir um desafio para qualquer arquitetura de integração. Para criar integrações robustas que são resilientes contra problemas e falhas, as Logic Apps oferece uma experiência de primeira classe para processamento de erros e exceções. 

## <a name="retry-policies"></a>Repita políticas

Para mais básica exceções e processamento de erros, pode utilizar a política de repetição. Se um pedido inicial foi excedido ou falha, que é qualquer pedido que resulta num 429 ou resposta 5xx, esta política define se e como a ação repete o pedido. 

Existem quatro tipos de políticas de repetição: predefinida, none, fixos e de intervalo exponencial. Se a definição de fluxo de trabalho não tiver uma política de repetição, a política predefinida, conforme definido pelo serviço, é utilizada em vez disso.

Para configurar as políticas de repetição, se aplicável, abra o estruturador de aplicação lógica para a sua aplicação lógica e aceda a **definições** para uma ação específica na sua aplicação lógica. Em alternativa, pode definir as políticas de repetição no **entradas** secção para um acionador, se não permanente na sua definição de fluxo de trabalho ou ação específica. Segue-se a sintaxe geral:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Para obter mais informações sobre a sintaxe e o **entradas** secção, consulte o [secção política de repetição ações de fluxo de trabalho e é acionado][retryPolicyMSDN]. Para obter informações sobre as limitações de política de repetição, consulte [limites Logic Apps e a configuração](../logic-apps/logic-apps-limits-and-config.md). 

### <a name="default"></a>Predefinição

Quando não definir uma política de repetição no **retryPolicy** secção, a sua aplicação lógica utiliza a política predefinida, que é um [política intervalo exponencial](#exponential-interval) que envia exponencialmente até quatro tentativas em aumentar intervalos são ampliados por 7.5 segundos. O intervalo é limitado entre 5 e 45 segundos. Esta política é equivalente a política neste exemplo, definição de fluxo de trabalho HTTP:

```json
"HTTP": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {}
}
```

### <a name="none"></a>Nenhuma

Se definir **retryPolicy** para **nenhum**, esta política não repete a ação pedidos falhados.

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| tipo | Sim | Cadeia | **Nenhum** | 
||||| 

### <a name="fixed-interval"></a>Intervalo fixo

Se definir **retryPolicy** para **fixo**, esta política repete a um pedido falhado por a aguardar o intervalo de tempo antes de enviar o pedido seguinte especificado.

| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| tipo | Sim | Cadeia | **fixed** |
| contagem | Sim | Número inteiro | O número de tentativas de repetição, que têm de ser entre 1 e 90 | 
| intervalo | Sim | Cadeia | O intervalo de repetição em [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), que tem de estar entre PT5S e PT1D | 
||||| 

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>Intervalo exponencial

Se definir **retryPolicy** para **exponencial**, esta política repete a um pedido falhado após um intervalo de tempo aleatório de um intervalo de exponencialmente crescente. A política também garante que enviar cada tentativa de repetição num intervalo aleatório que é superior ao **minimumInterval** e inferior a **maximumInterval**. Necessitam de políticas exponencial **contagem** e **intervalo**, enquanto que os valores para **minimumInterval** e **maximumInterval** são opcionais. Se quiser substituir os valores predefinidos de PT5S e PT1D respetivamente, pode adicionar estes valores.

| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| tipo | Sim | Cadeia | **exponential** |
| contagem | Sim | Número inteiro | O número de tentativas de repetição, que têm de ser entre 1 e 90  |
| intervalo | Sim | Cadeia | O intervalo de repetição em [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), que tem de estar entre PT5S e PT1D. |
| minimumInterval | Não | Cadeia | O intervalo mínimo de repetição no [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), que tem de estar entre PT5S e **intervalo** |
| maximumInterval | Não | Cadeia | O intervalo mínimo de repetição no [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations), que tem de estar entre **intervalo** e PT1D | 
||||| 

Esta tabela mostra como uma variável de aleatório uniform no intervalo indicado é gerada para cada repetição até e incluindo **contagem**:

**Intervalo de variável aleatório**

| Repetir número | Intervalo mínimo | Intervalo máximo |
| ------------ | ---------------- | ---------------- |
| 1 | Max(0, **minimumInterval**) | Min(interval, **maximumInterval**) |
| 2 | Max(interval, **minimumInterval**) | Min(2 * interval, **maximumInterval**) |
| 3 | Número máximo (2 * intervalo, **minimumInterval**) | Min(4 * interval, **maximumInterval**) |
| 4 | Máx. (4 * intervalo, **minimumInterval**) | Min(8 * interval, **maximumInterval**) |
| .... | | | 
|||| 

## <a name="catch-and-handle-failures-with-the-runafter-property"></a>Detetar e processar falhas com a propriedade RunAfter

Cada ação de aplicação lógica declara as ações que devem ser concluído antes de começa a ação, semelhante à forma como especificar a ordem dos passos no fluxo de trabalho. Na definição de uma ação, o **runAfter** propriedade define esta ordenação e é um objeto que descreve quais as ações e Estados de ação executar a ação.

Por predefinição, todas as ações que adicionar no Designer de aplicação lógica estão definidas para executar após o passo anterior, quando o resultado do passo anterior é **com êxito**. No entanto, pode personalizar o **runAfter** valor, para que as ações acionados quando as ações anteriores resultam como **falha**, **ignorados**, ou uma combinação destes valores. Por exemplo, para adicionar um item para um tópico de barramento de serviço específico após específico **Insert_Row** ação falhar, pode utilizar este exemplo **runAfter** definição:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

O **runAfter** propriedade está definida para ser executada quando o **Insert_Row** estado da ação é **falha**. Para executar a ação, se o estado da ação **com êxito**, **falha**, ou **ignorados**, utilizam esta sintaxe:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Ações que executaram e concluir com êxito após ter uma ação anterior tenha falhado, estão marcados como **com êxito**. Este comportamento significa que se com êxito catch todas as falhas no fluxo de trabalho, a execução em si é marcado como **com êxito**.

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>Avaliar ações com âmbitos e os respetivos resultados

Semelhante ao executar passos após ações individuais com o **runAfter** propriedade, pode agrupar ações em conjunto no interior de um [âmbito](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md). Pode utilizar âmbitos quando pretender logicamente agrupar ações, avaliar o estado agregado de âmbito e executar ações com base no estado. Depois de todas as ações num âmbito termine a execução, o âmbito próprio obtém o seu próprio Estado. 

Para verificar o estado de um âmbito, pode utilizar os mesmos critérios que utilizar para verificar o estado de execução de uma aplicação lógica, tais como **com êxito**, **falha**, e assim sucessivamente. 

Por predefinição, quando for bem-sucedida ações do âmbito, o âmbito é marcado **com êxito**. Se a ação final num âmbito resulta como **falha** ou **abortado**, estado do âmbito está marcado como **falha**. 

Para detetar exceções num **falha** âmbito e executadas ações que processar esses erros, pode utilizar o **runAfter** propriedade para esse **falha** âmbito. Dessa forma, se *qualquer* falha nas ações no âmbito e utilizar o **runAfter** propriedade para esse âmbito, pode criar uma única ação para detetar falhas.

Para os limites nos âmbitos de mensagens em fila, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md).

### <a name="get-context-and-results-for-failures"></a>Obter o contexto e os resultados para falhas

Embora a deteção de falhas de um âmbito é útil, pode também querer contexto para ajudar a compreender exatamente as ações de falha juntamente com quaisquer erros ou códigos de estado que foram devolvidos. O  **@result()** função de fluxo de trabalho fornece o contexto sobre o resultado de todas as ações de um âmbito.

O  **@result()** função aceita um parâmetro único (nome do âmbito) e devolve uma matriz de todos os ação resultados de dentro desse âmbito. Estes objetos de ação incluem os mesmos atributos como a  **@actions()** objeto, tal como a ação a hora de início, hora de fim, o estado, entradas, IDs de correlação e saídas. Para enviar o contexto para todas as ações que falha dentro de um âmbito, emparelhar facilmente um  **@result()** funcionar com uma **runAfter** propriedade.

Para executar uma ação *para cada* ação num âmbito que tenha um **falha** resultado, e para filtrar a matriz de resultados para as ações de falha, pode ser emparelhado  **@result()** com um  **[filtro matriz](../connectors/connectors-native-query.md)**  ação e um  **[ForEach](../logic-apps/logic-apps-control-flow-loops.md)**  ciclo. Pode colocar a matriz de resultado filtrados e efetuar uma ação para cada falha utilizando o **ForEach** ciclo. 

Eis um exemplo, seguido por uma explicação detalhada, que envia um pedido POST de HTTP com o corpo da resposta de todas as ações que falhou no âmbito "My_Scope":

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Eis as instruções detalhadas que descreve o que acontece neste exemplo:

1. Para obter o resultado de todas as ações dentro "My_Scope", o **filtro matriz** filtros de ação  **@result('My_Scope')**.

2. A condição de **filtro matriz** alguma  **@result()** item que tem um Estado igual a **falha**. Esta condição filtra a matriz de todos os ação resultados de "My_Scope" para uma matriz com apenas os resultados de ação de falha.

3. Efetuar uma **para cada** cíclicas ação no *matriz filtrado* produz. Este passo executa uma ação *para cada* falhou o resultado da ação que foi filtrado anteriormente.

   Se a falha de uma única ação no âmbito, as ações no **foreach** execute apenas uma vez. 
   Várias ações falhadas causam uma ação por falha.

4. Enviar um HTTP POST **foreach** do item de corpo de resposta, o que é  **@item() ['saídas'] ['corpo']**. O  **@result()** forma item é igual a  **@actions()** formam e pode ser analisado da mesma forma.

5. Inclua dois cabeçalhos personalizados com o nome da ação falhada  **@item() ['name']** e a executar o cliente ID de controlo  **@item() ['clientTrackingId']**.

Para referência, eis um exemplo de um único  **@result()** item, que mostra o **nome**, **corpo**, e **clientTrackingId** propriedades que são analisadas no exemplo anterior. Fora de um **foreach** ação,  **@result()** devolve uma matriz destes objetos.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Para efetuar diferentes excepção a processar a padrões, pode utilizar as expressões descritas anteriormente neste artigo. Pode optar por executar uma única excepção a processar a ação fora do âmbito que aceita a matriz filtrada completa de falhas e remova o **foreach** ação. Também pode incluir outras propriedades útil do  **@result()** resposta conforme descrita anteriormente.

## <a name="azure-diagnostics-and-telemetry"></a>Telemetria e diagnóstico do Azure

Os padrões de anteriores são ótimos processar erros e exceções dentro de uma execução, mas também pode identificar e respondem a erros independentes a execução de si próprio. 
[Diagnóstico do Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) fornece uma forma simples para enviar todos os eventos de fluxo de trabalho, incluindo todos os Estados e executar a ação, para uma conta de armazenamento do Azure ou de um hub de eventos criados com o Event Hubs do Azure. 

Para avaliar os Estados da execução, pode monitorizar os registos e as métricas ou publicá-los em qualquer ferramenta monitorização que preferir. Uma opção de potencial é transmitido em fluxo através do Event Hubs para todos os eventos [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). No Stream Analytics, pode escrever consultas em direto com base em qualquer anomalias, médias ou falhas dos registos de diagnóstico. Pode utilizar o Stream Analytics para enviar informações para outras origens de dados, tais como filas, tópicos, SQL Server, base de dados do Azure Cosmos ou Power BI.

## <a name="next-steps"></a>Passos Seguintes

* [Veja como um cliente baseia-se com Azure Logic Apps de processamento de erros](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Localizar mais cenários e exemplos de Logic Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Saiba como criar implementações automatizadas para aplicações lógicas](../logic-apps/logic-apps-create-deploy-template.md)
* [Criar e implementar aplicações lógicas com o Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9