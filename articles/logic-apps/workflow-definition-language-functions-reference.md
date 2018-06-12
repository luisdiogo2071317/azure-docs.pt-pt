---
title: Funções de linguagem de definição de fluxo de trabalho - Azure Logic Apps | Microsoft Docs
description: Saiba mais sobre as funções para criar logic apps com a linguagem de definição de fluxo de trabalho
services: logic-apps
author: ecfan
manager: jeconnoc
editor: ''
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 6f1871e1e135ecb9e7cb37c0bedff3737d3febb7
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301393"
---
# <a name="workflow-definition-language-functions-reference-for-azure-logic-apps"></a>Referência das funções de linguagem de definição de fluxo de trabalho para o Azure Logic Apps

Este artigo descreve as funções que pode utilizar durante a criação de fluxos de trabalho com [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Para saber mais acerca das funções nas definições da aplicação lógica, consulte [linguagem de definição de fluxo de trabalho para o Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md#functions). 

> [!NOTE]
> A sintaxe para definições de parâmetros, um ponto de interrogação (?) que aparece depois de um parâmetro significa que o parâmetro é opcional. Por exemplo, consulte [getFutureTime()](#getFutureTime).

<a name="action"></a>

## <a name="action"></a>action

Devolver o *atual* ação de saída do runtime ou valores de outras pares de nome e valor JSON, o que pode atribuir a uma expressão. Por predefinição, esta função referencia o objeto de ação completo, mas pode especificar opcionalmente uma propriedade cujo valor que pretende. Consulte também [actions()](../logic-apps/workflow-definition-language-functions-reference.md#actions).

Pode utilizar o `action()` função apenas nestes locais: 

* O `unsubscribe` propriedade para uma ação do webhook para que possa aceder o resultado do original `subscribe` pedido
* O `trackedProperties` propriedade de uma ação
* O `do-until` cíclicas condição de uma ação

```
action()
action().outputs.body.<property> 
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Propriedade*> | Não | Cadeia | O nome para a propriedade do objecto de acção cujo valor que pretende: **nome**, **startTime**, **endTime**, **entradas**,  **produz**, **estado**, **código**, **trackingId**, e **clientTrackingId**. No portal do Azure, pode localizar estas propriedades, revendo os detalhes de um específico histórico de execução. Para obter mais informações, consulte [API de REST - ações de execução do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*ação-output*> | Cadeia | O resultado da ação atual ou de propriedade | 
|||| 

<a name="actionBody"></a>

## <a name="actionbody"></a>actionBody

Devolver uma ação `body` saída no tempo de execução. A abreviatura para `actions('<actionName>').outputs.body`. Consulte [body()](#body) e [actions()](#actions).

```
actionBody('<actionName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | O nome para a ação `body` resultado que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*saída de corpo de ação*> | Cadeia | O `body` o resultado da ação especificada | 
|||| 

*Exemplo*

Neste exemplo obtém o `body` o resultado da ação Twitter `Get user`: 

```
actionBody('Get_user')
```

E devolve o resultado deste:

```json
"body": {
  "FullName": "Contoso Corporation",
  "Location": "Generic Town, USA",
  "Id": 283541717,
  "UserName": "ContosoInc",
  "FollowersCount": 172,
  "Description": "Leading the way in transforming the digital workplace.",
  "StatusesCount": 93,
  "FriendsCount": 126,
  "FavouritesCount": 46,
  "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="actionOutputs"></a>

## <a name="actionoutputs"></a>actionOutputs

Devolve o resultado de uma ação no tempo de execução. A abreviatura para `actions('<actionName>').outputs`. Consulte [actions()](#actions).

```
actionOutputs('<actionName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | O nome para a ação de saída do que pretende que o | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*Saída*> | Cadeia | O resultado da ação especificada | 
|||| 

*Exemplo*

Neste exemplo obtém o resultado da ação Twitter `Get user`: 

```
actionOutputs('Get_user')
```

E devolve o resultado deste:

```json
{ 
  "statusCode": 200,
  "headers": {
    "Pragma": "no-cache",
    "Vary": "Accept-Encoding",
    "x-ms-request-id": "a916ec8f52211265d98159adde2efe0b",
    "X-Content-Type-Options": "nosniff",
    "Timing-Allow-Origin": "*",
    "Cache-Control": "no-cache",
    "Date": "Mon, 09 Apr 2018 18:47:12 GMT",
    "Set-Cookie": "ARRAffinity=b9400932367ab5e3b6802e3d6158afffb12fcde8666715f5a5fbd4142d0f0b7d;Path=/;HttpOnly;Domain=twitter-wus.azconn-wus.p.azurewebsites.net",
    "X-AspNet-Version": "4.0.30319",
    "X-Powered-By": "ASP.NET",
    "Content-Type": "application/json; charset=utf-8",
    "Expires": "-1",
    "Content-Length": "339"
  },
  "body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
  }
}
```

<a name="actions"></a>

## <a name="actions"></a>ações

Devolva o resultado de uma ação no tempo de execução ou valores a partir de outros pares de nome e valor JSON, o que pode atribuir a uma expressão. Por predefinição, a função referencia o objeto de ação completo, mas pode especificar opcionalmente uma propriedade cujo valor que pretende. Para versões de expressão compacta, consulte [actionBody()](#actionBody), [actionOutputs()](#actionOutputs), e [body()](#body). Para a ação atual, consulte [action()](#action).

> [!NOTE] 
> Anteriormente, pode utilizar o `actions()` função ou o `conditions` elemento ao especificar que uma ação foi executada com base na saída de outra ação. No entanto, para declarar explicitamente dependências entre as ações, terá agora de utilizar a ação dependente `runAfter` propriedade. Para saber mais sobre o `runAfter` propriedade, consulte [detetar e processar falhas com a propriedade runAfter](../logic-apps/logic-apps-workflow-definition-language.md).

```
actions('<actionName>')
actions('<actionName>').outputs.body.<property> 
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | O nome para o objecto de acção cuja saída pretende  | 
| <*Propriedade*> | Não | Cadeia | O nome para a propriedade do objecto de acção cujo valor que pretende: **nome**, **startTime**, **endTime**, **entradas**,  **produz**, **estado**, **código**, **trackingId**, e **clientTrackingId**. No portal do Azure, pode localizar estas propriedades, revendo os detalhes de um específico histórico de execução. Para obter mais informações, consulte [API de REST - ações de execução do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflowrunactions/get). | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*ação-output*> | Cadeia | O resultado da ação especificada ou de propriedade | 
|||| 

*Exemplo*

Neste exemplo obtém o `status` valor da propriedade da ação Twitter `Get user` no tempo de execução: 

```
actions('Get_user').outputs.body.status 
```

E devolve o resultado deste: `"Succeeded"`

<a name="add"></a>

## <a name="add"></a>adicionar

Devolve o resultado da adição de dois números.

```
add(<summand_1>, <summand_2>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*summand_1*>, <*summand_2*> | Sim | Número inteiro, flutuante, ou misto | Os números a adicionar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*soma de resultado*> | Um número inteiro ou número de vírgula flutuante | O resultado da adição de números especificados | 
|||| 

*Exemplo*

Este exemplo adiciona os números especificados:

```
add(1, 1.5)
```

E devolve o resultado deste: `2.5`

<a name="addDays"></a>

## <a name="adddays"></a>addDays

Adicione um número de dias para um carimbo.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*dias*> | Sim | Número inteiro | O número positivo ou negativo de dias a adicionar | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo mais o número especificado de dias  | 
|||| 

*Exemplo 1*

Este exemplo adiciona 10 dias para o carimbo especificado:

```
addDays('2018-03-15T13:00:00Z', 10)
```

E devolve o resultado deste: `"2018-03-25T00:00:0000000Z"`

*Exemplo 2*

Neste exemplo subtrai de cinco dias a contar o carimbo especificado:

```
addDays('2018-03-15T00:00:00Z', -5)
```

E devolve o resultado deste: `"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

## <a name="addhours"></a>addHours

Adicione um número de horas para um carimbo.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*horas*> | Sim | Número inteiro | O número positivo ou negativo horas a adicionar | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo mais o número especificado de horas  | 
|||| 

*Exemplo 1*

Este exemplo adiciona 10 horas para o carimbo especificado:

```
addHours('2018-03-15T00:00:00Z', 10)
```

E devolve o resultado deste: `"2018-03-15T10:00:0000000Z"`

*Exemplo 2*

Neste exemplo subtrai do carimbo especificado de cinco horas:

```
addHours('2018-03-15T15:00:00Z', -5)
```

E devolve o resultado deste: `"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

## <a name="addminutes"></a>addMinutes

Adicione um número de minutos para um carimbo.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*minutos*> | Sim | Número inteiro | O número positivo ou negativo de minutos para adicionar | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo e o número de minutos especificado | 
|||| 

*Exemplo 1*

Este exemplo adiciona 10 minutos para o carimbo especificado:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

E devolve o resultado deste: `"2018-03-15T00:20:00.0000000Z"`

*Exemplo 2*

Neste exemplo subtrai cinco minutos do carimbo especificado:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

E devolve o resultado deste: `"2018-03-15T00:15:00.0000000Z"`

<a name="addProperty"></a>

## <a name="addproperty"></a>addProperty

Adicionar uma propriedade e o valor ou o par nome / valor, para um objeto JSON e retorno do objeto atualizado. Se o objeto já existe no tempo de execução, a função emite um erro.

```
addProperty(<object>, '<property>', <value>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*objeto*> | Sim | Object | O objeto JSON em que pretende adicionar uma propriedade | 
| <*Propriedade*> | Sim | Cadeia | O nome da propriedade adicionar | 
| <*Valor*> | Sim | Qualquer | O valor da propriedade |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*objeto atualizado*> | Object | O objeto JSON atualizado com a propriedade especificada | 
|||| 

*Exemplo*

Este exemplo adiciona o `accountNumber` propriedade para o `customerProfile` objeto, que é convertido em JSON com o [JSON()](#json) função. A função atribui um valor que é gerado pelo [guid()](#guid) funcionar e devolve o objeto atualizado:

```
addProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="addSeconds"></a>

## <a name="addseconds"></a>addSeconds

Adicione um número de segundos para um carimbo.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*Segundos*> | Sim | Número inteiro | O número positivo ou negativo de segundos a adicionar | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo e o número de segundos especificado  | 
|||| 

*Exemplo 1*

Este exemplo adiciona 10 segundos para o carimbo especificado:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

E devolve o resultado deste: `"2018-03-15T00:00:10.0000000Z"`

*Exemplo 2*

Neste exemplo subtrai cinco segundos para o carimbo especificado:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

E devolve o resultado deste: `"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

## <a name="addtotime"></a>addToTime

Adicione um número de unidades de tempo para um carimbo. Consulte também [getFutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*intervalo*> | Sim | Número inteiro | O número de unidades de tempo especificado para adicionar | 
| <*timeUnit*> | Sim | Cadeia | A unidade de tempo para utilizar com *intervalo*: "Segunda", "Minutos", "Horas", "Dia", "Semanas", "Meses", "Ano" | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo e o número de unidades de tempo especificado  | 
|||| 

*Exemplo 1*

Este exemplo adiciona um dia para o carimbo especificado:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day') 
```

E devolve o resultado deste: `"2018-01-02T00:00:00:0000000Z"`

*Exemplo 2*

Este exemplo adiciona um dia para o carimbo especificado:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

E devolve o resultado usando o formato "D" opcional: `"Tuesday, January 2, 2018"`

<a name="and"></a>

## <a name="and"></a>e

Verifique se todas as expressões forem verdadeiras. Devolve verdadeiro quando todas as expressões forem verdadeiras ou devolver false quando, pelo menos, uma expressão é falsa.

```
and(<expression1>, <expression2>, ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Sim | Booleano | As expressões para verificar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando todas as expressões forem verdadeiras. Devolveu false quando, pelo menos, uma expressão é falsa. | 
|||| 

*Exemplo 1*

Estes exemplos verificam se os valores booleanos especificados estão todos os verdadeiras:

```
and(true, true)
and(false, true)
and(false, false)
```

E devolve estes resultados:

* Primeiro exemplo: ambas as expressões forem verdadeiras, por isso, devolve `true`. 
* Segundo exemplo: uma expressão é false, por isso, devolve `false`.
* Exemplo de terceiro: ambas as expressões são falsas, devolve, por isso, `false`.

*Exemplo 2*

Estes exemplos verificam se são verdadeiras todas as expressões especificadas:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

E devolve estes resultados:

* Primeiro exemplo: ambas as expressões forem verdadeiras, por isso, devolve `true`. 
* Segundo exemplo: uma expressão é false, por isso, devolve `false`.
* Exemplo de terceiro: ambas as expressões são falsas, devolve, por isso, `false`.

<a name="array"></a>

## <a name="array"></a>array

Devolva uma matriz de um único especificado de entrada. Várias entradas, consulte [createArray()](#createArray). 

```
array('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia para a criação de uma matriz | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*valor*>] | Array | Uma matriz que contenha a entrada especificada único | 
|||| 

*Exemplo*

Este exemplo cria uma matriz da cadeia "Olá":

```
array('hello')
```

E devolve o resultado deste: `["hello"]`

<a name="base64"></a>

## <a name="base64"></a>base64

Devolva a versão para uma cadeia com codificação base64.

```
base64('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia de entrada | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*cadeia base64*> | Cadeia | A versão com codificação base64 para a cadeia de entrada | 
|||| 

*Exemplo*

Neste exemplo converte a cadeia "Olá" a uma cadeia com codificação base64:

```
base64('hello')
```

E devolve o resultado deste: `"aGVsbG8="`

<a name="base64ToBinary"></a>

## <a name="base64tobinary"></a>base64ToBinary

Devolva a versão binária para uma cadeia com codificação base64.

```
base64ToBinary('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com codificação base64 a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*binário para base64 cadeia*> | Cadeia | A versão binária para a cadeia com codificação base64 | 
|||| 

*Exemplo*

Neste exemplo converte o "aGVsbG8 =" cadeia com codificação base64 para uma cadeia binária:

```
base64ToBinary('aGVsbG8=')
```

E devolve o resultado deste: 

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

## <a name="base64tostring"></a>base64ToString

Devolva a versão de cadeia para uma cadeia com codificação base64, eficazmente descodificar a cadeia base64. Utilize esta função vez [decodeBase64()](#decodeBase64). Embora ambas as funções funcionam da mesma forma, `base64ToString()` preferido.

```
base64ToString('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com codificação base64 para descodificação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*descodificar-base64-cadeia*> | Cadeia | A versão de cadeia para uma cadeia com codificação base64 | 
|||| 

*Exemplo*

Neste exemplo converte o "aGVsbG8 =" cadeia com codificação base64 apenas numa cadeia:

```
base64ToString('aGVsbG8=')
```

E devolve o resultado deste: `"hello"`

<a name="binary"></a>

## <a name="binary"></a>Binário 

Devolva a versão binária para uma cadeia.

```
binary('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*binária de entrada-valor*> | Cadeia | A versão binária para a cadeia especificada | 
|||| 

*Exemplo*

Neste exemplo converte a cadeia "Olá" para uma cadeia binária:

```
binary('hello')
```

E devolve o resultado deste: 

`"0110100001100101011011000110110001101111"`

<a name="body"></a>

## <a name="body"></a>corpo

Devolver uma ação `body` saída no tempo de execução. A abreviatura para `actions('<actionName>').outputs.body`. Consulte [actionBody()](#actionBody) e [actions()](#actions).

```
body('<actionName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | O nome para a ação `body` resultado que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | -----| ----------- | 
| <*saída de corpo de ação*> | Cadeia | O `body` o resultado da ação especificada | 
|||| 

*Exemplo*

Neste exemplo obtém o `body` o resultado do `Get user` Twitter ação: 

```
body('Get_user')
```

E devolve o resultado deste: 

```json
"body": {
    "FullName": "Contoso Corporation",
    "Location": "Generic Town, USA",
    "Id": 283541717,
    "UserName": "ContosoInc",
    "FollowersCount": 172,
    "Description": "Leading the way in transforming the digital workplace.",
    "StatusesCount": 93,
    "FriendsCount": 126,
    "FavouritesCount": 46,
    "ProfileImageUrl": "https://pbs.twimg.com/profile_images/908820389907722240/gG9zaHcd_400x400.jpg"
}
```

<a name="bool"></a>

## <a name="bool"></a>bool

Devolva a versão para um valor booleana.

```
bool(<value>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Qualquer | O valor a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | A versão booleana para o valor especificado | 
|||| 

*Exemplo*

Estes exemplos converter valores especificados para os valores booleanos: 

```
bool(1)
bool(0)
```

E devolve estes resultados: 

* Primeiro exemplo: `true` 
* Segundo exemplo: `false`

<a name="coalesce"></a>

## <a name="coalesce"></a>Unir

Devolva o primeiro valor não nulo a partir de um ou mais parâmetros. Cadeias vazias, as matrizes vazias e objetos em branco não são nulos.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*object_1*>, <*object_2*>,... | Sim | Qualquer, pode misturar tipos | Um ou mais itens para procurar um valor nulo | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*primeiro não-null-item*> | Qualquer | O primeiro item ou valor que não seja nulo. Se todos os parâmetros são nulos, esta função devolve um valor nula. | 
|||| 

*Exemplo*

Estes exemplos devolvem o primeiro valor não nulo do valores especificados ou null quando todos os valores são nulos:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

E devolve estes resultados: 

* Primeiro exemplo: `true` 
* Segundo exemplo: `"hello"`
* Terceiro exemplo: `null`

<a name="concat"></a>

## <a name="concat"></a>concat

Combinar dois ou mais cadeias e a cadeia combinada de retorno. 

```
concat('<text1>', '<text2>', ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Text1*>, <*text2*>,... | Sim | Cadeia | Pelo menos duas cadeias a combinar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*text1text2...*> | Cadeia | A cadeia criada a partir de cadeias de entrada combinadas | 
|||| 

*Exemplo*

Neste exemplo combina as cadeias de "Olá" e "Mundo":

```
concat('Hello', 'World')
```

E devolve o resultado deste: `"HelloWorld"`

<a name="contains"></a>

## <a name="contains"></a>contém

Verifique se uma coleção tem um item específico. Devolve verdadeiro quando o item é encontrado, ou de retorno falso quando não foi encontrado. Esta função é maiúsculas e minúsculas.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Especificamente, esta função funciona nestes tipos de coleção: 

* A *cadeia* para localizar um *subcadeia*
* Um *matriz* para localizar um *valor*
* A *dicionário* para localizar um *chave*

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Dicionário, cadeia ou matriz | A coleção para verificar | 
| <*Valor*> | Sim | Cadeia, matriz ou dicionário, respetivamente | O item a localizar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando o item é encontrado. Devolveu false quando não foi encontrado. |
|||| 

*Exemplo 1*

Este exemplo verifica a cadeia "Olá mundo" para a subcadeia "mundo" e devolve true:

```
contains('hello world', 'world')
```

*Exemplo 2*

Neste exemplo verifica a cadeia "Olá mundo" para a subcadeia "universo" e devolve false:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

## <a name="convertfromutc"></a>convertFromUtc

Converta um carimbo de Universal tempo coordenada (UTC) com o fuso horário de destino.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*destinationTimeZone*> | Sim | Cadeia | O nome da zona de tempo de destino. Para obter mais informações, consulte [IDs de fuso horário](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp convertido*> | Cadeia | Converter o carimbo com o fuso horário de destino | 
|||| 

*Exemplo 1*

Neste exemplo converte um carimbo com o fuso horário especificado: 

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

E devolve o resultado deste: `"2018-01-01T00:00:00.0000000"`

*Exemplo 2*

Neste exemplo converte um carimbo para o fuso horário especificado e o formato:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

E devolve o resultado deste: `"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

## <a name="converttimezone"></a>convertTimeZone

Converter um carimbo de fuso horário a origem com o fuso horário de destino.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*sourceTimeZone*> | Sim | Cadeia | O nome para o fuso horário de origem. Para obter mais informações, consulte [IDs de fuso horário](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*destinationTimeZone*> | Sim | Cadeia | O nome da zona de tempo de destino. Para obter mais informações, consulte [IDs de fuso horário](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp convertido*> | Cadeia | Converter o carimbo com o fuso horário de destino | 
|||| 

*Exemplo 1*

Neste exemplo converte o fuso horário de origem para o fuso horário de destino: 

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

E devolve o resultado deste: `"2018-01-01T00:00:00.0000000"`

*Exemplo 2*

Neste exemplo converte um fuso horário para o fuso horário especificado e o formato:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

E devolve o resultado deste: `"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

## <a name="converttoutc"></a>convertToUtc

Converta um carimbo de origem fuso horário Universal tempo coordenada (UTC).

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*sourceTimeZone*> | Sim | Cadeia | O nome para o fuso horário de origem. Para obter mais informações, consulte [IDs de fuso horário](https://docs.microsoft.com/previous-versions/windows/embedded/gg154758(v=winembedded.80)). | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp convertido*> | Cadeia | O carimbo convertido para UTC | 
|||| 

*Exemplo 1*

Neste exemplo converte um timestamp UTC: 

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

E devolve o resultado deste: `"2018-01-01T08:00:00.0000000Z"`

*Exemplo 2*

Neste exemplo converte um timestamp UTC:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

E devolve o resultado deste: `"Monday, January 1, 2018"`

<a name="createArray"></a>

## <a name="createarray"></a>createArray

Devolva uma matriz de várias entradas. Para as matrizes de entrada único, consulte [array()](#array).

```
createArray('<object1>', '<object2>', ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*objeto1*>, <*object2*>,... | Sim | Qualquer, mas não misto | Pelo menos dois itens para criar a matriz | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*objeto1*>, <*object2*>,...] | Array | A matriz de criada a partir de todos os itens de entrada | 
|||| 

*Exemplo*

Este exemplo cria uma matriz destas entradas:

```
createArray('h', 'e', 'l', 'l', 'o')
```

E devolve o resultado deste: `["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

## <a name="datauri"></a>dataUri

Devolve um identificador de recurso uniforme (URI) de dados para uma cadeia. 

```
dataUri('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*uri de dados*> | Cadeia | O URI de dados para a cadeia de entrada | 
|||| 

*Exemplo*

Este exemplo cria um URI de dados para a cadeia "Olá":

```
dataUri('hello') 
```

E devolve o resultado deste: `"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

## <a name="datauritobinary"></a>dataUriToBinary

Devolva a versão binária para um identificador de recurso uniforme (URI) de dados. Utilize esta função vez [decodeDataUri()](#decodeDataUri). Embora ambas as funções funcionam da mesma forma, `decodeDataUri()` preferido.

```
dataUriToBinary('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | O URI para converter de dados | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*binário de dados-uri*> | Cadeia | A versão binária para o URI de dados | 
|||| 

*Exemplo*

Este exemplo cria uma binária versão destes URI de dados:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

E devolve o resultado deste: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

## <a name="datauritostring"></a>dataUriToString

Devolva a versão de cadeia para um identificador de recurso uniforme (URI) de dados.

```
dataUriToString('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | O URI para converter de dados | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*cadeia de dados-uri*> | Cadeia | A versão de cadeia para o URI de dados | 
|||| 

*Exemplo*

Este exemplo cria uma cadeia para este URI de dados:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

E devolve o resultado deste: `"hello"`

<a name="dayOfMonth"></a>

## <a name="dayofmonth"></a>DayOfMonth

Devolva o dia do mês de um carimbo. 

```
dayOfMonth('<timestamp>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*dia do mês*> | Número inteiro | O dia do mês do carimbo especificado | 
|||| 

*Exemplo*

Neste exemplo devolve o número para o dia do mês deste timestamp:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

E devolve o resultado deste: `15`

<a name="dayOfWeek"></a>

## <a name="dayofweek"></a>dayOfWeek

Devolva o dia da semana de um carimbo.  

```
dayOfWeek('<timestamp>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*dia da semana*> | Número inteiro | O dia da semana do carimbo especificado onde domingo for 0, segunda é 1 e assim sucessivamente | 
|||| 

*Exemplo*

Neste exemplo devolve o número para o dia da semana deste timestamp:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

E devolve o resultado deste: `3`

<a name="dayOfYear"></a>

## <a name="dayofyear"></a>DayOfYear

Devolva o dia do ano de um carimbo. 

```
dayOfYear('<timestamp>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*dia do ano*> | Número inteiro | O dia do ano do carimbo especificado | 
|||| 

*Exemplo*

Neste exemplo devolve o número do dia do ano a partir deste timestamp:

```
dayOfYear('2018-03-15T13:27:36Z')
```

E devolve o resultado deste: `74`

<a name="decodeBase64"></a>

## <a name="decodebase64"></a>decodeBase64

Devolva a versão de cadeia para uma cadeia com codificação base64, eficazmente descodificar a cadeia base64. Considere a utilização de [base64ToString()](#base64ToString) vez `decodeBase64()`. Embora ambas as funções funcionam da mesma forma, `base64ToString()` preferido.

```
decodeBase64('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com codificação base64 para descodificação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*descodificar-base64-cadeia*> | Cadeia | A versão de cadeia para uma cadeia com codificação base64 | 
|||| 

*Exemplo*

Este exemplo cria uma cadeia de uma cadeia com codificação base64:

```
decodeBase64('aGVsbG8=')
```

E devolve o resultado deste: `"hello"`

<a name="decodeDataUri"></a>

## <a name="decodedatauri"></a>decodeDataUri

Devolva a versão binária para um identificador de recurso uniforme (URI) de dados. Considere a utilização de [dataUriToBinary()](#dataUriToBinary), vez `decodeDataUri()`. Embora ambas as funções funcionam da mesma forma, `dataUriToBinary()` preferido.

```
decodeDataUri('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | Os dados de cadeia URI para descodificação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*binário de dados-uri*> | Cadeia | A versão binária para uma cadeia URI de dados | 
|||| 

*Exemplo*

Neste exemplo devolve a versão binária destes URI de dados:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

E devolve o resultado deste: 

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

## <a name="decodeuricomponent"></a>decodeUriComponent

Devolve uma cadeia que substitui o escape carateres com versões descodificadas. 

```
decodeUriComponent('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com os carateres de escape para descodificação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*descodificar-uri*> | Cadeia | A cadeia atualizada com os carateres de escape descodificados | 
|||| 

*Exemplo*

Este exemplo substitui os carateres de escape nesta cadeia com versões descodificadas:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

E devolve o resultado deste: `"https://contoso.com"`

<a name="div"></a>

## <a name="div"></a>div

Devolve o resultado de número inteiro da divisão de dois números. Para obter o resultado do resto, consulte [mod()](#mod).

```
div(<dividend>, <divisor>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*dividendo*> | Sim | Um número inteiro ou número de vírgula flutuante | O número dividir pelo *divisor* | 
| <*Divisor*> | Sim | Um número inteiro ou número de vírgula flutuante | O número que divide o *dividendo*, mas não pode ser 0 | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado quociente*> | Número inteiro | O resultado de número inteiro de dividir o primeiro número, o segundo número | 
|||| 

*Exemplo*

Ambos os exemplos dividem o primeiro número, o segundo número:

```
div(10, 5)
div(11, 5)
```

E devolver este resultado de: `2`

<a name="encodeUriComponent"></a>

## <a name="encodeuricomponent"></a>encodeUriComponent

Devolva uma versão de identificador (URI) codificado de recurso uniforme uma cadeia, substituindo carateres não seguro URL com carateres de escape. Considere a utilização de [uriComponent()](#uriComponent), vez `encodeUriComponent()`. Embora ambas as funções funcionam da mesma forma, `uriComponent()` preferido.

```
encodeUriComponent('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia a converter para formato com codificação URI | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*uri codificado*> | Cadeia | A cadeia URI codificado com carateres de escape | 
|||| 

*Exemplo*

Este exemplo cria uma versão codificado de URI para esta cadeia:

```
encodeUriComponent('https://contoso.com')
```

E devolve o resultado deste: `"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

## <a name="empty"></a>Vazio

Verifique se uma coleção está vazia. Devolve verdadeiro quando a coleção está vazia, ou falso quando não vazio de retorno.

```
empty('<collection>')
empty([<collection>])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Objeto, cadeia ou matriz | A coleção para verificar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando a coleção está vazia. Devolva falso quando não vazio. | 
|||| 

*Exemplo* 

Estes exemplos, verifique se as coleções especificadas estão vazios:

```
empty('')
empty('abc')
```

E devolve estes resultados: 

* Primeiro exemplo: passar uma cadeia vazia, pelo que a função devolve `true`. 
* Segundo exemplo: passa a cadeia "abc", pelo que a função devolve `false`. 

<a name="endswith"></a>

## <a name="endswith"></a>endsWith

Verifique se uma cadeia termina com uma subcadeia específica. Devolve verdadeiro quando a subcadeia for encontrada, ou de retorno falso quando não foi encontrado. Esta função não é sensível.

```
endsWith('<text>', '<searchText>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia de verificação | 
| <*searchText*> | Sim | Cadeia | A final subcadeia a localizar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO  | Booleano | Devolve verdadeiro quando a subcadeia final é encontrada. Devolveu false quando não foi encontrado. | 
|||| 

*Exemplo 1* 

Neste exemplo verifica se a cadeia "Olá mundo" termina com a cadeia de "mundo":

```
endsWith('hello world', 'world')
```

E devolve o resultado deste: `true`

*Exemplo 2*

Neste exemplo verifica se a cadeia "Olá mundo" termina com a cadeia "universo":

```
endsWith('hello world', 'universe')
```

E devolve o resultado deste: `false`

<a name="equals"></a>

## <a name="equals"></a>é igual a

Verifique se os valores, expressões ou objetos são equivalentes. Devolve verdadeiro quando ambas são equivalentes ou devolvem falsas quando não estiverem equivalentes.

```
equals('<object1>', '<object2>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*objeto1*>, <*object2*> | Sim | vários | Os valores, expressões ou objetos a comparar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando ambas são equivalentes. Devolva falso quando não equivalente. | 
|||| 

*Exemplo*

Estes exemplos, verifique se as entradas especificadas são equivalentes. 

```
equals(true, 1)
equals('abc', 'abcd')
```

E devolve estes resultados: 

* Primeiro exemplo: ambos os valores são equivalentes, pelo que a função devolve `true`.
* Segundo exemplo: ambos os valores não são equivalentes, pelo que a função devolve `false`.

<a name="first"></a>

## <a name="first"></a>primeiro

Devolva o primeiro item de uma cadeia ou matriz.

```
first('<collection>')
first([<collection>])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Cadeia ou matriz | A coleção onde encontrar o primeiro item |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*item de coleção primeiro*> | Qualquer | O primeiro item na coleção | 
|||| 

*Exemplo*

Estes exemplos encontrar o primeiro item nestas coleções:

```
first('hello')
first([0, 1, 2])
```

E devolver estes resultados: 

* Primeiro exemplo: `"h"`
* Segundo exemplo: `0`

<a name="float"></a>

## <a name="float"></a>flutuante

Converta uma versão de cadeia para um número de vírgula flutuante de um número real de ponto flutuante. Pode utilizar esta função só ao transmitir parâmetros personalizados para uma aplicação, tais como uma aplicação lógica.

```
float('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia que tem um número de vírgula flutuante válido para converter |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de vírgula flutuante*> | Flutuante | O número de vírgula flutuante para a cadeia especificada | 
|||| 

*Exemplo*

Este exemplo cria uma versão de cadeia para este número de vírgula flutuante:

```
float('10.333')
```

E devolve o resultado deste: `10.333`

<a name="formatDateTime"></a>

## <a name="formatdatetime"></a>formatDateTime

Devolva uma timestamp no formato especificado.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp reformatada*> | Cadeia | O carimbo atualizado no formato especificado | 
|||| 

*Exemplo*

Neste exemplo converte um carimbo para o formato especificado:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

E devolve o resultado deste: `"2018-03-15T12:00:00"`

<a name="formDataMultiValues"></a>

## <a name="formdatamultivalues"></a>formDataMultiValues

Devolver uma matriz com valores que correspondam a um nome de chave uma ação *dados do formulário* ou *codificado de formulário* saída. 

```
formDataMultiValues('<actionName>', '<key>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | A ação cuja saída tem o valor da chave pretende obter | 
| <*chave*> | Sim | Cadeia | O nome da chave cujo valor que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*matriz com chave valores*>] | Array | Uma matriz com todos os valores que correspondem a chave especificada | 
|||| 

*Exemplo* 

Este exemplo cria uma matriz de valor da chave "Requerente" dados do formulário ou saída codificado de forma a ação especificada:  

```
formDataMultiValues('Send_an_email', 'Subject')
```

E devolve o texto do requerente numa matriz, por exemplo: `["Hello world"]`

<a name="formDataValue"></a>

## <a name="formdatavalue"></a>formDataValue

Devolver um valor único que corresponda a um nome de chave uma ação *dados do formulário* ou *codificado de formulário* saída. Se a função localizar mais do que uma correspondência, a função emite um erro.

```
formDataValue('<actionName>', '<key>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | A ação cuja saída tem o valor da chave pretende obter | 
| <*chave*> | Sim | Cadeia | O nome da chave cujo valor que pretende |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*chave-valor*> | Cadeia | O valor da chave especificada  | 
|||| 

*Exemplo* 

Este exemplo cria uma cadeia de valor da chave "Requerente" dados do formulário ou saída codificado de forma a ação especificada:  

```
formDataValue('Send_an_email', 'Subject')
```

E devolve o texto do requerente como uma cadeia, por exemplo: `"Hello world"`

<a name="getFutureTime"></a>

## <a name="getfuturetime"></a>getFutureTime

Devolva o timestamp atual e as unidades de tempo especificado.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*intervalo*> | Sim | Número inteiro | O número de unidades de tempo especificado a subtrair | 
| <*timeUnit*> | Sim | Cadeia | A unidade de tempo para utilizar com *intervalo*: "Segunda", "Minutos", "Horas", "Dia", "Semanas", "Meses", "Ano" | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo atual e o número de unidades de tempo especificado | 
|||| 

*Exemplo 1*

Suponha o timestamp atual é "2018-03-01T00:00:00.0000000Z". Este exemplo adiciona cinco dias para esse timestamp:

```
getFutureTime(5, 'Day')
```

E devolve o resultado deste: `"2018-03-06T00:00:00.0000000Z"`

*Exemplo 2*

Suponha o timestamp atual é "2018-03-01T00:00:00.0000000Z". Este exemplo adiciona cinco dias e converte o resultado para o formato "D":

```
getFutureTime(5, 'Day', 'D')
```

E devolve o resultado deste: `"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

## <a name="getpasttime"></a>getPastTime

Devolva o timestamp atual menos as unidades de tempo especificado.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*intervalo*> | Sim | Número inteiro | O número de unidades de tempo especificado a subtrair | 
| <*timeUnit*> | Sim | Cadeia | A unidade de tempo para utilizar com *intervalo*: "Segunda", "Minutos", "Horas", "Dia", "Semanas", "Meses", "Ano" | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo atual menos o número de unidades de tempo especificado | 
|||| 

*Exemplo 1*

Suponha o timestamp atual é "2018-02-01T00:00:00.0000000Z". Neste exemplo subtrai cinco dias a partir desse timestamp:

```
getPastTime(5, 'Day')
```

E devolve o resultado deste: `"2018-01-27T00:00:00.0000000Z"`

*Exemplo 2*

Suponha o timestamp atual é "2018-02-01T00:00:00.0000000Z". Este exemplo subtrai cinco dias e converte o resultado para o formato "D":

```
getPastTime(5, 'Day', 'D')
```

E devolve o resultado deste: `"Saturday, January 27, 2018"`

<a name="greater"></a>

## <a name="greater"></a>maior

Verifique se o primeiro valor é maior do que o segundo valor. Devolve verdadeiro quando o primeiro valor é mais ou devolver false quando inferior.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Número inteiro, flutuante ou uma cadeia | O primeiro valor para verificar se maior que o segundo valor | 
| <*compareTo*> | Sim | Número inteiro, flutuante ou uma cadeia, respetivamente | O valor de comparação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando o primeiro valor é maior do que o segundo valor. Devolva falso quando o primeiro valor é igual ou menor do que o segundo valor. | 
|||| 

*Exemplo*

Estes exemplos, verifique se o primeiro valor é maior do que o segundo valor:

```
greater(10, 5)
greater('apple', 'banana')
```

E devolver estes resultados: 

* Primeiro exemplo: `true`
* Segundo exemplo: `false`

<a name="greaterOrEquals"></a>

## <a name="greaterorequals"></a>greaterOrEquals

Verifique se o primeiro valor é maior que ou igual ao valor segundo.
Devolve verdadeiro quando o primeiro valor é maior ou igual ou devolver falso quando o primeiro valor for menor.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Número inteiro, flutuante ou uma cadeia | O primeiro valor para verificar se maior que ou igual ao valor segundo | 
| <*compareTo*> | Sim | Número inteiro, flutuante ou uma cadeia, respetivamente | O valor de comparação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando o primeiro valor é maior que ou igual ao valor segundo. Falso quando o primeiro texto seja menor que o segundo valor de retorno. | 
|||| 

*Exemplo*

Estes exemplos, verifique se o primeiro valor é igual ou maior do que o segundo valor:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

E devolver estes resultados: 

* Primeiro exemplo: `true`
* Segundo exemplo: `false`

<a name="guid"></a>

## <a name="guid"></a>GUID

Gerar um identificador exclusivo global (GUID) como uma cadeia, por exemplo, "c2ecc88d-88c8-4096-912c-d6f2e2b138ce": 

```
guid()
```

Além disso, pode especificar um formato diferente para o GUID que não seja o formato predefinido, "D", que é 32 dígitos separados por hífenes.

```
guid('<format>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*formato*> | Não | Cadeia | Um único [especificador de formato](https://msdn.microsoft.com/library/97af8hh4) para o GUID devolvido. Por predefinição, o formato é "D", mas pode utilizar "N", "D", "B", "P" ou "X". | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*Valor GUID*> | Cadeia | Um GUID gerado aleatoriamente | 
|||| 

*Exemplo* 

Este exemplo gera o mesmo GUID, mas como 32 dígitos, separados por hífenes e entre parênteses: 

```
guid('P')
```

E devolve o resultado deste: `"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

## <a name="if"></a>Se

Certifique-se uma expressão true ou false. Com base no resultado, devolva um valor especificado.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*expressão*> | Sim | Booleano | A expressão para verificar | 
| <*valueIfTrue*> | Sim | Qualquer | O valor a devolver quando a expressão for verdadeira | 
| <*valueIfFalse*> | Sim | Qualquer | O valor a devolver quando a expressão é falsa | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*especificado retorno-valor*> | Qualquer | O valor especificado, que devolve com base em se a expressão é true ou false | 
|||| 

*Exemplo* 

Este exemplo devolve `"yes"` porque a expressão especificada devolve true. Caso contrário, devolve o exemplo `"no"`:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

## <a name="indexof"></a>indexOf

Devolve a posição inicial ou o valor de índice de uma subcadeia. Esta função não é sensível e índices de começar com o número 0. 

```
indexOf('<text>', '<searchText>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia que tenha a subcadeia a localizar | 
| <*searchText*> | Sim | Cadeia | A subcadeia a localizar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de índice*>| Número inteiro | O valor de índice ou a posição inicial para a subcadeia especificada. <p>Se a cadeia não for encontrada, devolve o número de -1. </br>Se a cadeia vazia, devolva o número 0. | 
|||| 

*Exemplo* 

Neste exemplo localiza o valor de índice inicial para a subcadeia de "mundo" na cadeia "Olá mundo":

```
indexOf('hello world', 'world')
```

E devolve o resultado deste: `6`

<a name="int"></a>

## <a name="int"></a>Int

Devolva a versão de número inteiro de uma cadeia.

```
int('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado de número inteiro*> | Número inteiro | A versão de número inteiro para a cadeia especificada | 
|||| 

*Exemplo* 

Este exemplo cria uma versão de número inteiro para a cadeia "10":

```
int('10')
```

E devolve o resultado deste: `10`

<a name="item"></a>

## <a name="item"></a>item

Quando utilizada dentro de uma ação repetida através de uma matriz, devolva o item atual na matriz durante iteração atual a ação. Também pode obter os valores de propriedades esse item. 

```
item()
```

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*item de matriz atual*> | Qualquer | O item atual na matriz para iteração atual a ação | 
|||| 

*Exemplo* 

Neste exemplo obtém o `body` elemento da mensagem atual para a ação "Send_an_email" dentro iteração atual de um para cada ciclo:

```
item().body
```

<a name="items"></a>

## <a name="items"></a>itens

Devolva o item atual de cada ciclo de um para cada ciclo. Utilize esta função dentro de cada ciclo.

```
items('<loopName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*loopName*> | Sim | Cadeia | O nome para cada ciclo | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*item*> | Qualquer | O item a partir de ciclo atual na especificado para cada ciclo | 
|||| 

*Exemplo* 

Neste exemplo obtém o item atual de ciclo para cada especificado:

```
items('myForEachLoopName')
```

<a name="json"></a>

## <a name="json"></a>json

Devolva o valor de tipo JavaScript Object Notation (JSON) ou o objeto para uma cadeia ou XML.

```
json('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia ou XML | A cadeia ou XML converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*Resultado de JSON*> | Tipo nativo JSON ou o objeto | O valor de tipo nativo JSON ou o objeto para a cadeia especificada ou XML. Se a cadeia for nula, a função devolve um objeto vazio. | 
|||| 

*Exemplo 1* 

Neste exemplo converte esta cadeia para o valor JSON:

```
json('[1, 2, 3]')
```

E devolve o resultado deste: `[1, 2, 3]`

*Exemplo 2*

Neste exemplo converte esta cadeia JSON: 

```
json('{"fullName": "Sophia Owen"}')
```

E devolve o resultado deste:

```
{
  "fullName": "Sophia Owen"
}
```

*Exemplo 3*

Neste exemplo converte esta XML para JSON: 

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

E devolve o resultado deste:

```json
{ 
   "?xml": { "@version": "1.0" }, 
   "root": { 
      "person": [ { 
         "@id": "1", 
         "name": "Sophia Owen", 
         "occupation": "Engineer" 
       } ] 
   } 
}
```

<a name="intersection"></a>

## <a name="intersection"></a>intersecção

Devolver uma coleção que tenha *apenas* os itens comuns entre coleções especificadas. A aparecer nos resultados, um item tem de aparecer em todas as coleções transmitidas para esta função. Se um ou mais itens têm o mesmo nome, o último item com esse nome é apresentado nos resultados.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>,... | Sim | Matriz ou o objeto, mas não ambos | As coleções de onde pretende *apenas* os itens comuns | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*itens de comuns*> | Matriz nem um objeto, respetivamente | Uma coleção que tenha apenas os itens comuns entre coleções especificadas | 
|||| 

*Exemplo* 

Neste exemplo localiza os itens comuns entre estes matrizes:  

```
intersection([1, 2, 3], [101, 2, 1, 10], [6, 8, 1, 2])
```

E devolve uma matriz com *apenas* estes itens: `[1, 2]`

<a name="join"></a>

## <a name="join"></a>aderir

Devolve uma cadeia que tem todos os itens a partir de uma matriz e tem cada caráter separada por um *delimitador*.

```
join([<collection>], '<delimiter>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Array | A matriz de que tem os itens de associação |  
| <*Delimitador*> | Sim | Cadeia | O separador que aparece entre cada caráter na cadeia resultante | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*char1*><*delimitador*><*char2*><*delimitador*>... | Cadeia | A cadeia resultante criada a partir de todos os itens na matriz especificada |
|||| 

*Exemplo* 

Este exemplo cria uma cadeia de todos os itens desta matriz com o caráter especificado como o delimitador de:

```
join([a, b, c], '.')
```

E devolve o resultado deste: `"a.b.c"`

<a name="last"></a>

## <a name="last"></a>última

Devolva o último item de uma coleção.

```
last('<collection>')
last([<collection>])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Cadeia ou matriz | A coleção onde encontrar o último item | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*item de recolha de último*> | Cadeia ou matriz, respetivamente | O último item na coleção | 
|||| 

*Exemplo* 

Estes exemplos encontrar o último item nestas coleções:

```
last('abcd')
last([0, 1, 2, 3])
```

E devolve estes resultados: 

* Primeiro exemplo: `"d"`
* Segundo exemplo: `3`

<a name="lastindexof"></a>

## <a name="lastindexof"></a>lastIndexOf

Devolva o valor de índice ou a posição final para uma subcadeia. Esta função não é sensível e índices de começar com o número 0.

```
lastIndexOf('<text>', '<searchText>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia que tenha a subcadeia a localizar | 
| <*searchText*> | Sim | Cadeia | A subcadeia a localizar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de índice de fim*> | Número inteiro | O valor de índice ou a posição final para a subcadeia especificada. <p>Se a cadeia não for encontrada, devolve o número de -1. </br>Se a cadeia vazia, devolva o número 0. | 
|||| 

*Exemplo* 

Neste exemplo localiza o valor final do índice para a subcadeia de "mundo" na cadeia "Olá mundo":

```
lastIndexOf('hello world', 'world')
```

E devolve o resultado deste: `10`

<a name="length"></a>

## <a name="length"></a>comprimento

Devolva o número de itens numa coleção.

```
length('<collection>')
length([<collection>])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Cadeia ou matriz | A coleção com os itens a contar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*o comprimento ou contagem*> | Número inteiro | O número de itens na coleção | 
|||| 

*Exemplo*

Estes exemplos contam o número de itens nestas coleções: 

```
length('abcd')
length([0, 1, 2, 3])
```

E devolver este resultado de: `4`

<a name="less"></a>

## <a name="less"></a>menor

Verifique se o primeiro valor é menor que o segundo valor.
Devolve verdadeiro quando o primeiro valor é menor ou devolver falso quando o primeiro valor é mais.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Número inteiro, flutuante ou uma cadeia | O primeiro valor para verificar se menor que o segundo valor | 
| <*compareTo*> | Sim | Número inteiro, flutuante ou uma cadeia, respetivamente | O item de comparação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando o primeiro texto seja menor que o segundo valor. Devolva falso quando o primeiro valor é igual ou maior do que o segundo valor. | 
|||| 

*Exemplo*

Estes exemplos Verifique se o primeiro valor é menor que o segundo valor.

```
less(5, 10)
less('banana', 'apple')
```

E devolver estes resultados: 

* Primeiro exemplo: `true`
* Segundo exemplo: `false`

<a name="lessOrEquals"></a>

## <a name="lessorequals"></a>lessOrEquals

Verifique se o primeiro valor é menor ou igual ao valor segundo.
Devolve verdadeiro quando o primeiro valor é menor ou igual ou devolver falso quando o primeiro valor é mais.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Número inteiro, flutuante ou uma cadeia | O primeiro valor para verificar se menor ou igual ao valor segundo | 
| <*compareTo*> | Sim | Número inteiro, flutuante ou uma cadeia, respetivamente | O item de comparação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO  | Booleano | Devolve verdadeiro quando o primeiro valor é menor ou igual ao valor segundo. Devolveu false quando o primeiro valor é maior que o segundo valor. |  
|||| 

*Exemplo*

Estes exemplos, verifique se o primeiro valor é menor ou igual ao valor segundo.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

E devolver estes resultados: 

* Primeiro exemplo: `true`
* Segundo exemplo: `false`

<a name="listCallbackUrl"></a>

## <a name="listcallbackurl"></a>listCallbackUrl

Devolva o "URL de chamada de retorno" que chama um acionador ou ação. Esta função funciona apenas com acionadores e ações para o **HttpWebhook** e **ApiConnectionWebhook** conector tipos, mas não o **Manual**,  **Periodicidade**, **HTTP**, e **APIConnection** tipos. 

```
listCallbackUrl()
```

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*URL de chamada de retorno*> | Cadeia | O URL de chamada de retorno para um acionador ou ação |  
|||| 

*Exemplo*

Este exemplo mostra um URL de chamada de retorno de exemplo que poderá devolver esta função:

`"https://prod-01.westus.logic.azure.com:443/workflows/<*workflow-ID*>/triggers/manual/run?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<*signature-ID*>"`

<a name="max"></a>

## <a name="max"></a>máx.

Devolva o valor mais alto de uma lista ou matriz com números inclusive em ambas as extremidades. 

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*number1*>, <*Número2*>,... | Sim | Número inteiro, flutuante ou ambos | O conjunto de números a partir do qual pretende que o valor mais alto | 
| [<*number1*>, <*Número2*>,...] | Sim | Matriz - número inteiro, flutuante ou ambos | A matriz de números a partir do qual pretende que o valor mais alto | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor máximo*> | Um número inteiro ou número de vírgula flutuante | O valor mais alto na matriz especificada ou conjunto de números | 
|||| 

*Exemplo* 

Estes exemplos obtém o valor mais alto do conjunto de números e a matriz de:

```
max(1, 2, 3)
max([1, 2, 3])
```

E devolver este resultado de: `3`

<a name="min"></a>

## <a name="min"></a>min.

Devolva o valor mais baixo de um conjunto de números ou uma matriz.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*number1*>, <*Número2*>,... | Sim | Número inteiro, flutuante ou ambos | O conjunto de números a partir do qual pretende que o menor valor | 
| [<*number1*>, <*Número2*>,...] | Sim | Matriz - número inteiro, flutuante ou ambos | A matriz de números a partir do qual pretende que o menor valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor mínimo*> | Um número inteiro ou número de vírgula flutuante | O valor mais baixo do conjunto especificado de números ou a matriz especificada | 
|||| 

*Exemplo* 

Estes exemplos obtém o valor mais baixo no conjunto de números e a matriz de:

```
min(1, 2, 3)
min([1, 2, 3])
```

E devolver este resultado de: `1`

<a name="mod"></a>

## <a name="mod"></a>MOD

Devolve o resto da divisão de dois números. Para obter o resultado de número inteiro, consulte [div()](#div).

```
mod(<dividend>, <divisor>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*dividendo*> | Sim | Um número inteiro ou número de vírgula flutuante | O número dividir pelo *divisor* | 
| <*Divisor*> | Sim | Um número inteiro ou número de vírgula flutuante | O número que divide o *dividendo*, mas não pode ser 0. | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*módulo resultado*> | Um número inteiro ou número de vírgula flutuante | O resto da divisão do primeiro número, o segundo número | 
|||| 

*Exemplo* 

Neste exemplo divide o primeiro número, o segundo número:

```
mod(3, 2)
```

E devolver este resultado de: `1`

<a name="mul"></a>

## <a name="mul"></a>MUL

Devolva o produto de multiplicando dois números.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*multiplicand1*> | Sim | Um número inteiro ou número de vírgula flutuante | O número a multiplicar por *multiplicand2* | 
| <*multiplicand2*> | Sim | Um número inteiro ou número de vírgula flutuante | O número que múltiplos *multiplicand1* | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado do produto*> | Um número inteiro ou número de vírgula flutuante | O produto de multiplicando o primeiro número, o segundo número | 
|||| 

*Exemplo* 

Estes exemplos vários o primeiro número, o segundo número:

```
mul(1, 2)
mul(1.5, 2)
```

E devolver estes resultados:

* Primeiro exemplo: `2`
* Segundo exemplo `3`

<a name="multipartBody"></a>

## <a name="multipartbody"></a>multipartBody

Devolva o corpo de uma parte específica no resultado de uma ação que tem várias partes.

```
multipartBody('<actionName>', <index>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*actionName*> | Sim | Cadeia | O nome para a ação que tem de saída com várias partes | 
| <*Índice*> | Sim | Número inteiro | O valor de índice para a parte que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*Corpo*> | Cadeia | O corpo para a parte especificada | 
|||| 

<a name="not"></a>

## <a name="not"></a>não

Verifique se uma expressão é falsa. Devolve verdadeiro quando a expressão é falsa, ou falso quando VERDADEIRO de retorno.

```
not(<expression>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*expressão*> | Sim | Booleano | A expressão para verificar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando a expressão é falsa. Devolva falso quando a expressão for verdadeira. |  
|||| 

*Exemplo 1*

Estes exemplos verificam se as expressões especificadas são falsas: 

```
not(false)
not(true)
```

E devolver estes resultados:

* Primeiro exemplo: A expressão é falsa, pelo que a função devolve `true`.
* Segundo exemplo: A expressão for verdadeira, pelo que a função devolve `false`.

*Exemplo 2*

Estes exemplos verificam se as expressões especificadas são falsas: 

```
not(equals(1, 2))
not(equals(1, 1))
```

E devolver estes resultados:

* Primeiro exemplo: A expressão é falsa, pelo que a função devolve `true`.
* Segundo exemplo: A expressão for verdadeira, pelo que a função devolve `false`.

<a name="or"></a>

## <a name="or"></a>ou

Verifique se, pelo menos, uma expressão for verdadeira. Devolve verdadeiro quando, pelo menos, uma expressão for verdadeira ou devolver false quando todos os são falsos.

```
or(<expression1>, <expression2>, ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Expression1*>, <*expression2*>,... | Sim | Booleano | As expressões para verificar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO | Booleano | Devolve verdadeiro quando, pelo menos, uma expressão for verdadeira. Devolveu false quando todas as expressões forem falsas. |  
|||| 

*Exemplo 1*

Estes exemplos, verifique se, pelo menos, uma expressão for verdadeira:

```
or(true, false)
or(false, false)
```

E devolver estes resultados:

* Primeiro exemplo: pelo menos uma expressão for verdadeira, pelo que a função devolve `true`.
* Segundo exemplo: ambas as expressões são falsos, pelo que a função devolve `false`.

*Exemplo 2*

Estes exemplos, verifique se, pelo menos, uma expressão for verdadeira:

```
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

E devolver estes resultados:

* Primeiro exemplo: pelo menos uma expressão for verdadeira, pelo que a função devolve `true`.
* Segundo exemplo: ambas as expressões são falsos, pelo que a função devolve `false`.

<a name="parameters"></a>

## <a name="parameters"></a>parâmetros

Devolva o valor para um parâmetro que é descrito a definição da aplicação lógica. 

```
parameters('<parameterName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*parameterName*> | Sim | Cadeia | O nome para o parâmetro cujo valor que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor do parâmetro*> | Qualquer | O valor para o parâmetro especificado | 
|||| 

*Exemplo* 

Suponha que tem este valor JSON:

```json
{
  "fullName": "Sophia Owen"
}
```

Neste exemplo obtém o valor para o parâmetro especificado:

```
parameters('fullName')
```

E devolve o resultado deste: `"Sophia Owen"`

<a name="rand"></a>

## <a name="rand"></a>rand

Devolva um número inteiro aleatório a partir de um intervalo especificado, o que é inclusive apenas no final inicial.

```
rand(<minValue>, <maxValue>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*MinValue*> | Sim | Número inteiro | O valor mais baixo inteiro no intervalo | 
| <*MaxValue*> | Sim | Número inteiro | O número inteiro que se segue o maior número inteiro no intervalo que pode devolver a função | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado aleatório*> | Número inteiro | O número de inteiro aleatório devolvido a partir do intervalo especificado |  
|||| 

*Exemplo*

Neste exemplo obtém um número aleatório inteiro no intervalo especificado, excluindo o valor máximo de: 

```
rand(1, 5)
```

E devolve um destes números como resultado: `1`, `2`, `3`, ou `4` 

<a name="range"></a>

## <a name="range"></a>intervalo

Devolva uma matriz de número inteiro que é iniciado a partir de um número inteiro especificado.

```
range(<startIndex>, <count>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*startIndex*> | Sim | Número inteiro | O valor de número inteiro que começa a matriz de como o primeiro item | 
| <*Contagem*> | Sim | Número inteiro | O número de números inteiros na matriz | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*intervalo resultado*>] | Array | A matriz com números inteiros a partir do índice especificado |  
|||| 

*Exemplo*

Este exemplo cria uma matriz de número inteiro que inicia a partir do índice especificado e tem o número especificado de números inteiros:

```
range(1, 4)
```

E devolve o resultado deste: `[1, 2, 3, 4]`

<a name="replace"></a>

## <a name="replace"></a>Substituir

Substituir uma subcadeia com a cadeia especificada e devolve a cadeia de resultado. Esta função é maiúsculas e minúsculas.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia que tenha a subcadeia para substituir | 
| <*oldText*> | Sim | Cadeia | A subcadeia para substituir | 
| <*newText*> | Sim | Cadeia | A cadeia de substituição | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*texto atualizado*> | Cadeia | A cadeia atualizada depois de substituir a subcadeia <p>Se a subcadeia não for encontrada, devolve da cadeia original. | 
|||| 

*Exemplo* 

Este exemplo localiza a subcadeia "antiga" em "a antiga cadeia" e substitui "antigo" com "novo": 

```
replace('the old string', 'old', 'new')
```

E devolve o resultado deste: `"the new string"`

<a name="removeProperty"></a>

## <a name="removeproperty"></a>removeProperty

Remover uma propriedade de um objeto e retorno do objeto atualizado.

```
removeProperty(<object>, '<property>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*objeto*> | Sim | Object | O objeto JSON na qual pretende remover uma propriedade | 
| <*Propriedade*> | Sim | Cadeia | O nome da propriedade remover | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*objeto atualizado*> | Object | O objeto JSON atualizado sem a propriedade especificada | 
|||| 

*Exemplo*

Neste exemplo remove o `"accountLocation"` propriedade de um `"customerProfile"` objeto, que é convertido em JSON com o [JSON()](#json) funcionar e devolve o objeto atualizado:

```
removeProperty(json('customerProfile'), 'accountLocation')
```

<a name="setProperty"></a>

## <a name="setproperty"></a>setProperty

Defina o valor da propriedade de um objeto e devolver o objeto atualizado. Para adicionar uma nova propriedade, pode utilizar esta função ou o [addProperty()](#addProperty) função.

```
setProperty(<object>, '<property>', <value>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*objeto*> | Sim | Object | O objeto JSON cuja propriedade pretende definir | 
| <*Propriedade*> | Sim | Cadeia | O nome para a propriedade nova ou existente definir | 
| <*Valor*> | Sim | Qualquer | O valor definido para a propriedade especificada |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*objeto atualizado*> | Object | O objeto JSON cuja propriedade definir atualizado | 
|||| 

*Exemplo*

Neste exemplo define o `"accountNumber"` propriedade um `"customerProfile"` objeto, que é convertido em JSON com o [JSON()](#json) função. A função atribui um valor gerado pelo [guid()](#guid) funcionar e devolve o objeto JSON atualizado:

```
setProperty(json('customerProfile'), 'accountNumber', guid())
```

<a name="skip"></a>

## <a name="skip"></a>Ignorar

Remover itens da frente de uma coleção e devolver *todas as outras* itens.

```
skip([<collection>], <count>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Array | A coleção cujos itens que pretende remover | 
| <*Contagem*> | Sim | Número inteiro | Um número inteiro positivo para o número de itens a remover em início | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*coleção atualizados*>] | Array | A coleção atualizada depois de remover os itens especificados | 
|||| 

*Exemplo*

Neste exemplo remove um item, o número de 0, do início da matriz especificada: 

```
skip([0, 1, 2, 3], 1)
```

E devolve esta matriz com os restantes itens: `[1,2,3]`

<a name="split"></a>

## <a name="split"></a>dividir

Devolve uma matriz que tem todos os carateres de uma cadeia e tem cada caráter separada por um *delimitador*.

```
split('<text>', '<separator>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia com os carateres dividir |  
| <*separador*> | Sim | Cadeia | O separador que aparece entre cada caráter na matriz resultante | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*char1*><*separador*><*char2*><*separador*>...] | Array | A matriz resultante criada a partir de todos os itens a cadeia especificada |
|||| 

*Exemplo* 

Este exemplo cria uma matriz da cadeia especificada, separando cada caráter com uma vírgula como o delimitador de:

```
split('abc', ',')
```

E devolve o resultado deste: `[a, b, c]`

<a name="startOfDay"></a>

## <a name="startofday"></a>startOfDay

Devolva o início do dia para um carimbo. 

```
startOfDay('<timestamp>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo especificado mas começando na marca de hora de zero para o dia | 
|||| 

*Exemplo* 

Neste exemplo localiza o início do dia para este carimbo:

```
startOfDay('2018-03-15T13:30:30Z')
```

E devolve o resultado deste: `"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

## <a name="startofhour"></a>startOfHour

Devolva o início da hora para um carimbo. 

```
startOfHour('<timestamp>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo especificado mas começando na marca de minuto de zero para a hora | 
|||| 

*Exemplo* 

Neste exemplo localiza o início da hora para este carimbo:

```
startOfHour('2018-03-15T13:30:30Z')
```

E devolve o resultado deste: `"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

## <a name="startofmonth"></a>startOfMonth

Devolva o início do mês para um carimbo. 

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo especificado mas começando no primeiro dia do mês na marca de hora de zero | 
|||| 

*Exemplo* 

Neste exemplo devolve o início do mês para este carimbo:

```
startOfMonth('2018-03-15T13:30:30Z')
```

E devolve o resultado deste: `"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

## <a name="startswith"></a>startsWith

Verificar se uma cadeia começa com uma subcadeia específica. Devolve verdadeiro quando a subcadeia for encontrada, ou de retorno falso quando não foi encontrado. Esta função não é sensível.

```
startsWith('<text>', '<searchText>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia de verificação | 
| <*searchText*> | Sim | Cadeia | A cadeia de partida para localizar | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| VERDADEIRO ou FALSO  | Booleano | Devolve verdadeiro quando a subcadeia inicial é encontrada. Devolveu false quando não foi encontrado. | 
|||| 

*Exemplo 1* 

Neste exemplo verifica se a cadeia "Olá mundo" começa com a subcadeia de "Olá":

```
startsWith('hello world', 'hello')
```

E devolve o resultado deste: `true`

*Exemplo 2*

Neste exemplo verifica se a cadeia "Olá mundo" começa com a subcadeia "saudações":

```
startsWith('hello world', 'greetings')
```

E devolve o resultado deste: `false`

<a name="string"></a>

## <a name="string"></a>cadeia

Devolva a versão de cadeia para um valor.

```
string(<value>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Qualquer | O valor a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de cadeia*> | Cadeia | A versão de cadeia para o valor especificado | 
|||| 

*Exemplo 1* 

Este exemplo cria a versão de cadeia para este número:

```
string(10)
```

E devolve o resultado deste: `"10"`

*Exemplo 2*

Este exemplo cria uma cadeia para o objeto JSON especificado e utiliza o caráter de barra invertida (\\) como um caráter de escape para o valor de duplo-aspas (").

```
string( { "name": "Sophie Owen" } )
```

E devolve o resultado deste: `"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

## <a name="sub"></a>sub

Devolve o resultado de subtraindo o segundo número do primeiro número.

```
sub(<minuend>, <subtrahend>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*minuend*> | Sim | Um número inteiro ou número de vírgula flutuante | O número do qual subtrair a *subtrahend* | 
| <*subtrahend*> | Sim | Um número inteiro ou número de vírgula flutuante | O número subtrair a *minuend* | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado*> | Um número inteiro ou número de vírgula flutuante | O resultado do subtraindo o segundo número do primeiro número | 
|||| 

*Exemplo* 

Neste exemplo subtrai o segundo número do número primeiro:

```
sub(10.3, .3)
```

E devolve o resultado deste: `10`

<a name="substring"></a>

## <a name="substring"></a>subcadeia

Caracteres de retorno de uma cadeia, a partir da posição especificada, ou o índice. Início de valores de índice com o número 0. 

```
substring('<text>', <startIndex>, <length>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia cujos carateres que pretende | 
| <*startIndex*> | Sim | Número inteiro | Um número positivo para a posição inicial ou um valor de índice | 
| <*comprimento*> | Sim | Número inteiro | Um número positivo de carateres que pretende incluir a subcadeia | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*resultado da subcadeia*> | Cadeia | Uma subcadeia com o número especificado de carateres, a partir da posição de índice especificado na cadeia de origem | 
|||| 

*Exemplo* 

Este exemplo cria uma subcadeia de cinco carateres da cadeia especificada, a partir do valor de índice 6:

```
substring('hello world', 6, 5)
```

E devolve o resultado deste: `"world"`

<a name="subtractFromTime"></a>

## <a name="subtractfromtime"></a>subtractFromTime

Subtrair um número de unidades de tempo de um carimbo. Consulte também [getPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia que contém o timestamp | 
| <*intervalo*> | Sim | Número inteiro | O número de unidades de tempo especificado a subtrair | 
| <*timeUnit*> | Sim | Cadeia | A unidade de tempo para utilizar com *intervalo*: "Segunda", "Minutos", "Horas", "Dia", "Semanas", "Meses", "Ano" | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atualizado*> | Cadeia | O carimbo menos o número de unidades de tempo especificado | 
|||| 

*Exemplo 1*

Neste exemplo subtrai um dia deste timestamp:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day') 
```

E devolve o resultado deste: `"2018-01-01T00:00:00:0000000Z"`

*Exemplo 2*

Neste exemplo subtrai um dia deste timestamp:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D') 
```

E devolve o resultado deste utilizando o formato "D" opcional: `"Monday, January, 1, 2018"`

<a name="take"></a>

## <a name="take"></a>tirar

Itens de retorno do início de uma coleção. 

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Coleção*> | Sim | Cadeia ou matriz | A coleção cujos itens que pretende | 
| <*Contagem*> | Sim | Número inteiro | Um número inteiro positivo para o número de itens que pretende que o do início | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*subconjunto*> ou [<*subconjunto*>] | Cadeia ou matriz, respetivamente | Uma cadeia ou matriz com o número especificado de itens obtidas a partir do início da coleção original | 
|||| 

*Exemplo*

Estes exemplos obter o número especificado de itens da frente destas coleções:

```
take('abcde`, 3)
take([0, 1, 2, 3, 4], 3)
```

E devolver estes resultados:

* Primeiro exemplo: `"abc"`
* Segundo exemplo: `[0, 1, 2]`

<a name="ticks"></a>

## <a name="ticks"></a>tiques

Devolver o `ticks` valor da propriedade para um carimbo especificado. A *marcas de escala* é um intervalo de 100 nanossegundos.

```
ticks('<timestamp>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Timestamp*> | Sim | Cadeia | A cadeia para um carimbo | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*número de ticks*> | Número inteiro | O número de ticks desde o carimbo especificado | 
|||| 

<a name="toLower"></a>

## <a name="tolower"></a>toLower

Devolva uma cadeia no formato em minúsculas. Se um carácter na cadeia de não tiver uma versão em minúsculas, esse caráter permanece inalterado na cadeia devolvida.

```
toLower('<text>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia a devolver no formato em minúsculas | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*texto em minúsculas*> | Cadeia | A cadeia no formato minúsculas original | 
|||| 

*Exemplo* 

Neste exemplo converte esta cadeia em minúsculas: 

```
toLower('Hello World')
```

E devolve o resultado deste: `"hello world"`

<a name="toUpper"></a>

## <a name="toupper"></a>toUpper

Devolva uma cadeia no formato maiúsculas. Se um carácter na cadeia de não tiver uma versão em maiúsculas, esse caráter permanece inalterado na cadeia devolvida.

```
toUpper('<text>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia a devolver no formato maiúsculas | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*texto em maiúsculas*> | Cadeia | A cadeia no formato maiúsculas original | 
|||| 

*Exemplo* 

Neste exemplo converte esta cadeia em maiúsculas:

```
toUpper('Hello World')
```

E devolve o resultado deste: `"HELLO WORLD"`

<a name="trigger"></a>

## <a name="trigger"></a>Acionador

Devolva o resultado de um acionador em tempo de execução ou valores a partir de outros pares de nome e valor JSON, o que pode atribuir a uma expressão. 

* Dentro de entradas de um acionador, esta função devolve o resultado da execução anterior. 

* Dentro de condição de um acionador, esta função devolve o resultado da execução atual. 

Por predefinição, a função referencia o objeto de Acionador de todo, mas pode especificar opcionalmente uma propriedade cujo valor que pretende. Além disso, esta função tem as versões de expressão compacta disponíveis, consulte [triggerOutputs()](#triggerOutputs) e [triggerBody()](#triggerBody). 

```
trigger()
```

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*saída de Acionador*> | Cadeia | O resultado de um acionador em tempo de execução | 
|||| 

<a name="triggerBody"></a>

## <a name="triggerbody"></a>triggerBody

Devolver um acionador `body` saída no tempo de execução. A abreviatura para `trigger().outputs.body`. Consulte [trigger()](#trigger). 

```
triggerBody()
```

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*saída de corpo de Acionador*> | Cadeia | O `body` o acionador de saída | 
|||| 

<a name="triggerFormDataMultiValues"></a>

## <a name="triggerformdatamultivalues"></a>triggerFormDataMultiValues

Devolver uma matriz com valores que correspondam a um nome de chave num acionador *dados do formulário* ou *codificado de formulário* saída. 

```
triggerFormDataMultiValues('<key>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*chave*> | Sim | Cadeia | O nome da chave cujo valor que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| [<*matriz com chave valores*>] | Array | Uma matriz com todos os valores que correspondem a chave especificada | 
|||| 

*Exemplo* 

Este exemplo cria uma matriz do valor da chave "feedUrl" dados do formulário ou saída de formulário com codificação de um acionador RSS: 

```
triggerFormDataMultiValues('feedUrl')
```

E devolve esta matriz como um resultado de exemplo: `["http://feeds.reuters.com/reuters/topNews"]`

<a name="triggerFormDataValue"></a>

## <a name="triggerformdatavalue"></a>triggerFormDataValue

Devolver uma cadeia com um valor único que corresponda a um nome de chave num acionador *dados do formulário* ou *codificado de formulário* saída. Se a função localizar mais do que uma correspondência, a função emite um erro.

```
triggerFormDataValue('<key>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*chave*> | Sim | Cadeia | O nome da chave cujo valor que pretende |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*chave-valor*> | Cadeia | O valor da chave especificada | 
|||| 

*Exemplo* 

Este exemplo cria uma cadeia a partir do valor de chave "feedUrl" dados do formulário ou saída de formulário com codificação de um acionador RSS:

```
triggerFormDataValue('feedUrl')
```

E devolve esta cadeia como um resultado de exemplo: `"http://feeds.reuters.com/reuters/topNews"` 

<a name="triggerMultipartBody"></a>

Devolva o corpo de uma parte específica na saída de um acionador que tem várias partes. 

```
triggerMultipartBody(<index>)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Índice*> | Sim | Número inteiro | O valor de índice para a parte que pretende |
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*Corpo*> | Cadeia | O corpo para a parte especificada na saída de várias partes de um acionador | 
|||| 

<a name="triggerOutputs"></a>

## <a name="triggeroutputs"></a>triggerOutputs

Devolva o resultado de um acionador em tempo de execução ou valores a partir de outros pares de nome e valor JSON. A abreviatura para `trigger().outputs`. Consulte [trigger()](#trigger). 

```
triggerOutputs()
```

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*saída de Acionador*> | Cadeia | O resultado de um acionador em tempo de execução  | 
|||| 

<a name="trim"></a>

## <a name="trim"></a>Cortar

Remover espaços em branco à direita e à esquerda de uma cadeia e devolver a cadeia atualizada.

```
trim('<text>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Texto*> | Sim | Cadeia | A cadeia com o espaço em branco de direita e à esquerda para remover | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*updatedText*> | Cadeia | Uma versão atualizada da cadeia original sem espaços em branco à esquerda nem à direita | 
|||| 

*Exemplo* 

Neste exemplo remove o espaço em branco à direita e à esquerda da cadeia "Olá mundo":  

```
trim(' Hello World  ')
```

E devolve o resultado deste: `"Hello World"`

<a name="union"></a>

## <a name="union"></a>União

Devolver uma coleção que tenha *todos os* os itens às coleções especificadas. A aparecer nos resultados, um item pode aparecer em qualquer coleção transmitida para esta função. Se um ou mais itens têm o mesmo nome, o último item com esse nome é apresentado nos resultados. 

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*collection1*>, <*collection2*>,...  | Sim | Matriz ou o objeto, mas não ambos | As coleções de onde pretende *todos os* os itens | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*updatedCollection*> | Matriz nem um objeto, respetivamente | Uma coleção com todos os itens de coleções especificadas - sem duplicados | 
|||| 

*Exemplo* 

Neste exemplo obtém *todos os* os itens destas coleções: 

```
union([1, 2, 3], [1, 2, 10, 101])
```

E devolve o resultado deste: `[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

## <a name="uricomponent"></a>uriComponent

Devolva uma versão de identificador (URI) codificado de recurso uniforme uma cadeia, substituindo carateres não seguro URL com carateres de escape. Utilize esta função vez [encodeUriComponent()](#encodeUriComponent). Embora ambas as funções funcionam da mesma forma, `uriComponent()` preferido.

```
uriComponent('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia a converter para formato com codificação URI | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*uri codificado*> | Cadeia | A cadeia URI codificado com carateres de escape | 
|||| 

*Exemplo*

Este exemplo cria uma versão codificado de URI para esta cadeia:

```
uriComponent('https://contoso.com')
```

E devolve o resultado deste: `"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

## <a name="uricomponenttobinary"></a>uriComponentToBinary

Devolva a versão binária para um componente do recurso uniforme (URI) do identificador.

```
uriComponentToBinary('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com codificação URI a converter | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*binário-para-codificado-uri*> | Cadeia | A versão binária para a cadeia com codificação URI. O conteúdo do binário é representado pelo e com codificação base64 `$content`. | 
|||| 

*Exemplo*

Este exemplo cria a versão binária para esta cadeia codificados do URI: 

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

E devolve o resultado deste: 

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

## <a name="uricomponenttostring"></a>uriComponentToString

Devolva que a versão de cadeia para um identificador de recurso uniforme (URI) codificado cadeia, de forma eficaz descodificar a cadeia com codificação URI.

```
uriComponentToString('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com codificação URI para descodificação | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*descodificar-uri*> | Cadeia | A versão descodificada para a cadeia com codificação URI | 
|||| 

*Exemplo*

Este exemplo cria a versão de cadeia descodificada para esta cadeia codificados do URI: 

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

E devolve o resultado deste: `"https://contoso.com"` 

<a name="uriHost"></a>

## <a name="urihost"></a>uriHost

Devolver o `host` valor para um identificador de recurso uniforme (URI).

```
uriHost('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `host` valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor do anfitrião*> | Cadeia | O `host` valor para o URI especificado | 
|||| 

*Exemplo*

Neste exemplo localiza o `host` valor para este URI: 

```
uriHost('https://www.localhost.com:8080')
```

E devolve o resultado deste: `"www.localhost.com"`

<a name="uriPath"></a>

## <a name="uripath"></a>uriPath

Devolver o `path` valor para um identificador de recurso uniforme (URI). 

```
uriPath('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `path` valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de caminho*> | Cadeia | O `path` valor para o URI especificado. Se `path` não ter um valor, devolve o caráter "/". | 
|||| 

*Exemplo*

Neste exemplo localiza o `path` valor para este URI: 

```
uriPath('http://www.contoso.com/catalog/shownew.htm?date=today')
```

E devolve o resultado deste: `"/catalog/shownew.htm"`

<a name="uriPathAndQuery"></a>

## <a name="uripathandquery"></a>uriPathAndQuery

Devolver o `path` e `query` valores para um identificador de recurso uniforme (URI).

```
uriPathAndQuery('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `path` e `query` valores pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de consulta de caminho*> | Cadeia | O `path` e `query` valores para o URI especificado. Se `path` não especificar um valor, devolve o caráter "/". | 
|||| 

*Exemplo*

Neste exemplo localiza o `path` e `query` valores para este URI:

```
uriPathAndQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

E devolve o resultado deste: `"/catalog/shownew.htm?date=today"`

<a name="uriPort"></a>

## <a name="uriport"></a>uriPort

Devolver o `port` valor para um identificador de recurso uniforme (URI).

```
uriPort('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `port` valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de porta*> | Número inteiro | O `port` valor para o URI especificado. Se `port` não especificar um valor, a porta predefinida para o protocolo de retorno. | 
|||| 

*Exemplo*

Este exemplo devolve o `port` valor para este URI:

```
uriPort('http://www.localhost:8080')
```

E devolve o resultado deste: `8080`

<a name="uriQuery"></a>

## <a name="uriquery"></a>uriQuery

Devolver o `query` valor para um identificador de recurso uniforme (URI).

```
uriQuery('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `query` valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de consulta*> | Cadeia | O `query` valor para o URI especificado | 
|||| 

*Exemplo*

Este exemplo devolve o `query` valor para este URI: 

```
uriQuery('http://www.contoso.com/catalog/shownew.htm?date=today')
```

E devolve o resultado deste: `"?date=today"`

<a name="uriScheme"></a>

## <a name="urischeme"></a>UriScheme

Devolver o `scheme` valor para um identificador de recurso uniforme (URI).

```
uriScheme('<uri>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*URI*> | Sim | Cadeia | O URI cujo `scheme` valor | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor de esquema*> | Cadeia | O `scheme` valor para o URI especificado | 
|||| 

*Exemplo*

Este exemplo devolve o `scheme` valor para este URI:

```
uriScheme('http://www.contoso.com/catalog/shownew.htm?date=today')
```

E devolve o resultado deste: `"http"`

<a name="utcNow"></a>

## <a name="utcnow"></a>utcNow

Devolva o timestamp atual. 

```
utcNow('<format>')
```

Opcionalmente, pode especificar um formato diferente com o <*formato*> parâmetro. 


| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*formato*> | Não | Cadeia | É um [especificador de formato único](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ou um [padrão de formato personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). O formato predefinido para o timestamp é ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (AAAA-MM-ddT:mm:ss:fffffffK), que está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e preserva informações de fuso horário. | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*timestamp atual*> | Cadeia | A data e hora atuais | 
|||| 

*Exemplo 1*

Suponhamos hoje é 15 de Abril de 2018 1:00:00 PM. Neste exemplo obtém o timestamp atual: 

```
utcNow()
```

E devolve o resultado deste: `"2018-04-15T13:00:00.0000000Z"`

*Exemplo 2*

Suponhamos hoje é 15 de Abril de 2018 1:00:00 PM. Neste exemplo obtém o timestamp atual utilizando o formato "D" opcional:

```
utcNow('D')
```

E devolve o resultado deste: `"Sunday, April 15, 2018"`

<a name="variables"></a>

## <a name="variables"></a>variáveis

Devolva o valor para uma variável especificada. 

```
variables('<variableName>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*variableName*> | Sim | Cadeia | O nome da variável cujo valor que pretende | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*valor da variável*> | Qualquer | O valor da variável especificada | 
|||| 

*Exemplo*

Suponha que o valor atual para uma variável de "numItems" é 20. Neste exemplo obtém o valor de número inteiro para esta variável:

```
variables('numItems')
```

E devolve o resultado deste: `20`

<a name="workflow"></a>

## <a name="workflow"></a>fluxo de trabalho

Devolva todos os detalhes sobre o fluxo de trabalho durante o tempo de execução. 

```
workflow().<property>
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Propriedade*> | Não | Cadeia | O nome para a propriedade de fluxo de trabalho cujo valor que pretende <p>Um objeto de fluxo de trabalho com estas propriedades: **nome**, **tipo**, **id**, **localização**, e **executar**. O **executar** valor da propriedade também é um objeto com estas propriedades: **nome**, **tipo**, e **id**. | 
||||| 

*Exemplo*

Neste exemplo devolve o nome de execução atual de um fluxo de trabalho:

```
workflow().run.name
```

<a name="xml"></a>

## <a name="xml"></a>xml

Devolva a versão XML para uma cadeia que contém um objeto JSON. 

```
xml('<value>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*Valor*> | Sim | Cadeia | A cadeia com o objeto JSON a converter <p>O objeto JSON tem de ter a propriedade de raiz apenas um. <br>Utilizar o caráter de barra invertida (\\) como um caráter de escape para as aspas aspas ("). | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*versão de XML*> | Object | O XML para a cadeia especificada ou o objeto JSON codificado | 
|||| 

*Exemplo 1*

Este exemplo cria a versão XML para esta cadeia que contém um objeto JSON: 

`xml( '{ \"name\": \"Sophia Owen\" }' )`

E devolve o resultado deste XML: 

```xml
<name>Sophia Owen</name>
```

*Exemplo 2*

Suponha que tem este objeto JSON:

```json
{ 
  "person": { 
    "name": "Sophia Owen", 
    "city": "Seattle" 
  } 
}
```

Este exemplo cria XML de uma cadeia que contém este objeto JSON:

`xml( '{ \"person\": { \"name\": \"Sophia Owen\", \"city\": \"Seattle\" } }' )`

E devolve o resultado deste XML: 

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

## <a name="xpath"></a>XPath

Verifique o XML para nós ou valores que correspondem a uma expressão de XPath (linguagem XML) e devolver valores ou nós correspondente. Uma expressão de XPath ou apenas "XPath", poderá navegar uma estrutura de documento XML, de modo a que possa selecionar nós ou valores de computação no conteúdo XML.

```
xpath('<xml>', '<xpath>')
```

| Parâmetro | Necessário | Tipo | Descrição | 
| --------- | -------- | ---- | ----------- | 
| <*XML*> | Sim | Qualquer | A cadeia XML para procurar nós ou valores que correspondam a um valor de expressão de XPath | 
| <*XPath*> | Sim | Qualquer | A expressão XPath utilizada para localizar o nó XML ou valores correspondente | 
||||| 

| Valor devolvido | Tipo | Descrição | 
| ------------ | ---- | ----------- | 
| <*nó XML*> | XML | Um nó XML quando apenas um único nó corresponde a expressão XPath especificada | 
| <*Valor*> | Qualquer | O valor de um nó XML quando apenas um valor único corresponde a expressão XPath especificada | 
| [<*xml Nó1*>, <*xml Nó2*>,...] </br>-ou- </br>[<*value1*>, <*value2*>,...] | Array | Uma matriz connosco XML ou os valores que correspondem a expressão XPath especificada | 
|||| 

*Exemplo 1*

Neste exemplo localiza nós que correspondem a `<name></name>` nó os argumentos especificados e devolve uma matriz com esses valores de nó: 

`xpath(xml(parameters('items')), '/produce/item/name')`

Seguem-se os argumentos:

* A cadeia "itens", que contém este XML:

  `"<?xml version="1.0"?> <produce> <item> <name>Gala</name> <type>apple</type> <count>20</count> </item> <item> <name>Honeycrisp</name> <type>apple</type> <count>10</count> </item> </produce>"`

  O exemplo utiliza o [parameters()](#parameters) funcionar para obter a cadeia XML no argumento "itens", mas tem também de converter a cadeia de formato XML utilizando o [xml()](#xml) função. 

* Esta expressão XPath que é transmitida como uma cadeia:

  `"/produce/item/name"`

Segue-se a matriz de resultado com os nós que corresponde ao `<name></name`:

`[ <name>Gala</name>, <name>Honeycrisp</name> ]`

*Exemplo 2*

A seguir no exemplo 1, neste exemplo localiza nós que correspondem a `<count></count>` nó e adiciona esses valores de nó com o `sum()` função:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

E devolve o resultado deste: `30`

*Exemplo 3*

Neste exemplo, ambas as expressões localizar nós que correspondem a `<location></location>` nó, os argumentos especificados, que incluem a XML com um espaço de nomes. As expressões de utilizam o caráter de barra invertida (\\) como um caráter de escape para as aspas aspas (").

* *Expressão 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *Expressão 2* 

  `xpath(xml(body('Http')), '/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]')`

Seguem-se os argumentos:

* Este XML, que inclui o espaço de nomes do documento XML, `xmlns="http://contoso.com"`: 

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* A expressão aqui de XPath:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name=()=\"file\"] and namespace-uri()=\"http://contoso.com\"/*[local-name()]=\"location\" and namespace-uri()=\"\"]`

Eis o nó de resultado que corresponda a `<location></location` nó:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Exemplo 4*

A seguir no exemplo 3, neste exemplo localiza o valor de `<location></location>` nó: 

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

E devolve o resultado deste: `"Paris"`

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md)
