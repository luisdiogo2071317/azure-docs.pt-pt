---
title: Acionadores de fluxo de trabalho e ações - Azure Logic Apps | Microsoft Docs
description: Saiba mais sobre os acionadores e ações para a criação de fluxos de trabalho automatizados e processos com logic apps
services: logic-apps
author: divyaswarnkar
manager: anneta
editor: ''
documentationcenter: ''
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/13/2017
ms.author: klam; LADocs
ms.openlocfilehash: 28d28888ce66c354da39dc636579655aadbb9e51
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Acionadores e ações para fluxos de trabalho de aplicação de lógica

Iniciar todas as aplicações lógicas com um acionador seguido de ações. Este artigo descreve os tipos de acionadores e ações que pode utilizar para criar integrações de sistema e automatizar fluxos de trabalho do negócio ou processos ao criar as logic apps. 
  
## <a name="triggers-overview"></a>Descrição geral de acionadores 

Iniciar todas as aplicações lógicas com um acionador, que especifica as chamadas que podem iniciar uma aplicação lógica de execução. Eis os tipos de acionadores que pode utilizar:

* A *consulta* acionador, que verifica o ponto final de HTTP do serviço em intervalos regulares
* A *push* acionar, que invoca o [API de REST do serviço de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)
  
Todos os acionadores contenham estes elementos de nível superior:  
  
```json
"<myTriggerName>": {
    "type": "<triggerType>",
    "inputs": { <callSettings> },
    "recurrence": {  
        "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
        "interval": "<recurrence-interval-based-on-frequency>"
    },
    "conditions": [ <array-with-required-conditions> ],
    "splitOn": "<property-used-for-creating-runs>",
    "operationOptions": "<options-for-operations-on-the-trigger>"
}
```

## <a name="trigger-types-and-inputs"></a>Tipos de Acionador e entradas  

Cada tipo de accionador tem uma interface diferente e diferentes *entradas* que define o respetivo comportamento. 

| Tipo de Acionador | Descrição | 
| ------------ | ----------- | 
| **Periodicidade** | É desencadeado com base numa agenda definida. Pode definir uma data futura e hora para que despoletou este acionador. Com base na frequência de, também pode especificar vezes e dias para executar o fluxo de trabalho. | 
| **Pedido**  | Faz com que a sua aplicação lógica para um ponto final que pode chamar, também conhecido como um acionador "manual". | 
| **HTTP** | Verifica, ou *inquéritos*, um ponto final de web HTTP. O ponto final de HTTP deve estar em conformidade com um contrato acionadora específico utilizando o padrão assíncrono "202" ou ao devolver uma matriz. | 
| **ApiConnection** | Consulta como um acionador HTTP, mas utiliza [gerida pela Microsoft APIs](../connectors/apis-list.md). | 
| **HTTPWebhook** | Faz com que a aplicação lógica para um ponto final de possível chamar EndRead, como o **pedido** acionar, mas chama um URL especificado para registar e anular o registo. |
| **ApiConnectionWebhook** | Como funciona o **HTTPWebhook** acionador, mas utiliza APIs gerida pela Microsoft. | 
||| 

Para obter mais informações, consulte [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md). 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Acionador de recorrência  

Este acionador seja executada com base no de periodicidade e a agenda que especificar e fornece uma forma fácil para regularmente executar um fluxo de trabalho. 

Eis um exemplo de Acionador de recorrência básica que é executada diariamente:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Também pode agendar uma data de início e hora para que despoletou o acionador. Por exemplo, para iniciar um relatório semanal cada segunda-feira, pode agendar a aplicação lógica para iniciar uma segunda específica com este exemplo: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime": "2017-09-18T00:00:00Z"
    }
}
```

Segue-se a definição para este acionador:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": <recurrence-interval-based-on-frequency>,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ <one-or-more-hour-marks> ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ <one-or-more-minute-marks> ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
        "timeZone": "<specify-time-zone>"
    }
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| frequência | Sim | Cadeia | A unidade de tempo para a frequência de Acionador é acionado. Utilizar apenas um destes valores: "segundo", "minutos", "horas", "dia", "semanas" ou "mês" | 
| intervalo | Sim | Número inteiro | Um número inteiro que descreve com que frequência o fluxo de trabalho é executada com base na frequência de. <p>Seguem-se os intervalos de mínimos e máximo: <p>-Mês: 1-16 meses </br>-Dia: 1-500 dias </br>-Hora: 1-12 000 horas </br>-Minuto: 1-72,000 minutos </br>-Segundo: segundos de 1-9,999,999<p>Por exemplo, se o intervalo é de 6 e a frequência é de "mês", em seguida, a periodicidade é a cada 6 meses. | 
| timeZone | Não | Cadeia | Aplica-se apenas quando especificar uma hora de início porque este acionador não aceita [Desvio UTC](https://en.wikipedia.org/wiki/UTC_offset). Especifique o fuso horário que pretende aplicar. | 
| startTime | Não | Cadeia | Especifique a data de início e a hora neste formato: <p>AAAA-MM-ddTHH se especificar um fuso horário <p>-ou- <p>AAAA-MM-Aaaathh se não especificar um fuso horário <p>Por isso, por exemplo, se quiser 18 de Setembro de 2017 às 2:00 PM, em seguida, especifique "2017-09-18T14:00:00" e especifique um fuso horário, como "Hora padrão do Pacífico". Em alternativa, especificar "2017-09-18T14:00:00Z" sem um fuso horário. <p>**Nota:** esta hora de início tem de seguir a [especificação de tempo de data ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) no [formato de hora UTC data](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), mas sem um [Desvio UTC](https://en.wikipedia.org/wiki/UTC_offset). Se não especificar um fuso horário, tem de adicionar a letra "Z" no final sem quaisquer espaços. Este "Z" refere-se para o equivalente [tempo nautical](https://en.wikipedia.org/wiki/Nautical_time). <p>Para agendas simples, a hora de início é a primeira ocorrência, enquanto para agendas complexas, o acionador não acionados qualquer define à hora de início. Para obter mais informações sobre datas de início e horas, consulte [criar e a agenda de tarefas em execução regularmente](../connectors/connectors-native-recurrence.md). | 
| weekDays | Não | Cadeia ou matriz de cadeia | Se especificar "Semana" para `frequency`, pode especificar um ou mais dias, separados por vírgulas, quando pretender executar o fluxo de trabalho: "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sábado" e "Domingo" | 
| hours | Não | Um número inteiro ou matriz de número inteiro | Se especificar "Dia" ou "Semana" para `frequency`, pode especificar um ou mais números inteiros entre 0 e 23, separados por vírgulas, como as horas do dia em que pretende executar o fluxo de trabalho. <p>Por exemplo, se especificar "10", "12" e "14", receberá 10 AM, 12 PM e 2 PM como as marcas de hora. | 
| minutes | Não | Um número inteiro ou matriz de número inteiro | Se especificar "Dia" ou "Semana" para `frequency`, pode especificar um ou mais números inteiros entre 0 e 59, separados por vírgulas, como os minutos da hora em que pretende executar o fluxo de trabalho. <p>Por exemplo, pode especificar "30" como a marca de minuto e utilizando o exemplo anterior de horas do dia, receber 10:30 AM, 12:30 PM e 2:30 PM. | 
||||| 

Por exemplo, este acionador de recorrência Especifica que a aplicação lógica é executada semanalmente cada segunda-feira às 10:30 AM, 12:30 PM e 2:30 da Tarde hora padrão do Pacífico, iniciar não define a 9 de Setembro de 2017 às 2:00 PM:

``` json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": 1,
        "schedule": {
            "hours": [
                10,
                12,
                14
            ],
            "minutes": [
                30
            ],
            "weekDays": [
                "Monday"
            ]
        },
       "startTime": "2017-09-07T14:00:00",
       "timeZone": "Pacific Standard Time"
    }
}
```

Para obter mais informações sobre periodicidade e exemplos de hora de início para este acionador, consulte [criar e a agenda de tarefas em execução regularmente](../connectors/connectors-native-recurrence.md).

## <a name="request-trigger"></a>Pedido de Acionador

Este acionador serve como um ponto final que pode utilizar para chamar a sua aplicação lógica através de um pedido de HTTP. Um acionador pedido aspeto neste exemplo:  
  
```json
"myRequestTrigger": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
        "schema": {
            "type": "Object",
            "properties": {
                "myInputProperty1": { "type" : "string" },
                "myInputProperty2": { "type" : "number" }
            },
            "required": [ "myInputProperty1" ]
        }
    }
} 
```

Este acionador tem uma propriedade opcional denominada `schema`:
  
| Nome do elemento | Necessário | Tipo | Descrição |
| ------------ | -------- | ---- | ----------- |
| Esquema | Não | Objeto | Um esquema JSON que valida o pedido de entrada. Útil para ajudar a saber quais as propriedades para fazer referência de passos de fluxo de trabalho subsequentes. | 
||||| 

Para invocar este acionador como um ponto final, tem de chamar o `listCallbackUrl` API. Consulte [API de REST do serviço de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>Acionador HTTP  

Este acionador consulta um ponto final especificado e verifica a resposta para determinar se o fluxo de trabalho deve ser executado ou não. Aqui, o `inputs` objeto aceita estes parâmetros necessários para construir uma chamada HTTP: 

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| método | Sim | Cadeia | Utiliza um destes métodos HTTP: "GET", "Publicar", "Colocar", "Eliminar", "Corrigir" ou "HEAD" | 
| uri | Sim| Cadeia | O HTTP ou HTTPs ponto final que verifica o acionador. Tamanho máximo de cadeia: 2 KB | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| autenticação | Não | Objeto | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). <p>Para além do programador, é uma propriedade mais suportada: `authority`. Por predefinição, este valor é `https://login.windows.net` quando não especificado, mas pode utilizar um valor diferente, tal como`https://login.windows\-ppe.net`. | 
||||| 

A *política de repetição* aplica-se a falhas intermitentes, caracterizadas como códigos de estado HTTP 408, 429 e 5xx, além de quaisquer exceções de conetividade. Pode definir esta política com o `retryPolicy` objeto conforme mostrado aqui:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Para funcionar bem com a sua aplicação lógica, o acionador HTTP requer a API de HTTP em conformidade com um padrão específico. O acionador reconhece estas propriedades:  
  
| Resposta | Necessário | Descrição | 
| -------- | -------- | ----------- |  
| Código de estado | Sim | O código de estado 200 ("OK") faz com que uma execução. Qualquer código de estado não fazer com que uma execução. | 
| Depois de repetir cabeçalho | Não | O número de segundos até que a aplicação lógica consulta o ponto final novamente. | 
| Cabeçalho de localização | Não | O URL para chamar o intervalo de consulta seguinte. Se não for especificado, o URL original é utilizado. | 
|||| 

Seguem-se alguns comportamentos de exemplo para diferentes tipos de pedidos:
  
| Código de resposta | Repetir após | Comportamento | 
| ------------- | ----------- | -------- | 
| 200 | {none} | Executar o fluxo de trabalho e, em seguida, novamente procurar mais dados após a periodicidade definida. | 
| 200 | 10 segundos | Executar o fluxo de trabalho e, em seguida, novamente procurar mais dados após 10 segundos. |  
| 202 | 60 segundos | Não aciona o fluxo de trabalho. A tentativa seguinte ocorre um minuto, sujeita a periodicidade definido. Se a periodicidade definida é inferior a um minuto, o cabeçalho depois de repetir tem precedência. Caso contrário, é utilizada a periodicidade definida. | 
| 400 | {none} | Pedir incorreta, não execute o fluxo de trabalho. Se não `retryPolicy` estiver definida, a política predefinida será utilizado. Depois de foi atingido o número de tentativas, o acionador verifica novamente dados após a periodicidade definida. | 
| 500 | {none}| Erro de servidor, não execute o fluxo de trabalho. Se não `retryPolicy` estiver definida, a política predefinida será utilizado. Depois de foi atingido o número de tentativas, o acionador verifica novamente dados após a periodicidade definida. | 
|||| 

Seguem-se as saídas de Acionador HTTP: 
  
| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto | O corpo da resposta HTTP | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection acionador  

Funcionalidade básica, este acionador funciona como o acionador HTTP. No entanto, os parâmetros para identificar a ação são diferentes. Segue-se um exemplo:   
  
```json
"myDailyReportTrigger": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            }
        },
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "myCategory"
    }
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| anfitrião | Sim | Objeto | O gateway alojada e o ID da aplicação de API | 
| método | Sim | Cadeia | Utiliza um destes métodos HTTP: "GET", "Publicar", "Colocar", "Eliminar", "Corrigir" ou "HEAD" | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| autenticação | Não | Objeto | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

Para o `host` objeto, seguem-se as propriedades:  
  
| Nome do elemento | Necessário | Descrição | 
| ------------ | -------- | ----------- | 
| runtimeUrl de API | Sim | O ponto final para a API gerido | 
| Nome da ligação |  | O nome da ligação de API gerido que o fluxo de trabalho utiliza. Tem de referenciar um parâmetro com o nome `$connection`. |
|||| 

A *política de repetição* aplica-se a falhas intermitentes, caracterizadas como códigos de estado HTTP 408, 429 e 5xx, além de quaisquer exceções de conetividade. Pode definir esta política com o `retryPolicy` objeto conforme mostrado aqui:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Seguem-se as saídas para um acionador de ligação de API:
  
| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto | O corpo da resposta HTTP | 
|||| 

Saiba mais sobre [como preços funciona para ligação da API aciona](../logic-apps/logic-apps-pricing.md#triggers).

## <a name="httpwebhook-trigger"></a>HTTPWebhook acionador  

Este acionador fornece um ponto final, semelhante a `Request` acionador, mas o acionador HTTPWebhook também chama um URL especificado para registar e anular o registo. Eis um exemplo de um acionador HTTPWebhook poderá aspeto:

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": {},
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": {}
        }
    },
    "conditions": []
}
```

Muitas destas secções são opcionais e o comportamento de Acionador HTTPWebhook depende as secções que fornecem ou omita. Seguem-se as propriedades para o acionador HTTPWebhook:
  
| Nome do elemento | Necessário | Descrição | 
| ------------ | -------- | ----------- |  
| subscrever | Não | Especifica o pedido de envio para chamar quando o acionador é criado e efetua o registo inicial. | 
| anular a subscrição | Não | Especifica o pedido de envio para chamar quando o acionador é eliminado. | 
|||| 

Pode especificar limites no acionador webhook da mesma forma como [limites assíncrona HTTP](#asynchronous-limits). Segue-se mais informações sobre o `subscribe` e `unsubscribe` ações:

* `subscribe` chama-se para que o acionador pode começar a escutar eventos. Esta chamada de saída começa com os mesmos parâmetros ações de HTTP padrão. Esta chamada ocorre quando o fluxo de trabalho é alterado de qualquer forma, por exemplo, quando as credenciais são revertidas ou altere os parâmetros de entrada o acionador. 
  
  Para suportar esta chamada, o `@listCallbackUrl()` função devolve um URL exclusivo para este acionador específico no fluxo de trabalho. Este URL representa o identificador único para os pontos finais que utilizam a API de REST do serviço.
  
* `unsubscribe` é chamado automaticamente quando uma operação compõe este acionador inválido, incluindo estas operações:

  * Eliminar ou desativar o acionador. 
  * Eliminar ou desativar o fluxo de trabalho. 
  * Eliminar ou desativar a subscrição. 
  
  Os parâmetros para esta função são os mesmos que o acionador HTTP.

Seguem-se as saídas do HTTPWebhook acionam e o conteúdo do pedido a receber de são:
  
| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto | O corpo da resposta HTTP | 
|||| 

## <a name="triggers-conditions"></a>Acionadores: condições

Para qualquer acionador, pode utilizar uma ou mais condições para determinar se o fluxo de trabalho deve ser executado ou não. Neste exemplo, os acionadores apenas relatório enquanto o fluxo de trabalho `sendReports` parâmetro está definido como true. 

```json
"myDailyReportTrigger": {
    "type": "Recurrence",
    "conditions": [ 
        {
            "expression": "@parameters('sendReports')"
        } 
    ],
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

Por fim, condições podem referenciar o código de estado do acionador. Por exemplo, pode iniciar um fluxo de trabalho apenas quando o seu Web site devolve um código de estado 500:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  

> [!NOTE]
> Por predefinição, um acionador desencadeado apenas no receção um "200 OK" resposta. Quando uma expressão referencia o código de estado de um acionador de qualquer forma, o comportamento predefinido do acionador é substituído. Por isso, se pretender que o acionador a acionar com base em vários códigos de estado, por exemplo, o código de estado 200 e o código de estado 201, tem de incluir esta declaração de como a condição: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-process-an-array-with-multiple-runs"></a>Acionadores: Processar uma matriz com várias execuções

Se o acionador devolve uma matriz para a sua aplicação lógica processar, por vezes, um ciclo "para cada" poderá demorar demasiado tempo a processar a cada item de matriz. Em vez disso, pode utilizar o **SplitOn** propriedade no seu acionador para *debatch* a matriz. 

Debatching divide se os itens de matriz e inicia uma nova instância de aplicação lógica que é executado para cada item de matriz. Esta abordagem é útil, por exemplo, quando pretender consultar um ponto final que poderá devolver vários novos itens entre intervalos de consulta.
Para o número máximo de matriz de itens que **SplitOn** pode processar numa execução de aplicação lógica única, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

> [!NOTE]
> Pode adicionar **SplitOn** apenas para acionadores ao definir ou substituir na vista de código para a definição de JSON da sua aplicação lógica manualmente. Não é possível utilizar **SplitOn** quando pretender implementar um padrão de resposta síncronas. Qualquer fluxo de trabalho que utiliza **SplitOn** e inclui uma resposta de ação é executado no modo assíncrono e envia imediatamente um `202 ACCEPTED` resposta.

Se o ficheiro Swagger o acionador descreve um payload de que é uma matriz, o **SplitOn** propriedade é adicionada automaticamente à sua acionador. Caso contrário, adicione esta propriedade no interior do payload de resposta com a matriz de que pretende debatch. 

Por exemplo, suponha que tem uma API que devolve esta resposta: 
  
```json
{
    "Status": "Succeeded",
    "Rows": [ 
        { 
            "id": 938109380,
            "name": "customer-name-one"
        },
        {
            "id": 938109381,
            "name": "customer-name-two"
        }
    ]
}
```
  
A aplicação lógica necessita apenas de conteúdo a partir de `Rows`, para que possa criar um acionador como neste exemplo.

``` json
"myDebatchTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "Second",
        "interval": "1"
    },
    "inputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.Rows"
}
```

> [!NOTE]
> Se utilizar o `SplitOn` de comandos, não é possível obter as propriedades que estão fora da matriz. Para que, neste exemplo, não é possível obter o `status` propriedade na resposta devolvida a partir da API de.
> 
> Para evitar uma falha se o `Rows` propriedade não existe, este exemplo utiliza o `?` operador.

A definição de fluxo de trabalho pode agora utilizar `@triggerBody().name` obter `customer-name-one` de primeira execução e `customer-name-two` da ser executada segundo. Por isso, o acionador produz aspeto como estes exemplos:

```json
{
    "body": {
        "id": 938109380,
        "name": "customer-name-one"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "customer-name-two"
    }
}
```
  
## <a name="triggers-fire-only-after-all-active-runs-finish"></a>Acionadores: Acionar que executa apenas após todas ativas nos concluir

Pode configurar os acionadores de periodicidade para que estes acionados apenas quando todas as execuções de Active Directory foram concluídas. Para configurar esta definição, defina o `operationOptions` propriedade `singleInstance`:

```json
"myTrigger": {
    "type": "Http",
    "inputs": { },
    "recurrence": { },
    "operationOptions": "singleInstance"
}
```

Se uma periodicidade agendada acontecer enquanto está em execução uma instância de fluxo de trabalho, o acionador ignora e aguarda até que o intervalo de periodicidade agendada seguinte para verificar novamente.

## <a name="actions-overview"></a>Descrição geral de ações

Existem muitos tipos de ações, cada um com o comportamento exclusivo. Cada tipo de ação tem entradas diferentes que definem o comportamento de uma ação. Ações de coleção podem conter muitos outras ações dentro de si próprios. 

### <a name="standard-actions"></a>Ações padrão  

| Tipo de ação | Descrição | 
| ----------- | ----------- | 
| **HTTP** | Chama um ponto final de web HTTP. | 
| **ApiConnection**  | Funciona como a ação de HTTP, mas utiliza [gerida pela Microsoft APIs](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Funciona da seguinte forma HTTPWebhook, mas utiliza APIs do gerida pela Microsoft. | 
| **Resposta** | Define a resposta para uma chamada. | 
| **Compor** | Constrói um objeto arbitrário de entradas a ação. | 
| **Função** | Representa uma função do Azure. | 
| **Aguarde** | Aguarda uma quantidade fixa de tempo ou até uma hora específica. | 
| **fluxo de trabalho** | Representa um fluxo de trabalho aninhado. | 
| **Compor** | Constrói um objeto arbitrário de entradas a ação. | 
| **Consulta** | Filtra uma matriz baseada numa condição. | 
| **Selecionar** | Projetos cada elemento de uma matriz para um novo valor. Por exemplo, pode converter uma matriz dos números existentes numa matriz de objetos. | 
| **Tabela** | Converte uma matriz de itens numa tabela CSV ou HTML. | 
| **Terminar** | Deixa de executar um fluxo de trabalho. | 
| **Aguarde** | Aguarda uma quantidade fixa de tempo ou até uma hora específica. | 
| **fluxo de trabalho** | Representa um fluxo de trabalho aninhado. | 
||| 

### <a name="collection-actions"></a>Ações de coleção

| Tipo de ação | Descrição | 
| ----------- | ----------- | 
| **If** | Avalia uma expressão e com base no resultado, é executado o ramo correspondente. | 
| **comutador** | Execute ações diferentes com base nos valores específicos de um objeto. | 
| **ForEach** | Esta ação ciclo itera através de uma matriz e efetua ações internas em cada item de matriz. | 
| **até** | Esta ação ciclo efetua ações internas até que uma condição resulta como true. | 
| **Âmbito** | Utilize para agrupar logicamente outras ações. | 
|||  

## <a name="http-action"></a>Ação de HTTP  

Uma ação de HTTP chama um ponto final especificado e verifica a resposta para determinar se o fluxo de trabalho deve ser executado ou não. Por exemplo:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest"
    }
}
```

Aqui, o `inputs` objeto aceita estes parâmetros necessários para construir uma chamada HTTP: 

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| método | Sim | Cadeia | Utiliza um destes métodos HTTP: "GET", "Publicar", "Colocar", "Eliminar", "Corrigir" ou "HEAD" | 
| uri | Sim| Cadeia | O HTTP ou HTTPs ponto final que verifica o acionador. Tamanho máximo de cadeia: 2 KB | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). <p>Para além do programador, é uma propriedade mais suportada: `authority`. Por predefinição, este valor é `https://login.windows.net` quando não especificado, mas pode utilizar um valor diferente, tal como`https://login.windows\-ppe.net`. | 
||||| 

Ações de HTTP e as ações de APIConnection suporta *Repita políticas*. Uma política de repetição aplica-se a falhas intermitentes, caracterizadas como códigos de estado HTTP 408, 429 e 5xx, além de quaisquer exceções de conetividade. Pode definir esta política com o `retryPolicy` objeto conforme mostrado aqui:
  
```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

Neste exemplo ação HTTP repete a obter as últimas novidades duas vezes se existirem falhas intermitentes para um total de execuções de três e um atraso de 30 segundo entre cada tentativa de:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy": {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

O intervalo entre tentativas é especificado no [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Este intervalo tem um valor predefinido e o mínimo de 20 segundos, enquanto o valor máximo é uma hora. A contagem de repetições predefinida e o máximo é de quatro horas. Se não especificar uma definição de política de repetição, um `fixed` é utilizada nenhuma estratégia com valores de contagem e o intervalo de repetição do predefinidos. Para desativar a política de repetição, defina o respetivo tipo para `None`.

### <a name="asynchronous-patterns"></a>Padrões assíncronas

Por predefinição, todas as ações baseado em HTTP suportam o padrão de operação assíncrona padrão. Por isso, se o servidor remoto indica que o pedido é aceite para processamento com uma resposta "202 ACEITES", o motor de Logic Apps mantém a consultar o URL especificado no cabeçalho de localização da resposta até atingir um Estado terminal, que é uma resposta não 202.
  
Para desativar o comportamento assíncrono descrito anteriormente, defina `operationOptions` para `DisableAsyncPattern` nas entradas de ação. Neste caso, a saída da ação baseia-se na resposta 202 inicial do servidor. Por exemplo:
  
```json
"invokeLongRunningOperationAction": {
    "type": "Http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

<a name="asynchronous-limits"></a>

#### <a name="asynchronous-limits"></a>Limites assíncronos

Pode limitar a duração de um padrão assíncrono para um intervalo de tempo específico. Se o intervalo de tempo decorrido sem atingir um Estado terminal, o estado a ação está marcado como `Cancelled` com um `ActionTimedOut` código. O limite de tempo limite é especificado no formato ISO 8601. Este exemplo mostra como pode especificar limites:


``` json
"<action-name>": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>Ação de APIConnection

Esta ação faz referência a um conector gerida pela Microsoft, que requerem uma referência a uma ligação válida e informações sobre os parâmetros e API. Eis um exemplo APIConnection ação:

```json
"Send_Email": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "POST",
        "body": {
            "Subject": "New tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| anfitrião | Sim | Objeto | Representa as informações do conector como a `runtimeUrl` e referência ao objeto de ligação. | 
| método | Sim | Cadeia | Utiliza um destes métodos HTTP: "GET", "Publicar", "Colocar", "Eliminar", "Corrigir" ou "HEAD" | 
| caminho | Sim | Cadeia | O caminho para a operação de API | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
||||| 

Uma política de repetição aplica-se a falhas intermitentes, caracterizadas como códigos de estado HTTP 408, 429 e 5xx, além de quaisquer exceções de conetividade. Pode definir esta política com o `retryPolicy` objeto conforme mostrado aqui:

```json
"retryPolicy": {
    "type": "<retry-policy-type>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```

## <a name="apiconnection-webhook-action"></a>Ação de webhook APIConnection

A ação de APIConnectionWebhook referencia um conector gerida pela Microsoft. Esta ação requer uma referência a uma ligação válida e informações sobre os parâmetros e API. Pode especificar limites na ação webhook da mesma forma como [limites assíncrona HTTP](#asynchronous-limits).

```json
"Send_approval_email": {
    "type": "ApiConnectionWebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| anfitrião | Sim | Objeto | Representa as informações do conector como a `runtimeUrl` e referência ao objeto de ligação. | 
| caminho | Sim | Cadeia | O caminho para a operação de API | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Ação de resposta  

Esta ação contém o payload de resposta completo de um pedido de HTTP e inclui uma `statusCode`, `body`, e `headers`:
  
```json
"myResponseAction": {
    "type": "Response",
    "inputs": {
        "statusCode": 200,
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        }
    },
    "runAfter": {}
}
```

A ação de resposta tem restrições especiais que não se aplicam a outras ações, especificamente:  
  
* Não pode ter ações de resposta ramos paralelos dentro de uma definição de aplicação lógica, uma vez que o pedido de entrada requer uma resposta determinística.
  
* Se o fluxo de trabalho atinge uma ação de resposta depois do pedido de entrada já recebeu uma resposta, a ação de resposta é considerada não foi possível ou em conflito. Como resultado, a aplicação de lógica executar está marcado como `Failed`.
  
* Um fluxo de trabalho com ações de resposta não é possível utilizar o `splitOn` comando na definição do acionador porque a chamada cria vários é executado. Como resultado, verifique a existência de neste caso, quando a operação de fluxo de trabalho é COLOCADA e devolver uma resposta "pedido incorreto".

## <a name="compose-action"></a>Componha ação

Esta ação permite-lhe construir um objeto arbitrário e a saída é o resultado da avaliação de entradas a ação. 

> [!NOTE]
> Pode utilizar o `Compose` ação para construir os resultados, incluindo objetos, matrizes e qualquer outro tipo suportado nativamente através das logic apps, como o XML e binary.

Por exemplo, pode utilizar o `Compose` ação intercalação saídas de várias ações:

```json
"composeUserRecordAction": {
    "type": "Compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
    }
}
```

## <a name="function-action"></a>Ação de função

Esta ação permite-lhe representam e chamada um [função do Azure](../azure-functions/functions-overview.md), por exemplo:

```json
"<my-Azure-Function-name>": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Web/sites/<your-Azure-function-app-name>/functions/<your-Azure-function-name>"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()"
        },
        "method": "POST",
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- |  
| id de função | Sim | Cadeia | O ID de recurso para a função do Azure que pretende ligar. | 
| método | Não | Cadeia | O método HTTP utilizado para chamar a função. Se não for especificado, "POST" é o método predefinido. | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
|||||

Quando guardar a sua aplicação lógica, o motor de Logic Apps efetua algumas verificações da função de referenciado:

* Tem de ter acesso à função.
* Pode utilizar apenas um acionador HTTP padrão ou acionador de JSON Webhook genérico.
* A função não deve ter quaisquer rota definida.
* São permitidos apenas "função" e níveis de autorização "anónimo".

> [!NOTE]
> O motor de Logic Apps obtém e coloca em cache o URL de Acionador, que é utilizado no tempo de execução. Para que o se qualquer operação invalida o URL em cache, a ação falhará em runtime. Para contornar este problema, guarde a aplicação lógica, que faz com que a aplicação lógica para obter e colocar em cache o URL de Acionador novamente.

## <a name="select-action"></a>Selecione a ação

Esta ação permite-lhe cada elemento de uma matriz de projeto para um novo valor. Neste exemplo, converte uma matriz de números de uma matriz de objetos:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| de | Sim | Matriz | A matriz de origem |
| seleccionar | Sim | Qualquer | A projecção aplicada a cada elemento na matriz de origem |
||||| 

O resultado do `select` ação é uma matriz que tenha a mesma cardinalidade porque a matriz de entrada. Cada elemento é transformado conforme definido pelo `select` propriedade. Se a entrada é uma matriz vazia, a saída também é uma matriz vazia.

## <a name="terminate-action"></a>Terminar a ação

Esta ação deixa de executar um fluxo de trabalho, cancelar quaisquer ações em curso e a ignorar as restantes ações. A ação de terminação não afeta as ações já foi concluídas.

Por exemplo, para parar uma execução que tenha `Failed` Estado:

```json
"HandleUnexpectedResponse": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response",
        }
    }
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Sim | Cadeia | A execução de destino do Estado, o que é `Failed` ou `Cancelled` |
| runError | Não | Objeto | Os detalhes do erro. Suportado apenas quando `runStatus` está definido como `Failed`. |
| código de runError | Não | Cadeia | Código de erro da execução |
| mensagem de runError | Não | Cadeia | Mensagem de erro da execução | 
||||| 

## <a name="query-action"></a>Ação de consulta

Esta ação permite-lhe uma matriz baseada numa condição de filtro. 

> [!NOTE]
> Não é possível utilizar a ação de Compose para construir os resultados, incluindo objetos, matrizes e qualquer outro tipo suportado nativamente através das logic apps, como o XML e binary.

Por exemplo, para selecionar maiores do que dois números:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| de | Sim | Matriz | A matriz de origem |
| onde | Sim | Cadeia | A condição que é aplicada a cada elemento da matriz de origem. Se não existem valores de satisfazem o `where` condição, o resultado é uma matriz vazia. |
||||| 

O resultado do `query` ação é uma matriz que tenha elementos da matriz de entrada que satisfaçam a condição.

## <a name="table-action"></a>Ação de tabela

Esta ação permite-lhe converter uma matriz numa tabela CSV ou HTML. 

```json
"ConvertToTable": {
    "type": "Table",
    "inputs": {
        "from": "<source-array>",
        "format": "CSV | HTML"
    }
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| de | Sim | Matriz | A matriz de origem. Se o `from` valor da propriedade é uma matriz vazia, o resultado é uma tabela vazia. | 
| formato | Sim | Cadeia | O formato de tabela que quiser, "CSV" ou "HTML" | 
| colunas | Não | Matriz | As colunas da tabela que pretende. Utilize para substituir a forma de tabela de predefinição. | 
| cabeçalho da coluna | Não | Cadeia | O cabeçalho de coluna | 
| valor de coluna | Sim | Cadeia | O valor de coluna | 
||||| 

Suponha que é possível definir uma ação de tabela com este exemplo:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "HTML"
    }
}
```

E utilizar esta matriz de `@triggerBody()`:

```json
[ {"ID": 0, "Name": "apples"},{"ID": 1, "Name": "oranges"} ]
```

Segue-se a saída neste exemplo:

<table><thead><tr><th>ID</th><th>Nome</th></tr></thead><tbody><tr><td>0</td><td>maçãs</td></tr><tr><td>1</td><td>laranjas</td></tr></tbody></table>

Para personalizar esta tabela, pode especificar as colunas explicitamente, por exemplo:

```json
"ConvertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [ 
            {
                "header": "Produce ID",
                "value": "@item().id"
            },
            {
              "header": "Description",
              "value": "@concat('fresh ', item().name)"
            }
        ]
    }
}
```

Segue-se a saída neste exemplo:

<table><thead><tr><th>Produzir ID</th><th>Descrição</th></tr></thead><tbody><tr><td>0</td><td>maçãs raiz</td></tr><tr><td>1</td><td>laranjas raiz</td></tr></tbody></table>

## <a name="wait-action"></a>Aguarde que a ação  

Esta ação suspende a execução do fluxo de trabalho para o intervalo especificado. Este exemplo faz com que o fluxo de trabalho aguarde 15 minutos:
  
```json
"waitForFifteenMinutesAction": {
    "type": "Wait",
    "inputs": {
        "interval": {
            "unit": "minute",
            "count": 15
        }
    }
}
```
  
Em alternativa, aguardar até que um momento específico, pode utilizar este exemplo:
  
```json
"waitUntilOctoberAction": {
    "type": "Wait",
    "inputs": {
        "until": {
            "timestamp": "2017-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> Pode especificar a duração da espera com uma o `interval` objeto ou a `until` objeto, mas não ambos.

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- | 
| até | Não | Objeto | A duração da espera com base num ponto no tempo | 
| até timestamp | Sim | Cadeia | O ponto no tempo no [formato de hora UTC data](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) quando expira a espera | 
| intervalo | Não | Objeto | A duração da espera com base na unidade de intervalo e contagem | 
| unidade de intervalo | Sim | Cadeia | A unidade de tempo. Utilizar apenas um destes valores: "segundo", "minutos", "horas", "dia", "semanas" ou "mês" | 
| Contagem de intervalo | Sim | Número inteiro | Um número inteiro que representa o número de unidades de intervalo utilizado para o período de espera | 
||||| 

## <a name="workflow-action"></a>Ação de fluxo de trabalho

Esta ação permite-lhe aninhar um fluxo de trabalho. O motor de Logic Apps efetua uma verificação de acesso no fluxo de trabalho subordinados, mais especificamente, o acionador, pelo que tem de ter acesso ao fluxo de trabalho subordinados. Por exemplo:

```json
"<my-nested-workflow-action-name>": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/<my-subscription-ID>/resourceGroups/<my-resource-group-name>/providers/Microsoft.Logic/<my-nested-workflow-action-name>",
            "triggerName": "mytrigger001"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Nome do elemento | Necessário | Tipo | Descrição | 
| ------------ | -------- | ---- | ----------- |  
| id de anfitrião | Sim | Cadeia| O ID de recurso para o fluxo de trabalho que pretende chamar | 
| triggerName de anfitrião | Sim | Cadeia | O nome do acionador que pretende invocar | 
| Consultas | Não | Objeto | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto | Representa o payload de que é enviado para o ponto final. | 
||||| 

Saídas esta ação baseiam-se na definir no `Response` ação para o fluxo de trabalho subordinados. Se o fluxo de trabalho subordinado não define uma `Response` ação, as saídas estão vazios.

## <a name="collection-actions-overview"></a>Descrição geral de ações de coleção

Para ajudar a controlar a execução do fluxo de trabalho, as ações de coleção podem incluir outras ações. Pode consultar diretamente ações numa coleção fora da coleção de referência. Por exemplo, se definir um `Http` ação num âmbito, em seguida, `@body('http')` ainda é válido em qualquer parte do fluxo de trabalho. Além disso, as ações numa coleção podem apenas "executar" outras ações na mesma coleção.

## <a name="if-action"></a>Se ação

Esta ação, o que é uma instrução condicional, permite-lhe avaliar uma condição e executar um ramo com base em se a expressão é avaliada como true. Se a condição for avaliada com êxito como VERDADEIRO, a condição está marcada como "Com êxito". As ações que estão a `actions` ou `else` objetos avaliar para estes valores:

* "Foi concluída com êxito" quando executar e concluída com êxito
* "Falha" quando executar e falhar
* "Ignorado" quando o respetivo ramo não executado

Saiba mais sobre [condicionais instruções as logic apps](../logic-apps/logic-apps-control-flow-conditional-statement.md).

``` json
"<my-condition-name>": {
  "type": "If",
  "expression": "<condition>",
  "actions": {
    "if-true-run-this-action": {
      "type": <action-type>,
      "inputs": {},
      "runAfter": {}
    }
  },
  "else": {
    "actions": {
        "if-false-run-this-action": {
            "type": <action-type>,
            "inputs": {},
            "runAfter": {}
        }
    }
  },
  "runAfter": {}
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| ações | Sim | Objeto | As ações a executar quando internas `expression` avalia para `true` | 
| expressão | Sim | Cadeia | A expressão a avaliar |
| pessoa | Não | Objeto | As ações a executar quando internas `expression` avalia para `false` |
||||| 

Por exemplo:

```json
"myCondition": {
    "type": "If",
    "actions": {
        "if-true-check-this-website": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://this-url"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if-false-check-this-other-website": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://this-other-url"
                },
                "runAfter": {}
            }
        }
    }
}
```  

### <a name="how-conditions-can-use-expressions-in-actions"></a>Como as condições podem utilizar expressões nas ações

Seguem-se alguns exemplos que mostram como pode utilizar expressões nas condições:
  
| Expressão de JSON | Resultado | 
| --------------- | ------ | 
| `"expression": "@parameters('hasSpecialAction')"` | Qualquer valor que avalia como verdadeiro faz com que esta condição passem. Suporta apenas expressões. Para converter outros tipos de valor booleano, utilize estas funções: `empty` ou `equals` | 
| `"expression": "@greater(actions('action1').output.value, parameters('threshold'))"` | Suporta as funções de comparação. Neste exemplo, a ação é executado apenas quando o resultado do action1 é superior ao valor de limiar. | 
| `"expression": "@or(greater(actions('action1').output.value, parameters('threshold')), less(actions('action1').output.value, 100))"` | Suporta as funções de lógica para criar expressões aninhadas. Neste exemplo, a ação é executada quando o resultado do action1 é mais do que o limiar ou em 100. | 
| `"expression": "@equals(length(actions('action1').outputs.errors), 0))"` | Para verificar se uma matriz tem quaisquer itens, pode utilizar funções de matriz. Neste exemplo, a ação é executada quando a matriz de erros está vazia. | 
| `"expression": "parameters('hasSpecialAction')"` | Esta expressão causa um erro e não é uma condição válida. Condições tem de utilizar o "@" símbolo. | 
||| 

## <a name="switch-action"></a>Ação de comutador

Esta ação, o que é uma instrução de comutador, efetua ações diferentes com base nos valores específicos de um objeto, expressão ou token. Esta ação avalia o objeto, expressão ou token, escolhe o cenário que corresponde ao resultado e executa as ações de apenas essa maiúsculas e minúsculas. Quando o resultado não corresponde a nenhum maiúsculas e minúsculas, executa a ação predefinida. Quando é executada da instrução switch, apenas um cenário deve corresponder ao resultado. Saiba mais sobre [mudar instruções as logic apps](../logic-apps/logic-apps-control-flow-switch-statement.md).

``` json
"<my-switch-statement-name>": {
   "type": "Switch",
   "expression": "<evaluate-this-object-expression-token>",
   "cases": {
      "myCase1" : {
         "actions" : {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions" : {
           "myAction2": {}
         },
         "case": "<result2>"
      }
   },
   "default": {
      "actions": {
          "myDefaultAction": {}
      }
   },
   "runAfter": {}
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| expressão | Sim | Cadeia | O objeto, expressão ou token avaliar | 
| casos | Sim | Objeto | Contém os conjuntos de internas ações executadas com base no resultado de expression. | 
| Caso | Sim | Cadeia | O valor para corresponder com o resultado | 
| ações | Sim | Objeto | As internas ações executadas para o case correspondente ao resultado expression | 
| predefinido | Não | Objeto | As ações internas que são executados quando não existem casos correspondem o resultado | 
||||| 

Por exemplo:

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions" : {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions" : {
           "Send_an_email_2": {...}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

## <a name="foreach-action"></a>Ação de foreach

Esta ação ciclo itera através de uma matriz e efetua ações internas em cada item de matriz. Por predefinição, o ciclo de Foreach é executado em paralelo. Para o número máximo de paralelo ciclos que "para cada" ciclos pode executar, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). Para cada ciclo são executados sequencialmente, defina o `operationOptions` parâmetro `Sequential`. Saiba mais sobre [Foreach repete nas logic apps](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop).

```json
"<my-forEach-loop-name>": {
    "type": "Foreach",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    },
    "foreach": "<array>",
    "runAfter": {}
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| ações | Sim | Objeto | As ações internas executar dentro do ciclo | 
| foreach | Sim | Cadeia | A matriz de iterar através do | 
| operationOptions | Não | Cadeia | Especifica as opções de operação para personalizar o comportamento. Atualmente, suporta apenas `Sequential` para executar sequencialmente iterações onde o comportamento predefinido é paralelo. |
||||| 

Por exemplo:

```json
"forEach_EmailAction": {
    "type": "Foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "Send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "foreach": "@body('email_filter')",
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a>Até que a ação

Esta ação ciclo executa ações internas até que a condição for avaliada como true. Saiba mais sobre ["até" ciclos em aplicações lógicas](../logic-apps/logic-apps-control-flow-loops.md#until-loop).

```json
 "<my-Until-loop-name>": {
    "type": "Until",
    "actions": {
        "myActionName": {
            "type": "<action-type>",
            "inputs": {},
            "runAfter": {}
        }
    },
    "expression": "<myCondition>",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- | 
| ações | Sim | Objeto | As ações internas executar dentro do ciclo | 
| expressão | Sim | Cadeia | A expressão a avaliar após cada iteração | 
| Limite | Sim | Objeto | Os limites do ciclo. Tem de definir, pelo menos, um limite. | 
| contagem | Não | Número inteiro | O limite no número de iterações para executar | 
| tempo limite | Não | Cadeia | O limite de tempo limite em [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) que especifica quanto o ciclo deve ser executado |
||||| 

Por exemplo:

```json
 "runUntilSucceededAction": {
    "type": "Until",
    "actions": {
        "Http": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 100,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

## <a name="scope-action"></a>Ação de âmbito

Esta ação permite-lhe logicamente ações de grupo num fluxo de trabalho. O âmbito também obtém o estado dela própria após todos os as ações concluir esse âmbito em execução. Saiba mais sobre [âmbitos](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md).

```json
"<my-scope-action-name>": {
    "type": "Scope",
    "actions": {
        "myInnerAction1": {
            "type": "<action-type>",
            "inputs": {}
        },
        "myInnerAction2": {
            "type": "<action-type>",
            "inputs": {}
        }
    }
}
```

| Nome | Necessário | Tipo | Descrição | 
| ---- | -------- | ---- | ----------- |  
| ações | Sim | Objeto | As ações internas executar dentro do âmbito |
||||| 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md)
* Saiba mais sobre [API de REST do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)