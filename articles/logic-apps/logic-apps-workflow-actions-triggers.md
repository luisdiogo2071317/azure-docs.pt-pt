---
title: Acionadores de fluxo de trabalho e ações - Azure Logic Apps | Microsoft Docs
description: Saiba mais sobre os acionadores e ações nas definições de fluxo de trabalho para o Azure Logic Apps
services: logic-apps
author: kevinlam1
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 5/8/2018
ms.author: klam; LADocs
ms.openlocfilehash: 88ee3d810a80bed418e8dbafa4f3e35ccf5e85b1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="triggers-and-actions-for-workflow-definitions-in-azure-logic-apps"></a>Acionadores e ações de definições de fluxo de trabalho no Azure Logic Apps

No [Azure Logic Apps](../logic-apps/logic-apps-overview.md), todos os fluxos de trabalho de aplicação de lógica de começar com acionadores seguidos de ações. Este artigo descreve os acionadores e ações que pode utilizar para criar logic apps para automatizar fluxos de trabalho de negócio ou processos nas suas soluções de integração. Pode criar as logic apps visualmente com o Designer de aplicações lógicas ou por criar diretamente as definições de fluxo de trabalho subjacente com o [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md). Pode utilizar o portal do Azure ou o Visual Studio. Saiba como [preços funciona para acionadores e ações](../logic-apps/logic-apps-pricing.md).

<a name="triggers-overview"></a>

## <a name="triggers-overview"></a>Descrição geral de acionadores

Iniciar todas as aplicações lógicas com um acionador, que define as chamadas que podem instanciar e iniciar um fluxo de trabalho de aplicação lógica. Eis os tipos de acionadores que pode utilizar:

* A *consulta* acionador, que verifica o ponto final de HTTP do serviço em intervalos regulares
* A *push* acionar, que invoca o [API de REST do serviço de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)
 
Todos os acionadores de ter estes elementos de nível superior, embora alguns são opcionais:  
  
```json
"<triggerName>": {
   "type": "<triggerType>",
   "inputs": { "<trigger-behavior-settings>" },
   "recurrence": { 
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "conditions": [ <array-with-required-conditions> ],
   "splitOn": "<property-used-for-creating-runs>",
   "operationOptions": "<optional-trigger-operations>"
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*triggerName*> | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O acionador escreva, por exemplo: "Http" ou "ApiConnection" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| recurrence | Objeto JSON | A frequência e o intervalo que descreve com que frequência o acionador é acionado |  
| frequência | Cadeia | A unidade de tempo que descreve com que frequência o acionador desencadeado: "Segundo", "minutos", "Horas", "Dia", "Semanas" ou "Mês" | 
| intervalo | Número inteiro | Um número inteiro que descreve com que frequência o acionador é acionado com base na frequência de. <p>Seguem-se os intervalos de mínimos e máximo: <p>-Mês: 1-16 meses </br>-Dia: 1-500 dias </br>-Hora: 1-12 000 horas </br>-Minuto: 1-72,000 minutos </br>-Segundo: segundos de 1-9,999,999<p>Por exemplo, se o intervalo é de 6 e a frequência é de "mês", em seguida, a periodicidade é a cada 6 meses. | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [Condições](#trigger-conditions) | Array | Uma ou mais condições que determinam se deve ou não executar o fluxo de trabalho | 
| [splitOn](#split-on-debatch) | Cadeia | Uma expressão que divide cópias de segurança, ou *debatches*, itens de matriz em várias instâncias de fluxo de trabalho para processamento. Esta opção está disponível para acionadores devolvem uma matriz e apenas quando trabalhar diretamente na vista de código. | 
| [operationOptions](#trigger-operation-options) | Cadeia | Alguns acionadores fornecem opções adicionais que permitem-lhe alterar o comportamento predefinido do acionador | 
||||| 

## <a name="trigger-types-and-details"></a>Tipos de Acionador e detalhes  

Cada tipo de accionador tem uma interface diferente e entradas que definem o comportamento do acionador. 

| Tipo de Acionador | Descrição | 
| ------------ | ----------- | 
| [**Periodicidade**](#recurrence-trigger) | É desencadeado com base numa agenda definida. Pode definir uma data futura e hora para que despoletou este acionador. Com base na frequência de, também pode especificar vezes e dias para executar o fluxo de trabalho. | 
| [**Pedido**](#request-trigger)  | Faz com que a sua aplicação lógica para um ponto final possível chamar EndRead, também conhecido como um acionador "manual". Por exemplo, consulte [chamar, acionador, ou aninhar fluxos de trabalho com pontos finais de HTTP](../logic-apps/logic-apps-http-endpoint.md). | 
| [**HTTP**](#http-trigger) | Verifica, ou *inquéritos*, um ponto final de web HTTP. Ponto final de HTTP deve estar em conformidade com um contrato de Acionador específico utilizando o padrão assíncrono "202" ou ao devolver uma matriz. | 
| [**ApiConnection**](#apiconnection-trigger) | Funciona como o acionador HTTP, mas utiliza [gerida pela Microsoft APIs](../connectors/apis-list.md). | 
| [**HTTPWebhook**](#httpwebhook-trigger) | Funciona como o acionador pedido, mas chama um URL especificado para registar e anular o registo. |
| [**ApiConnectionWebhook**](#apiconnectionwebhook-trigger) | Funciona como o acionador HTTPWebhook, mas utiliza [gerida pela Microsoft APIs](../connectors/apis-list.md). | 
||| 

<a name="recurrence-trigger"></a>

## <a name="recurrence-trigger"></a>Acionador de recorrência  

Este acionador seja executada com base no seu periodicidade especificados e a agenda e fornece uma forma fácil para regularmente executar um fluxo de trabalho. 

Segue-se a definição do acionador:

```json
"Recurrence": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month",
      "interval": <recurrence-interval-based-on-frequency>,
      "startTime": "<start-date-time-with-format-YYYY-MM-DDThh:mm:ss>",
      "timeZone": "<time-zone>",
      "schedule": {
         // Applies only when frequency is Day or Week. Separate values with commas.
         "hours": [ <one-or-more-hour-marks> ], 
         // Applies only when frequency is Day or Week. Separate values with commas.
         "minutes": [ <one-or-more-minute-marks> ], 
         // Applies only when frequency is Week. Separate values with commas.
         "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
      }
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```
*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| Recorrência | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "Recurrence" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| recurrence | Objeto JSON | A frequência e o intervalo que descreve com que frequência o acionador é acionado |  
| frequência | Cadeia | A unidade de tempo que descreve com que frequência o acionador desencadeado: "Segundo", "minutos", "Horas", "Dia", "Semanas" ou "Mês" | 
| intervalo | Número inteiro | Um número inteiro que descreve com que frequência o acionador é acionado com base na frequência de. <p>Seguem-se os intervalos de mínimos e máximo: <p>-Mês: 1-16 meses </br>-Dia: 1-500 dias </br>-Hora: 1-12 000 horas </br>-Minuto: 1-72,000 minutos </br>-Segundo: segundos de 1-9,999,999<p>Por exemplo, se o intervalo é de 6 e a frequência é de "mês", em seguida, a periodicidade é a cada 6 meses. | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| startTime | Cadeia | A data de início e a hora neste formato: <p>AAAA-MM-ddTHH se especificar um fuso horário <p>-ou- <p>AAAA-MM-Aaaathh se não especificar um fuso horário <p>Por isso, por exemplo, se quiser 18 de Setembro de 2017 às 2:00 PM, em seguida, especifique "2017-09-18T14:00:00" e especificar um fuso horário, como "Hora padrão do Pacífico", ou "2017-09-18T14:00:00Z" sem um fuso horário. <p>**Nota:** esta hora de início tem de seguir a [especificação de tempo de data ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) no [formato de hora UTC data](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), mas sem um [Desvio UTC](https://en.wikipedia.org/wiki/UTC_offset). Se não especificar um fuso horário, tem de adicionar a letra "Z" no final sem quaisquer espaços. Este "Z" refere-se para o equivalente [tempo nautical](https://en.wikipedia.org/wiki/Nautical_time). <p>Para agendas simples, a hora de início é a primeira ocorrência, enquanto para agendas complexas, o acionador não acionados qualquer define à hora de início. Para obter mais informações sobre datas de início e horas, consulte [criar e a agenda de tarefas em execução regularmente](../connectors/connectors-native-recurrence.md). | 
| timeZone | Cadeia | Aplica-se apenas quando especificar uma hora de início porque este acionador não aceita [Desvio UTC](https://en.wikipedia.org/wiki/UTC_offset). Especifique o fuso horário que pretende aplicar. | 
| hours | Um número inteiro ou matriz de número inteiro | Se especificar "Dia" ou "Semana" para `frequency`, pode especificar um ou mais números inteiros entre 0 e 23, separados por vírgulas, como as horas do dia em que pretende executar o fluxo de trabalho. <p>Por exemplo, se especificar "10", "12" e "14", receberá 10 AM, 12 PM e 2 PM como as marcas de hora. | 
| minutes | Um número inteiro ou matriz de número inteiro | Se especificar "Dia" ou "Semana" para `frequency`, pode especificar um ou mais números inteiros entre 0 e 59, separados por vírgulas, como os minutos da hora em que pretende executar o fluxo de trabalho. <p>Por exemplo, pode especificar "30" como a marca de minuto e utilizando o exemplo anterior de horas do dia, receber 10:30 AM, 12:30 PM e 2:30 PM. | 
| weekDays | Cadeia ou matriz de cadeia | Se especificar "Semana" para `frequency`, pode especificar um ou mais dias, separados por vírgulas, quando pretender executar o fluxo de trabalho: "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sábado" e "Domingo" | 
| Simultaneidade | Objeto JSON | Para acionadores periódicas e consultar, este objeto Especifica o número máximo de instâncias de fluxo de trabalho que podem ser executadas ao mesmo tempo. Utilize este valor para limitar os pedidos a receber de sistemas de back-end. <p>Por exemplo, este valor define o limite de concorrência para 10 instâncias: `"concurrency": { "runs": 10 }` | 
| operationOptions | Cadeia | O `singleInstance` opção especifica que o acionador desencadeado apenas depois de terminar todas as execuções de Active Directory. Consulte [Acionadores: acionados apenas depois de Active Directory é executada concluir](#single-instance). | 
|||| 

*Exemplo 1*

Este acionador de recorrência básico é executada diariamente:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

*Exemplo 2*

Pode especificar uma data de início e hora para que despoletou o acionador. Este acionador de recorrência inicia a data especificada e é acionado diariamente:

```json
"recurrenceTriggerName": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Day",
      "interval": 1,
      "startTime": "2017-09-18T00:00:00Z"
   }
}
```

*Exemplo 3*

Este acionador de recorrência inicia no 9 de Setembro de 2017 à 2:00 PM e é acionado semanalmente cada segunda-feira às 10:30 AM, 12:30 PM e 2:30 da Tarde hora padrão do Pacífico:

``` json
"myRecurrenceTrigger": {
   "type": "Recurrence",
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "hours": [ 10, 12, 14 ],
         "minutes": [ 30 ],
         "weekDays": [ "Monday" ]
      },
      "startTime": "2017-09-07T14:00:00",
      "timeZone": "Pacific Standard Time"
   }
}
```

Para obter mais informações e exemplos para este acionador, consulte [criar e a agenda de tarefas em execução regularmente](../connectors/connectors-native-recurrence.md).

<a name="request-trigger"></a>

## <a name="request-trigger"></a>Pedido de Acionador

Este acionador faz com que a aplicação lógica possível chamar EndRead através da criação de um ponto final que pode aceitar pedidos HTTP recebidos. Para chamar este acionador, tem de utilizar o `listCallbackUrl` API no [API de REST do serviço de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows). Para saber como utilizar este acionador como um ponto final de HTTP, consulte [chamar, acionador, ou aninhar fluxos de trabalho com pontos finais de HTTP](../logic-apps/logic-apps-http-endpoint.md).

```json
"manual": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "GET | POST | PUT | PATCH | DELETE | HEAD",
      "relativePath": "<relative-path-for-accepted-parameter>",
      "schema": {
         "type": "object",
         "properties": { 
            "<propertyName>": {
               "type": "<property-type>"
            }
         },
         "required": [ "<required-properties>" ]
      }
   }
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| Manual | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "Pedir" | 
| tipo | Cadeia | O tipo de pedido, o que é "Http" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| método | Cadeia | O método que pedidos têm de utilizar para chamar o acionador: "GET", "Colocar", "Publicar", "Corrigir", "Eliminar" ou "HEAD" |
| RelativePath | Cadeia | O caminho relativo para o parâmetro que aceita URL do ponto final de HTTP | 
| Esquema | Objeto JSON | O esquema JSON que descreve e valida o payload ou entradas, que o acionador recebe o pedido de entrada. Este esquema ajuda ações de fluxo de trabalho subsequentes saber as propriedades para efeitos de referência. | 
| propriedades | Objeto JSON | Uma ou mais propriedades no esquema JSON que descreve o payload | 
| Necessário | Array | Uma ou mais propriedades que necessitam de valores | 
|||| 

*Exemplo*

Este acionador pedido Especifica que um pedido recebido utilizar o método de HTTP POST para chamar o acionador e um esquema que valida a entrada no pedido de entrada: 

```json
"myRequestTrigger": {
   "type": "Request",
   "kind": "Http",
   "inputs": {
      "method": "POST",
      "schema": {
         "type": "Object",
         "properties": {
            "customerName": {
               "type": "String"
            },
            "customerAddress": { 
               "type": "Object",
               "properties": {
                  "streetAddress": {
                     "type": "String"
                  },
                  "city": {
                     "type": "String"
                  }
               }
            }
         }
      }
   }
} 
```

<a name="http-trigger"></a>

## <a name="http-trigger"></a>Acionador HTTP  

Este acionador consulta um ponto final especificado e verifica a resposta. A resposta determina se o fluxo de trabalho deve ser executado ou não. O `inputs` objeto JSON inclui e requer o `method` e `uri` parâmetros necessários para construir a chamada HTTP:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "uri": "<HTTP-or-HTTPS-endpoint-to-poll>",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": <recurrence-interval-based-on-frequency>
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| HTTP | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "Http" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| método | Sim | Cadeia | O método HTTP para a consulta o ponto final especificado: "GET", "Colocar", "Publicar", "Corrigir", "Eliminar" ou "HEAD" | 
| uri | Sim| Cadeia | O URL de ponto final HTTP ou HTTPS que o acionador verifica ou consulta <p>Tamanho máximo de cadeia: 2 KB | 
| recurrence | Objeto JSON | A frequência e o intervalo que descreve com que frequência o acionador é acionado |  
| frequência | Cadeia | A unidade de tempo que descreve com que frequência o acionador desencadeado: "Segundo", "minutos", "Horas", "Dia", "Semanas" ou "Mês" | 
| intervalo | Número inteiro | Um número inteiro que descreve com que frequência o acionador é acionado com base na frequência de. <p>Seguem-se os intervalos de mínimos e máximo: <p>-Mês: 1-16 meses </br>-Dia: 1-500 dias </br>-Hora: 1-12 000 horas </br>-Minuto: 1-72,000 minutos </br>-Segundo: segundos de 1-9,999,999<p>Por exemplo, se o intervalo é de 6 e a frequência é de "mês", em seguida, a periodicidade é a cada 6 meses. | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| consultas de  | Objeto JSON | Quaisquer parâmetros de consulta que pretende incluir o URL <p>Por exemplo, este elemento adiciona o `?api-version=2015-02-01` cadeia para o URL de consulta: <p>`"queries": { "api-version": "2015-02-01" }` <p>Resultado: `https://contoso.com?api-version=2015-02-01` | 
| cabeçalhos | Objeto JSON | Um ou mais cabeçalhos a enviar no pedido <p>Por exemplo, para definir o idioma e o tipo de um pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Objeto JSON | O payload (dados) para enviar para o ponto final | 
| autenticação | Objeto JSON | O método que o pedido de entrada deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). Para além do programador, a `authority` propriedade é suportada. Quando não especificado, o valor predefinido é `https://login.windows.net`, mas pode utilizar um valor diferente, tal como`https://login.windows\-ppe.net`. | 
| retryPolicy | Objeto JSON | Este objeto customizes o comportamento de repetição de erros intermitentes que tenham 4xx ou 5xx códigos de estado. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| Simultaneidade | Objeto JSON | Para acionadores periódicas e consultar, este objeto Especifica o número máximo de instâncias de fluxo de trabalho que podem ser executadas ao mesmo tempo. Utilize este valor para limitar os pedidos a receber de sistemas de back-end. <p>Por exemplo, este valor define o limite de concorrência para 10 instâncias: <p>`"concurrency": { "runs": 10 }` | 
| operationOptions | Cadeia | O `singleInstance` opção especifica que o acionador desencadeado apenas depois de terminar todas as execuções de Active Directory. Consulte [Acionadores: acionados apenas depois de Active Directory é executada concluir](#single-instance). | 
|||| 

Para funcionar bem com a sua aplicação lógica, o acionador HTTP requer que a API de HTTP está em conformidade com um padrão específico. O acionador HTTP reconhece estas propriedades:  
  
| Resposta | Necessário | Descrição | 
| -------- | -------- | ----------- |  
| Código de estado | Sim | O "200 OK" código de estado inicia uma execução. Qualquer código de estado não iniciar uma execução. | 
| Depois de repetir cabeçalho | Não | O número de segundos até que a aplicação lógica consulta o ponto final novamente | 
| Cabeçalho de localização | Não | O URL para chamar o intervalo de consulta seguinte. Se não for especificado, o URL original é utilizado. | 
|||| 

*Comportamentos de exemplo para pedidos de diferentes*

| Código de estado | Repetir após | Comportamento | 
| ----------- | ----------- | -------- | 
| 200 | {none} | Executar o fluxo de trabalho e, em seguida, novamente procurar mais dados após a periodicidade definida. | 
| 200 | 10 segundos | Executar o fluxo de trabalho e, em seguida, novamente procurar mais dados após 10 segundos. |  
| 202 | 60 segundos | Não aciona o fluxo de trabalho. A tentativa seguinte ocorre um minuto, sujeita a periodicidade definido. Se a periodicidade definida é inferior a um minuto, o cabeçalho depois de repetir tem precedência. Caso contrário, é utilizada a periodicidade definida. | 
| 400 | {none} | Pedir incorreta, não execute o fluxo de trabalho. Se não `retryPolicy` estiver definida, a política predefinida será utilizado. Depois de foi atingido o número de tentativas, o acionador verifica novamente dados após a periodicidade definida. | 
| 500 | {none}| Erro de servidor, não execute o fluxo de trabalho. Se não `retryPolicy` estiver definida, a política predefinida será utilizado. Depois de foi atingido o número de tentativas, o acionador verifica novamente dados após a periodicidade definida. | 
|||| 

### <a name="http-trigger-outputs"></a>Saídas de Acionador HTTP

| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto JSON | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto JSON | O corpo da resposta HTTP | 
|||| 

<a name="apiconnection-trigger"></a>

## <a name="apiconnection-trigger"></a>APIConnection acionador  

Este acionador funciona como o [acionador HTTP](#http-trigger), mas utiliza [APIs gerida pela Microsoft](../connectors/apis-list.md) para os parâmetros para este acionador diferem. 

Segue-se a definição do acionador, apesar de muitas secções são opcionais, pelo que o comportamento do accionador depende se pretende ou não estão incluídas secções:

```json
"<APIConnectionTriggerName>": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "<managed-API-endpoint-URL>"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         },
      },
      "method": "GET | PUT | POST | PATCH | DELETE | HEAD",
      "queries": "<query-parameters>",
      "headers": { "<headers-for-request>" },
      "body": { "<payload-to-send>" },
      "authentication": { "<authentication-method>" },
      "retryPolicy": {
          "type": "<retry-policy-type>",
          "interval": "<retry-interval>",
          "count": <number-retry-attempts>
      }
   },
   "recurrence": {
      "frequency": "Second | Minute | Hour | Day | Week | Month | Year",
      "interval": "<recurrence-interval-based-on-frequency>"
   },
   "runtimeConfiguration": {
      "concurrency": {
         "runs": <maximum-number-for-concurrently-running-workflow-instances>
      }
   },
   "operationOptions": "singleInstance"
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| *APIConnectionTriggerName* | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "ApiConnection" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| anfitrião | Objeto JSON | O objeto JSON que descreve o gateway de anfitrião e o ID para a API gerida <p>O `host` objeto JSON tem estes elementos: `api` e `connection` | 
| api | Objeto JSON | O URL de ponto final para a API gerido: <p>`"runtimeUrl": "<managed-API-endpoint-URL>"` | 
| ligação | Objeto JSON | O nome para a API ligações gerido que utiliza o fluxo de trabalho, que tem de incluir uma referência a um parâmetro com o nome `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>'].name"` | 
| método | Cadeia | O método HTTP para comunicar com a API gerida: "GET", "Colocar", "Publicar", "Corrigir", "Eliminar" ou "HEAD" | 
| recurrence | Objeto JSON | A frequência e o intervalo que descreve com que frequência o acionador é acionado |  
| frequência | Cadeia | A unidade de tempo que descreve com que frequência o acionador desencadeado: "Segundo", "minutos", "Horas", "Dia", "Semanas" ou "Mês" | 
| intervalo | Número inteiro | Um número inteiro que descreve com que frequência o acionador é acionado com base na frequência de. <p>Seguem-se os intervalos de mínimos e máximo: <p>-Mês: 1-16 meses </br>-Dia: 1-500 dias </br>-Hora: 1-12 000 horas </br>-Minuto: 1-72,000 minutos </br>-Segundo: segundos de 1-9,999,999<p>Por exemplo, se o intervalo é de 6 e a frequência é de "mês", em seguida, a periodicidade é a cada 6 meses. | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| consultas de  | Objeto JSON | Quaisquer parâmetros de consulta que pretende incluir o URL <p>Por exemplo, este elemento adiciona o `?api-version=2015-02-01` cadeia para o URL de consulta: <p>`"queries": { "api-version": "2015-02-01" }` <p>Resultado: `https://contoso.com?api-version=2015-02-01` | 
| cabeçalhos | Objeto JSON | Um ou mais cabeçalhos a enviar no pedido <p>Por exemplo, para definir o idioma e o tipo de um pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Objeto JSON | O objeto JSON que descreve o payload (dados) para enviar para a API gerida | 
| autenticação | Objeto JSON | O método que um pedido recebido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | Objeto JSON | Este objeto customizes o comportamento de repetição de erros intermitentes que tenham 4xx ou 5xx códigos de estado: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| Simultaneidade | Objeto JSON | Para acionadores periódicas e consultar, este objeto Especifica o número máximo de instâncias de fluxo de trabalho que podem ser executadas ao mesmo tempo. Utilize este valor para limitar os pedidos a receber de sistemas de back-end. <p>Por exemplo, este valor define o limite de concorrência para 10 instâncias: `"concurrency": { "runs": 10 }` | 
| operationOptions | Cadeia | O `singleInstance` opção especifica que o acionador desencadeado apenas depois de terminar todas as execuções de Active Directory. Consulte [Acionadores: acionados apenas depois de Active Directory é executada concluir](#single-instance). | 
||||

*Exemplo*

```json
"Create_daily_report": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "api": {
            "runtimeUrl": "https://myReportsRepo.example.com/"
         },
         "connection": {
            "name": "@parameters('$connections')['<connection-name>'].name"
         }     
      },
      "method": "POST",
      "body": {
         "category": "statusReports"
      }  
   },
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

### <a name="apiconnection-trigger-outputs"></a>Saídas de Acionador de APIConnection
 
| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto JSON | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto JSON | O corpo da resposta HTTP | 
|||| 

<a name="httpwebhook-trigger"></a>

## <a name="httpwebhook-trigger"></a>HTTPWebhook acionador  

Este acionador funciona como o [acionador pedido](#request-trigger) através da criação de um ponto final possível chamar EndRead para a sua aplicação lógica. No entanto, este acionador também chama um URL de ponto final especificado para registar ou anular o registo uma subscrição. Pode especificar limites no acionador webhook da mesma forma como [limites assíncrona HTTP](#asynchronous-limits). 

Segue-se a definição do acionador, apesar de muitas secções são opcionais e o comportamento do acionador depende as secções que se utilizar ou omita:

```json
"HTTP_Webhook": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "<subscribe-to-endpoint-URL>",
            "headers": { "<headers-for-request>" },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {},
            "retryPolicy": {}
        },
        "unsubscribe": {
            "method": "POST",
            "url": "<unsubscribe-from-endpoint-URL>",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "<subscription-topic>"
            },
            "authentication": {}
        }
    },
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| HTTP_Webhook | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "HttpWebhook" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| subscrever | Objeto JSON| O pedido de envio para chamar e efetuar o registo inicial quando o acionador é criado. Esta chamada acontece para que o acionador pode começar a escutar eventos em ponto final. Para obter mais informações, consulte [subscrever e anular a subscrição](#subscribe-unsubscribe). | 
| método | Cadeia | O método HTTP utilizado para o pedido de subscrição: "GET", "Colocar", "Publicar", "Corrigir", "Eliminar" ou "HEAD" | 
| uri | Cadeia | O URL de ponto final para onde enviar o pedido de subscrição | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| anular a subscrição | Objeto JSON | O envio do pedido para chamar e cancelar a subscrição quando uma operação faz com que o acionador inválido automaticamente. Para obter mais informações, consulte [subscrever e anular a subscrição](#subscribe-unsubscribe). | 
| método | Cadeia | O método HTTP a utilizar para o pedido de cancelamento: "GET", "Colocar", "Publicar", "Corrigir", "Eliminar" ou "HEAD" | 
| uri | Cadeia | O URL de ponto final para onde enviar o pedido de cancelamento | 
| corpo | Objeto JSON | O objeto JSON que descreve o payload (dados) para o pedido de subscrição ou cancelamento | 
| autenticação | Objeto JSON | O método que um pedido recebido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
| retryPolicy | Objeto JSON | Este objeto customizes o comportamento de repetição de erros intermitentes que tenham 4xx ou 5xx códigos de estado: <p>`"retryPolicy": { "type": "<retry-policy-type>", "interval": "<retry-interval>", "count": <number-retry-attempts> }` <p>Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
|||| 

*Exemplo*

```json
"myAppSpotTrigger": {
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
      },
      "unsubscribe": {
         "method": "POST",
         "url": "https://pubsubhubbub.appspot.com/subscribe",
         "body": {
            "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
            "hub.mode": "unsubscribe",
            "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
         },
      }
   },
}
```

<a name="subscribe-unsubscribe"></a>

### <a name="subscribe-and-unsubscribe"></a>`subscribe` e `unsubscribe`

O `subscribe` chamada acontece quando o fluxo de trabalho é alterado de qualquer forma, por exemplo, quando as credenciais são renovadas ou o acionador de entrada a alteração de parâmetros. A chamada utiliza os mesmos parâmetros como ações de HTTP padrão. 
 
O `unsubscribe` chamada ocorre automaticamente quando uma operação faz com que o acionador HTTPWebhook inválido, por exemplo:

* Eliminar ou desativar o acionador. 
* Eliminar ou desativar o fluxo de trabalho. 
* Eliminar ou desativar a subscrição. 

Para suportar estas chamadas a `@listCallbackUrl()` função devolve um exclusivo "URL de chamada de retorno" para este acionador. Este URL representa um identificador exclusivo para os pontos finais que utilizam a API de REST do serviço. Os parâmetros para esta função são os mesmos que o acionador HTTP.

### <a name="httpwebhook-trigger-outputs"></a>Saídas de Acionador de HTTPWebhook

| Nome do elemento | Tipo | Descrição |
| ------------ | ---- | ----------- |
| cabeçalhos | Objeto JSON | Os cabeçalhos da resposta HTTP | 
| corpo | Objeto JSON | O corpo da resposta HTTP | 
|||| 

<a name="apiconnectionwebhook-trigger"></a>

## <a name="apiconnectionwebhook-trigger"></a>ApiConnectionWebhook acionador

Este acionador funciona como o [HTTPWebhook acionador](#httpwebhook-trigger), mas utiliza [gerida pela Microsoft APIs](../connectors/apis-list.md). 

Segue-se a definição do acionador:

```json
"<ApiConnectionWebhookTriggerName>": {
   "type": "ApiConnectionWebhook",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['<connection-name>']['connectionId']"
         }
      },        
      "body": {
          "NotificationUrl": "@{listCallbackUrl()}"
      },
      "queries": "<query-parameters>"
   }
}
```

*Necessário*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*ApiConnectionWebhookTriggerName*> | Objeto JSON | O nome para o acionador, que é um objeto descrito no formato de Javascript Object Notation (JSON)  | 
| tipo | Cadeia | O tipo de Acionador, que é "ApiConnectionWebhook" | 
| Entradas | Objeto JSON | Entradas do accionador que definem o comportamento do acionador | 
| anfitrião | Objeto JSON | O objeto JSON que descreve o gateway de anfitrião e o ID para a API gerida <p>O `host` objeto JSON tem estes elementos: `api` e `connection` | 
| ligação | Objeto JSON | O nome para a API ligações gerido que utiliza o fluxo de trabalho, que tem de incluir uma referência a um parâmetro com o nome `$connection`: <p>`"name": "@parameters('$connections')['<connection-name>']['connectionId']"` | 
| corpo | Objeto JSON | O objeto JSON que descreve o payload (dados) para enviar para a API gerida | 
| NotificationUrl | Cadeia | Devolve um exclusivo "URL de chamada de retorno" para este acionador que pode utilizar a API gerida | 
|||| 

*Opcional*

| Nome do elemento | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| consultas de  | Objeto JSON | Quaisquer parâmetros de consulta que pretende incluir o URL <p>Por exemplo, este elemento adiciona o `?folderPath=Inbox` cadeia para o URL de consulta: <p>`"queries": { "folderPath": "Inbox" }` <p>Resultado: `https://<managed-API-URL>?folderPath=Inbox` | 
|||| 

<a name="trigger-conditions"></a>

## <a name="triggers-conditions"></a>Acionadores: condições

Para qualquer acionador, pode incluir uma matriz com um ou mais condições que determinam se o fluxo de trabalho deve ser executado ou não. Neste exemplo, o acionador de relatório desencadeado apenas durante o fluxo de trabalho `sendReports` parâmetro está definido como true. 

```json
"myDailyReportTrigger": {
   "type": "Recurrence",
   "conditions": [ {
      "expression": "@parameters('sendReports')"
   } ],
   "recurrence": {
      "frequency": "Day",
      "interval": 1
   }
}
```

Além disso, as condições podem referenciar o código de estado o acionador. Por exemplo, suponha que pretende iniciar um fluxo de trabalho apenas quando o seu Web site devolve um código de estado "500":

``` json
"conditions": [ {
   "expression": "@equals(triggers().code, 'InternalServerError')"  
} ]  
```  

> [!NOTE]
> Por predefinição, um acionador desencadeado apenas no receção um "200 OK" resposta. Quando uma expressão referencia o código de estado de um acionador de qualquer forma, o comportamento predefinido do acionador é substituído. Por isso, se pretender que o acionador a acionar para vários códigos de estado, por exemplo, o código de estado 200 e código de estado 201, tem de incluir esta declaração de como a condição: 
>
> `@or(equals(triggers().code, 200),equals(triggers().code, 201))` 

<a name="split-on-debatch"></a>

## <a name="triggers-split-an-array-into-multiple-runs"></a>Acionadores: Dividir uma matriz em várias execuções

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
        "interval": 1
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

<a name="trigger-operation-options"></a>

## <a name="triggers-operation-options"></a>Acionadores: Opções de operação

Estes acionadores fornecem mais opções que permitem-lhe alterar o comportamento predefinido.

| Acionador | Opção de operação | Descrição |
|---------|------------------|-------------|
| [Periodicidade](#recurrence-trigger), <br>[HTTP](#http-trigger), <br>[ApiConnection](#apiconnection-trigger) | singleInstance | Tiver concluído a acionar o acionador apenas após todos os Active Directory é executada. |
||||

<a name="single-instance"></a>

### <a name="triggers-fire-only-after-active-runs-finish"></a>Acionadores: Acionados apenas após a conclusão de execução de Active Directory

Para acionadores onde pode definir a periodicidade, pode especificar que o acionar de Acionador é executado apenas após todas ativas nos tiver concluído a. Se uma periodicidade agendada acontecer enquanto está em execução uma instância de fluxo de trabalho, o acionador ignora e aguarda até à agendada seguinte periodicidade antes de a verificar novamente. Por exemplo:

```json
"myRecurringTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Hour",
        "interval": 1,
    },
    "operationOptions": "singleInstance"
}
```

<a name="actions-overview"></a>

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
| consultas de  | Não | Objeto JSON | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto JSON | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto JSON | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto JSON | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto JSON | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). <p>Para além do programador, é uma propriedade mais suportada: `authority`. Por predefinição, este valor é `https://login.windows.net` quando não especificado, mas pode utilizar um valor diferente, tal como`https://login.windows\-ppe.net`. | 
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
| anfitrião | Sim | Objeto JSON | Representa as informações do conector como a `runtimeUrl` e referência ao objeto de ligação. | 
| método | Sim | Cadeia | Utiliza um destes métodos HTTP: "GET", "Publicar", "Colocar", "Eliminar", "Corrigir" ou "HEAD" | 
| caminho | Sim | Cadeia | O caminho para a operação de API | 
| consultas de  | Não | Objeto JSON | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto JSON | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto JSON | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto JSON | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto JSON | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
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
| anfitrião | Sim | Objeto JSON | Representa as informações do conector como a `runtimeUrl` e referência ao objeto de ligação. | 
| caminho | Sim | Cadeia | O caminho para a operação de API | 
| consultas de  | Não | Objeto JSON | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto JSON | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto JSON | Representa o payload de que é enviado para o ponto final. | 
| retryPolicy | Não | Objeto JSON | Utilize este objeto para personalizar o comportamento de repetição para 4xx ou 5xx erros. Para obter mais informações, consulte [Repita políticas](../logic-apps/logic-apps-exception-handling.md). | 
| operationsOptions | Não | Cadeia | Define o conjunto de comportamentos especiais para substituir. | 
| autenticação | Não | Objeto JSON | Representa o método que o pedido deve utilizar para autenticação. Para obter mais informações, consulte [autenticação de saída do agendador](../scheduler/scheduler-outbound-authentication.md). |
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
| consultas de  | Não | Objeto JSON | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto JSON | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto JSON | Representa o payload de que é enviado para o ponto final. | 
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
| de | Sim | Array | A matriz de origem |
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
| runError | Não | Objeto JSON | Os detalhes do erro. Suportado apenas quando `runStatus` está definido como `Failed`. |
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
| de | Sim | Array | A matriz de origem |
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
| de | Sim | Array | A matriz de origem. Se o `from` valor da propriedade é uma matriz vazia, o resultado é uma tabela vazia. | 
| formato | Sim | Cadeia | O formato de tabela que quiser, "CSV" ou "HTML" | 
| colunas | Não | Array | As colunas da tabela que pretende. Utilize para substituir a forma de tabela de predefinição. | 
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
| até | Não | Objeto JSON | A duração da espera com base num ponto no tempo | 
| até timestamp | Sim | Cadeia | O ponto no tempo no [formato de hora UTC data](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) quando expira a espera | 
| intervalo | Não | Objeto JSON | A duração da espera com base na unidade de intervalo e contagem | 
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
| consultas de  | Não | Objeto JSON | Representa a quaisquer parâmetros de consulta que pretende incluir no URL. <p>Por exemplo, `"queries": { "api-version": "2015-02-01" }` adiciona `?api-version=2015-02-01` para o URL. | 
| cabeçalhos | Não | Objeto JSON | Representa a cada cabeçalho que é enviado no pedido. <p>Por exemplo, para definir o idioma e escreva num pedido: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| corpo | Não | Objeto JSON | Representa o payload de que é enviado para o ponto final. | 
||||| 

Saídas esta ação baseiam-se na definir no `Response` ação para o fluxo de trabalho subordinados. Se o fluxo de trabalho subordinado não define uma `Response` ação, as saídas estão vazios.

## <a name="collection-actions-overview"></a>Descrição geral de ações de coleção

Para ajudar a controlar a execução do fluxo de trabalho, as ações de coleção podem incluir outras ações. Pode consultar diretamente ações numa coleção fora da coleção de referência. Por exemplo, se definir um `Http` ação num âmbito, em seguida, `@body('http')` ainda é válido em qualquer parte do fluxo de trabalho. Além disso, as ações numa coleção podem apenas "executar" outras ações na mesma coleção.

## <a name="if-action"></a>Se ação

Esta ação, o que é uma instrução condicional, permite-lhe avaliar uma condição e executar um ramo com base em se a expressão é avaliada como true. Se a condição for avaliada com êxito como VERDADEIRO, a condição está marcada com estado de "Com êxito". As ações que estão a `actions` ou `else` objetos avaliar para estes valores:

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
| ações | Sim | Objeto JSON | As ações a executar quando internas `expression` avalia para `true` | 
| expressão | Sim | Cadeia | A expressão a avaliar |
| pessoa | Não | Objeto JSON | As ações a executar quando internas `expression` avalia para `false` |
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
      "myCase1": {
         "actions": {
           "myAction1": {}
         },
         "case": "<result1>"
      },
      "myCase2": {
         "actions": {
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
| casos | Sim | Objeto JSON | Contém os conjuntos de internas ações executadas com base no resultado de expression. | 
| Caso | Sim | Cadeia | O valor para corresponder com o resultado | 
| ações | Sim | Objeto JSON | As internas ações executadas para o case correspondente ao resultado expression | 
| predefinido | Não | Objeto JSON | As ações internas que são executados quando não existem casos correspondem o resultado | 
||||| 

Por exemplo:

``` json
"myApprovalEmailAction": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {...}
         },
         "case": "Approve"
      },
      "Case_2": {
         "actions": {
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
| ações | Sim | Objeto JSON | As ações internas executar dentro do ciclo | 
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
| ações | Sim | Objeto JSON | As ações internas executar dentro do ciclo | 
| expressão | Sim | Cadeia | A expressão a avaliar após cada iteração | 
| Limite | Sim | Objeto JSON | Os limites do ciclo. Tem de definir, pelo menos, um limite. | 
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
| ações | Sim | Objeto JSON | As ações internas executar dentro do âmbito |
||||| 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md)
* Saiba mais sobre [API de REST do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)