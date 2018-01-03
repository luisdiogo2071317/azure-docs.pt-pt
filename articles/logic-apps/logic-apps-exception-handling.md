---
title: "Erro e excepção a processar para Logic Apps no Azure | Microsoft Docs"
description: "Padrões de erro e excepção a processar a Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Processar os erros e exceções em aplicações lógicas

As Logic Apps no Azure fornece ferramentas avançadas e padrões para o ajudar a garantir que o integrações são resilientes contra falhas e robusto. Qualquer arquitetura de integração quê o desafio de processar corretamente um período de indisponibilidade ou problemas de sistemas dependentes. As Logic Apps faz com que o processamento de erros uma experiência de primeira classe. Fornece as ferramentas que necessárias para agir sobre exceções e erros nos seus fluxos de trabalho.

## <a name="retry-policies"></a>Repita políticas

Uma política de repetição é o tipo de exceção e processamento de erros mais básico. Se um pedido inicial expirar ou falhar (qualquer pedido que resulte num 429 ou resposta 5xx), uma política de repetição define se e como a ação deve ser repetida. 

Existem quatro tipos de políticas de repetição: predefinida, none, fixos e de intervalo exponencial. Se uma política de repetição não é fornecida na definição de fluxo de trabalho, a política predefinida, conforme definido pelo serviço é utilizada. 

Pode configurar as políticas de repetição no *entradas* para uma ação específica ou acionador se se tratar de repetição. Da mesma forma, pode configurar as políticas de repetição (se aplicável) no Designer de aplicação lógica. Para configurar uma política de repetição, no Designer de aplicação lógica, aceda a **definições** para uma ação específica.

Para obter informações sobre as limitações das políticas de repetição, consulte [limites Logic Apps e a configuração](../logic-apps/logic-apps-limits-and-config.md). Para obter mais informações sobre a sintaxe suportado, consulte o [repetir a secção política ações de fluxo de trabalho e é acionado][retryPolicyMSDN].

### <a name="default"></a>Predefinição

Se não definir uma política de repetição (**retryPolicy** não está definido), a política predefinida é utilizada. A política predefinida é uma política de intervalo exponencial que envia até quatro tentativas, ao aumentar exponencialmente intervalos ampliados por 7.5 segundos. O intervalo está limitado a entre 5 e 45 segundos. Esta política predefinida é equivalente a política neste exemplo, definição de fluxo de trabalho HTTP:

```json
"HTTP":
{
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
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>Nenhuma

Se **retryPolicy** está definido como **nenhum**, um pedido falhado não for repetido.

| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| tipo | Sim | Cadeia | **nenhum** |

### <a name="fixed-interval"></a>Intervalo fixo

Se **retryPolicy** está definido como **fixo**, a política repete a um pedido falhado por a aguardar o intervalo de tempo antes de enviar o pedido seguinte especificado.

| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| tipo | Sim | Cadeia | **fixo** |
| contagem | Sim | Número inteiro | Número de tentativas de repetição. Tem de ser entre 1 e 90. |
| intervalo | Sim | Cadeia | Repita o intervalo em [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Tem de estar entre PT5S e PT1D. |

### <a name="exponential-interval"></a>Intervalo exponencial

Se **retryPolicy** está definido como **exponencial**, a política repete a um pedido falhado após um intervalo de tempo aleatório de um intervalo de exponencialmente crescente. Cada tentativa de repetição é garantida a serem enviados num intervalo aleatório que é superior ao **minimumInterval** e inferior a **maximumInterval**. Uma variável de aleatório uniform no intervalo indicado na tabela seguinte é gerada para cada tentativas até e incluindo **contagem**:

**Intervalo de variável aleatório**

| Repetir número | Intervalo mínimo | Intervalo máximo |
| ------------ |  ------------ |  ------------ |
| 1 | Máx. (0, **minimumInterval**) | Min (intervalo, **maximumInterval**) |
| 2 | Máx. (intervalo, **minimumInterval**) | Mín. (2 * intervalo, **maximumInterval**) |
| 3 | Número máximo (2 * intervalo, **minimumInterval**) | Min (4 * intervalo, **maximumInterval**) |
| 4 | Máx. (4 * intervalo, **minimumInterval**) | Min (8 * intervalo, **maximumInterval**) |
| ... |

Para as políticas de tipo exponencial **contagem** e **intervalo** são necessários. Os valores de **minimumInterval** e **maximumInterval** são opcionais. Pode adicionar ao substituir os valores predefinidos de PT5S e PT1D, respetivamente.

| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| tipo | Sim | Cadeia | **exponencial** |
| contagem | Sim | Número inteiro | Número de tentativas de repetição. Tem de ser entre 1 e 90.  |
| intervalo | Sim | Cadeia | Repita o intervalo em [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Tem de estar entre PT5S e PT1D. |
| minimumInterval | Não | Cadeia | Repita o intervalo mínimo na [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Tem de estar entre PT5S e **intervalo**. |
| maximumInterval | Não | Cadeia | Repita o intervalo mínimo na [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Tem de estar entre **intervalo** e PT1D. |

## <a name="catch-failures-with-the-runafter-property"></a>Detetar falhas com a propriedade runAfter

Cada ação de aplicação lógica declara que ações devem ser concluído antes do início da ação. É semelhante a ordenação os passos no fluxo de trabalho. Na definição de ação, esta ordem é conhecido como o **runAfter** propriedade. 

O **runAfter** propriedade é um objeto que descreve quais as ações e Estados de ação executar a ação. Por predefinição, todas as ações que adicionou ao utilizar o Designer de aplicação lógica são configuradas para ser executado após o passo anterior, se o resultado do passo anterior for **com êxito**. 

No entanto, pode personalizar o **runAfter** valor a acionar ações quando ações anteriores têm um resultado de **falha**, **ignorados**, ou um conjunto possíveis destes valores. Se pretender adicionar um item para um tópico do Service Bus do Azure designado após uma ação específica **Insert_Row** falhar, pode utilizar o seguinte **runAfter** configuração:

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

Tenha em atenção que **runAfter** está definido para acionar se o **Insert_Row** resultado de ação é **falha**. Para executar a ação, se o estado da ação **com êxito**, **falha**, ou **ignorados**, utilizam esta sintaxe:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Ações que executaram e concluir com êxito após ter uma ação anterior tenha falhado estão marcadas como **com êxito**. Isto significa que se com êxito catch todas as falhas no fluxo de trabalho, a execução em si é marcado como **com êxito**.

## <a name="scopes-and-results-to-evaluate-actions"></a>Âmbitos e resultados avaliar ações

Pode agrupar ações em conjunto no interior de um [âmbito](../logic-apps/logic-apps-loops-and-scopes.md), de forma semelhante ao que executa após ações individuais. Um âmbito funciona um agrupamento lógico das ações. 

Âmbitos são úteis para organizar as suas ações de aplicação lógica e efetuar avaliações agregadas o estado de um âmbito. O âmbito próprio recebe um Estado depois de terminar todas as ações de um âmbito. O estado de âmbito é determinado dos mesmos critérios como uma execução. Se a ação final um ramo de execução é **falha** ou **abortado**, o estado é **falha**.

A acionar ações específicas para quaisquer falhas que ocorreu dentro do âmbito, pode utilizar **runAfter** com um âmbito que está marcado como **falha**. Se *qualquer* ações no âmbito falharem, se utilizar **runAfter** para um âmbito, poderá criar uma única ação para detetar falhas.

### <a name="get-the-context-of-failures-with-results"></a>Obter o contexto de falhas com resultados

Embora a deteção de falhas de um âmbito é útil, pode também querer contexto para ajudar a compreender exatamente as ações de falha e quaisquer erros ou códigos de estado que foram devolvidos. O  **@result()** função de fluxo de trabalho fornece o contexto sobre o resultado de todas as ações de um âmbito.

O  **@result()** função utiliza um único parâmetro (nome de âmbito) e devolve uma matriz de todos os ação resultados do dentro desse âmbito. Estes objetos de ação incluem os mesmos atributos como a  **@actions()** produz de objeto, incluindo a hora de início de ação, hora de fim de ação, estado da ação, entradas de ação, correlação da ação IDs e ação. 

Para enviar o contexto de todas as ações que falha dentro de um âmbito, emparelhar facilmente um  **@result()** funcionar com uma **runAfter** propriedade.

Para executar uma ação *para cada* ação num âmbito que tenha um **falha** resultado, e para filtrar a matriz de resultados para as ações que falharam, pode ser emparelhado  **@result()** com um [Filter_array](../connectors/connectors-native-query.md) ação e um [foreach](../logic-apps/logic-apps-loops-and-scopes.md) ciclo. Com a matriz de resultado filtradas, pode executar uma ação para cada falha utilizando o **foreach** ciclo. 

Eis um exemplo que envia um POST de HTTP do pedido com o corpo da resposta de todas as ações que falhou no âmbito My_Scope:

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

Eis as instruções detalhadas para descrever o que acontece no exemplo anterior:

1. Para obter o resultado de todas as ações dentro My_Scope, o **Filter_array** filtros de ação  **@result('My_Scope')**.

2. A condição de **Filter_array** alguma  **@result()** item que tem um Estado igual a **falha**. Esta condição filtra a matriz de todos os resultados de ação de My_Scope, para uma matriz com resultados única ação falhou.

3. Efetuar uma **foreach** ação no *matriz filtrado* produz. Este passo executa uma ação *para cada* falhou o resultado da ação que foi filtrado anteriormente.

    Se a falha de uma única ação no âmbito, as ações no **foreach** execute apenas uma vez. Várias ações falhadas causam uma ação por falha.

4. Enviar um HTTP POST **foreach** do item de corpo de resposta, ou  **@item() ['saídas'] ['corpo']**. O  **@result()** forma item é igual a  **@actions()** forma. Poder ser analisada da mesma forma.

5. Inclua dois cabeçalhos personalizados com o nome da ação falhada  **@item() ['name']** e a executar o cliente ID de controlo  **@item() ['clientTrackingId']**.

Para referência, eis um exemplo de um único  **@result()** item. Mostra o **nome**, **corpo**, e **clientTrackingId** propriedades que são analisadas no exemplo anterior. Fora um **foreach** ação,  **@result()** devolve uma matriz destes objetos.

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

Para diferentes excepção a processar padrões, pode utilizar as expressões descritas anteriormente no artigo. Pode optar por executar uma única excepção a processar a ação fora do âmbito que aceita a matriz filtrada completa de falhas e remova o **foreach**. Também pode incluir outras propriedades útil do  **@result()** resposta, conforme descrito anteriormente.

## <a name="azure-diagnostics-and-telemetry"></a>Telemetria e diagnóstico do Azure

Os padrões descritos neste artigo fornecem formas excelente lidar com erros e exceções no interior de uma execução, mas também pode identificar e respondem a erros independentes a execução de si próprio. [Diagnóstico do Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) fornece uma forma simples para enviar todos os eventos de fluxo de trabalho (incluindo todos os Estados de execução e a ação) para uma conta de armazenamento do Azure ou para um hub de eventos em Event Hubs do Azure. 

Para avaliar os Estados da execução, pode monitorizar os registos e as métricas ou publicá-los a qualquer ferramenta monitorização que preferir. É uma opção de potencial seja transmitido em fluxo através do Event Hubs para todos os eventos [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). No Stream Analytics, pode escrever consultas em direto com base em qualquer anomalias, médias ou falhas dos registos de diagnóstico. Pode utilizar o Stream Analytics para enviar informações para outras origens de dados, como a filas, tópicos, SQL Server, base de dados do Azure Cosmos ou Power BI.

## <a name="next-steps"></a>Passos Seguintes

* Consulte a forma como um cliente [baseia-se com as Logic Apps no Azure de processamento de erros](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* Localizar mais [Logic Apps exemplos e cenários](../logic-apps/logic-apps-examples-and-scenarios.md).
* Saiba como criar [automatizada implementações para as logic apps](../logic-apps/logic-apps-create-deploy-template.md).
* Saiba como [criar e implementar aplicações lógicas com o Visual Studio](logic-apps-deploy-from-vs.md).

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
