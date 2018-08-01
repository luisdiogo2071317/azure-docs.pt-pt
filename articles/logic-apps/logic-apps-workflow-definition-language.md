---
title: Referência de esquema para a linguagem de definição de fluxo de trabalho - Azure Logic Apps | Documentos da Microsoft
description: Escrever definições de fluxo de trabalho personalizado para o Azure Logic Apps com a linguagem de definição de fluxo de trabalho
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: reference
ms.date: 04/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 0ac191f1191357ecc7292d51b298567f7f4e4786
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391229"
---
# <a name="schema-reference-for-workflow-definition-language-in-azure-logic-apps"></a>Referência de esquema para a linguagem de definição de fluxo de trabalho no Azure Logic Apps

Quando cria um fluxo de trabalho de aplicação lógica com [do Azure Logic Apps](../logic-apps/logic-apps-overview.md), definição subjacente do seu fluxo de trabalho descreve a lógica real que é executada para a aplicação lógica. Esta descrição segue uma estrutura que tenha definido e validadas pelo esquema de linguagem de definição de fluxo de trabalho, que usa [JavaScript Object Notation (JSON)](https://www.json.org/). 
  
## <a name="workflow-definition-structure"></a>Estrutura de definição de fluxo de trabalho

Uma definição de fluxo de trabalho tem, pelo menos, um acionador que cria uma instância da sua aplicação lógica, além de uma ou mais ações que executa a aplicação lógica. 

Esta é a estrutura de alto nível para uma definição de fluxo de trabalho:  
  
```json
"definition": {
  "$schema": "<workflow-definition-language-schema-version>",
  "contentVersion": "<workflow-definition-version-number>",
  "parameters": { "<workflow-parameter-definitions>" },
  "triggers": { "<workflow-trigger-definitions>" },
  "actions": { "<workflow-action-definitions>" },
  "outputs": { "<workflow-output-definitions>" }
}
```
  
| Elemento | Necessário | Descrição | 
|---------|----------|-------------| 
| definição | Sim | O elemento de partida para a sua definição de fluxo de trabalho | 
| $schema | Apenas quando externamente que referencia uma definição de fluxo de trabalho | A localização para o ficheiro de esquema JSON que descreve a versão de linguagem de definição de fluxo de trabalho, que pode ser encontrado aqui: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Não | O número de versão para a sua definição de fluxo de trabalho, o que é "1.0.0.0" por predefinição. Para ajudar a identificar e confirmar a definição correta quando implementar um fluxo de trabalho, especifique um valor a utilizar. | 
| parâmetros | Não | As definições para um ou mais parâmetros que passam dados para seu fluxo de trabalho <p><p>Parâmetros máximos: 50 | 
| acionadores | Não | As definições para um ou mais disparadores que criar uma instância de seu fluxo de trabalho. Pode definir mais do que um acionador, mas apenas com a linguagem de definição de fluxo de trabalho, não visualmente por meio do Designer de aplicações lógicas. <p><p>Acionadores máximos: 10 | 
| ações | Não | As definições para uma ou mais ações executar em tempo de execução do fluxo de trabalho <p><p>Ações máximas: 250 | 
| saídas | Não | As definições para as saídas que retornam a partir de um fluxo de trabalho execute <p><p>Saídas máximas: 10 |  
|||| 

## <a name="parameters"></a>Parâmetros

Na `parameters` secção, definir todos os parâmetros de fluxo de trabalho que a aplicação lógica utiliza na implementação, para aceitar entradas. Declarações de parâmetros e valores de parâmetros são necessários na implementação. Antes de poder utilizar estes parâmetros em outras seções de fluxo de trabalho, certifique-se de que declarar todos os parâmetros nestas secções. 

Esta é a estrutura geral para uma definição de parâmetro:  

```json
"parameters": {
  "<parameter-name>": {
    "type": "<parameter-type>",
    "defaultValue": "<default-parameter-value>",
    "allowedValues": [ <array-with-permitted-parameter-values> ],
    "metadata": { 
      "key": { 
        "name": "<key-value>"
      } 
    }
  }
},
```

| Elemento | Necessário | Tipo | Descrição |  
|---------|----------|------|-------------|  
| tipo | Sim | int, número de vírgula flutuante, cadeia de caracteres, securestring, booleano, matriz, objeto JSON, secureobject <p><p>**Tenha em atenção**: para todas as palavras-passe, chaves e segredos, utilize o `securestring` e `secureobject` tipos porque o `GET` operação não retorna esses tipos. | O tipo para o parâmetro |
| defaultValue | Não | Mesmo que `type` | O valor de parâmetro predefinido quando é especificado nenhum valor quando cria uma instância de fluxo de trabalho | 
| allowedValues | Não | Mesmo que `type` | Uma matriz com valores que pode aceitar o parâmetro |  
| do IdP | Não | Objeto JSON | Quaisquer outros detalhes de parâmetro, por exemplo, o nome ou uma descrição legível para a sua aplicação lógica ou dados de tempo de design usados pelo Visual Studio ou outras ferramentas |  
||||

## <a name="triggers-and-actions"></a>Acionadores e ações  

Numa definição de fluxo de trabalho, o `triggers` e `actions` secções definem as chamadas que ocorrem durante a execução do fluxo de trabalho. Para sintaxe e obter mais informações sobre estas secções, consulte [acionadores de fluxo de trabalho e as ações](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Saídas 

Na `outputs` secção, definir os dados que o fluxo de trabalho pode retornar quando terminar em execução. Por exemplo, para controlar um estado específico ou um valor de cada execução, especifique que a saída de fluxo de trabalho retorna esses dados. 

> [!NOTE]
> Quando a responder a pedidos recebidos a partir da API de um serviço REST, não utilize `outputs`. Em alternativa, utilize o `Response` tipo de ação. Para obter mais informações, consulte [acionadores de fluxo de trabalho e as ações](../logic-apps/logic-apps-workflow-actions-triggers.md).

Esta é a estrutura geral para obter uma definição de saída: 

```json
"outputs": {
  "<key-name>": {  
    "type": "<key-type>",  
    "value": "<key-value>"  
  }
} 
```

| Elemento | Necessário | Tipo | Descrição | 
|---------|----------|------|-------------| 
| <*nome da chave*> | Sim | Cadeia | O nome da chave para a saída de valor de retorno |  
| tipo | Sim | int, número de vírgula flutuante, cadeia de caracteres, securestring, booleano, matriz, objeto JSON | O tipo para o valor de retorno de saída | 
| valor | Sim | Mesmo que `type` | O valor de retorno de saída |  
||||| 

Para obter o resultado de um fluxo de trabalho execute, reveja o histórico de execuções e detalhes no portal do Azure da aplicação lógica ou utilize o [API do REST de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows). Também é possível passar saída a sistemas externos, por exemplo, o Power BI para que possa criar dashboards. 

<a name="expressions"></a>

## <a name="expressions"></a>Expressões

Com o JSON, pode ter valores literais que existem no tempo de design, por exemplo:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Também pode ter valores que não existem até o Runtime. Para representar estes valores, pode usar *expressões*, que é avaliado no tempo de execução. Uma expressão é uma seqüência que pode conter um ou mais [funções](#functions), [operadores](#operators), variáveis, valores explícitos ou constantes. Na sua definição de fluxo de trabalho, pode usar uma expressão em qualquer lugar num valor de cadeia de caracteres do JSON atribuindo a expressão com no início de sessão (\@). Quando a avaliação de uma expressão que representa um valor JSON, o corpo da expressão é extraído, removendo o \@ caractere e, sempre resulta em outro valor JSON. 

Por exemplo, para definido anteriormente `customerName` propriedade, pode obter o valor da propriedade com o [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) numa expressão de função e atribuir esse valor para o `accountName` propriedade:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Interpolação de cadeias de caracteres* também permitem-lhe utiliza várias expressões no interior de cadeias de caracteres que são compactadas com o \@ caráter e das chaves ({}). Esta é a sintaxe:

```json
@{ "<expression1>", "<expression2>" }
```

O resultado é sempre uma cadeia de caracteres, tornando essa funcionalidade semelhante do `concat()` funcione, por exemplo: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Se tiver uma cadeia literal que começa com o \@ caráter, o prefixo a \@ caractere com outra \@ caractere como um caráter de escape: \@\@

Estes exemplos mostram como as expressões são avaliadas:

| Valor JSON | Resultado |
|------------|--------| 
| "Sophia Owen" | Devolver estes carateres: 'Sophia Owen' |
| "matriz [1]" | Devolver estes carateres: "array [1]" |
| "\@\@" | Devolver estes carateres como uma cadeia de caracteres de um caractere: '\@' |   
| " \@" | Devolver estes carateres como uma cadeia de caracteres de dois caracteres: ' \@' |
|||

Para estes exemplos, vamos supor que definir "myBirthMonth" igual a "Janeiro" e "myAge", igual ao número 42:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Estes exemplos mostram como são avaliadas as expressões seguintes:

| Expressão de JSON | Resultado |
|-----------------|--------| 
| "\@parameters('myBirthMonth')" | Retornar essa cadeia de caracteres: "Janeiro" |  
| "\@{parameters('myBirthMonth')}" | Retornar essa cadeia de caracteres: "Janeiro" |  
| "\@parameters('myAge')" | Devolver este número: 42 |  
| "\@{parameters('myAge')}" | Devolver este número como uma cadeia de caracteres: "42" |  
| "Minha idade é \@{parameters('myAge')}" | Retornar essa cadeia de caracteres: "a minha idade é 42" |  
| "\@concat ("minha idade é", string(parameters('myAge')))" | Retornar essa cadeia de caracteres: "a minha idade é 42" |  
| "É a minha idade \@ \@{parameters('myAge')}" | Retornar essa cadeia de caracteres, que inclui a expressão: "é a minha idade \@{parameters('myAge')}" | 
||| 

Quando está trabalhando visualmente no Designer de aplicações lógicas, pode criar expressões através do Expression builder, por exemplo: 

![Estruturador de aplicações lógicas > Construtor de expressões](./media/logic-apps-workflow-definition-language/expression-builder.png)

Quando tiver terminado, a expressão será exibido para a propriedade correspondente em sua definição de fluxo de trabalho, por exemplo, o `searchQuery` propriedade aqui:

```json
"Search_tweets": {
  "inputs": {
    "host": {
      "connection": {
       "name": "@parameters('$connections')['twitter']['connectionId']"
      }
    }
  },
  "method": "get",
  "path": "/searchtweets",
  "queries": {
    "maxResults": 20,
    "searchQuery": "Azure @{concat('firstName','', 'LastName')}"
  }
},
```

<a name="operators"></a>

## <a name="operators"></a>Operadores

Na [expressões](#expressions) e [funções](#functions), operadores de executam tarefas específicas, como uma propriedade ou um valor numa matriz de referência. 

| Operador | Tarefa | 
|----------|------|
| ' | Para utilizar um literal de cadeia como entrada ou em expressões e funções, encapsule a cadeia de caracteres apenas com plicas, por exemplo, `'<myString>'`. Não utilize aspas (""), que entram em conflito com a formatação do JSON em torno de uma expressão de toda. Por exemplo: <p>**Sim**: length('Hello') </br>**Não**: length("Hello") <p>Quando transmitir matrizes ou números, não precisa de pontuação de encapsulamento de aplicações. Por exemplo: <p>**Sim**: comprimento ([1, 2, 3]) </br>**Não**: comprimento ("[1, 2, 3]") | 
| [] | Para fazer referência a um valor numa posição específica (índice) numa matriz, utilize parênteses Retos. Por exemplo, para obter o segundo item numa matriz: <p>`myArray[1]` | 
| . | Para fazer referência a uma propriedade num objeto, utilize o operador de ponto. Por exemplo, para obter o `name` propriedade para um `customer` objeto JSON: <p>`"@parameters('customer').name"` | 
| ? | Para fazer referência nulas propriedades num objeto sem um erro de tempo de execução, utilize o operador de ponto de interrogação. Por exemplo, para lidar com nulo saídas a partir de um acionador, pode utilizar esta expressão: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Funções

Algumas expressões de obtém os valores de ações de tempo de execução que ainda poderão não existir quando uma aplicação lógica começa a ser executada. Para referenciar ou trabalhar com esses valores em expressões, pode usar [ *funções*](../logic-apps/workflow-definition-language-functions-reference.md). Por exemplo, pode usar funções matemáticas para cálculos, como o [Add ()](../logic-apps/workflow-definition-language-functions-reference.md#add) função, que devolve a soma de números inteiros ou floats. Para obter informações detalhadas sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).
Em alternativa, continue a aprender sobre as funções e sua finalidade geral.

Seguem-se apenas algumas tarefas de exemplo que pode realizar com as funções: 

| Tarefa | Sintaxe da função | Resultado | 
| ---- | --------------- | ------ | 
| Retorne uma cadeia de caracteres no formato em minúsculas. | toLower ("<*texto*>") <p>Por exemplo: toLower('Hello') | "hello" | 
| Devolve um identificador exclusivo global (GUID). | GUID() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Este exemplo mostra como pode obter o valor do `customerName` parâmetro e atribuir esse valor para o `accountName` propriedade utilizando o [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) função numa expressão:

```json
"accountName": "@parameters('customerName')"
```

Aqui estão algumas outras formas gerais que pode utilizar funções em expressões de:

| Tarefa | Sintaxe da função numa expressão | 
| ---- | -------------------------------- | 
| Realizar o trabalho com um item passando esse item para uma função. | "\@<*functionName*> (<*item*>)" | 
| 1. Obter o *parameterName*do valor utilizando aninhada `parameters()` função. </br>2. Executar o trabalho com o resultado ao passar esse valor para *functionName*. | "\@<*functionName*> (parâmetros (" <*parameterName*> "))" | 
| 1. Obter o resultado da função interna aninhada *functionName*. </br>2. Passar o resultado para a função externa *functionName2*. | "\@<*functionName2*> (<*functionName*> (<*item*>))" | 
| 1. Obter o resultado da *functionName*. </br>2. Uma vez que o resultado é um objeto com a propriedade *propertyName*, obter o valor dessa propriedade. | "\@<*functionName*>(<*item*>). <*propertyName*>" | 
||| 

Por exemplo, o `concat()` função pode assumir dois ou mais valores de cadeia de caracteres como parâmetros. Esta função combina essas cadeias de caracteres numa cadeia de caracteres. Pode passar em literais de cadeia de caracteres, por exemplo, "Sophia" e "Owen", para que obtém uma cadeia de caracteres combinada, "SophiaOwen":

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Em alternativa, pode obter valores de cadeia de caracteres de parâmetros. Este exemplo utiliza a `parameters()` função em cada `concat()` parâmetro e o `firstName` e `lastName` parâmetros. Então, passa as cadeias de caracteres resultantes para o `concat()` funcionar para que obtém uma cadeia de caracteres combinada, por exemplo, "SophiaOwen":

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

De qualquer forma, os dois exemplos atribuir o resultado para o `customerName` propriedade. 

Para obter informações detalhadas sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).
Em alternativa, continue a aprender sobre as funções com base na sua finalidade geral.

<a name="string-functions"></a>

### <a name="string-functions"></a>Funções de cadeia

Para trabalhar com cadeias de caracteres, pode usar essas funções de cadeia de caracteres e também alguns [funções de coleção](#collection-functions). Funções de cadeia só funcionam em cadeias de caracteres. 

| Função de cadeia de caracteres | Tarefa | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Combinar dois ou mais cadeias de caracteres e retornar a cadeia de caracteres combinada. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Verifique se uma cadeia de caracteres termina com a subcadeia especificada. | 
| [guid](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Gere um identificador exclusivo global (GUID) como uma cadeia de caracteres. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Devolve a posição inicial para uma subcadeia. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Devolve a posição final para uma subcadeia. | 
| [replace](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Substituir uma subcadeia com a cadeia especificada e retornar a cadeia de caracteres atualizada. | 
| [dividir](../logic-apps/workflow-definition-language-functions-reference.md#split) | Devolva uma matriz que tem todos os carateres de uma cadeia de caracteres e separa cada caractere com o caráter delimitador específico. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Verificar se uma cadeia de caracteres começa com uma subcadeia específica. | 
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Devolva carateres de uma cadeia de caracteres, a partir da posição especificada. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Retorne uma cadeia de caracteres no formato em minúsculas. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Retorne uma cadeia de caracteres no formato em maiúsculas. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Remover espaços em branco à esquerda e à direita de uma cadeia de caracteres e retornar a cadeia de caracteres atualizada. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Funções de coleção

Para trabalhar com coleções, em geral, matrizes, cadeias de caracteres e, às vezes, dicionários, pode utilizar estas funções de coleção. 

| Função de coleção | Tarefa | 
| ------------------- | ---- | 
| [Contém](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Verifique se uma coleção tem um item específico. |
| [empty](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Verifique se uma coleção está vazia. | 
| [first](../logic-apps/workflow-definition-language-functions-reference.md#first) | Devolva o primeiro item a partir de uma coleção. | 
| [intersection](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Retornar uma coleção que tenha *apenas* os itens comuns em coleções especificadas. | 
| [aderir](../logic-apps/workflow-definition-language-functions-reference.md#join) | Retornar uma cadeia de caracteres que tenha *todos os* os itens de uma matriz, separados pelo caractere especificado. | 
| [last](../logic-apps/workflow-definition-language-functions-reference.md#last) | Devolva o último item a partir de uma coleção. | 
| [Comprimento](../logic-apps/workflow-definition-language-functions-reference.md#length) | Devolva o número de itens numa cadeia ou matriz. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Remover itens da frente de uma coleção e devolver *todas as outras* itens. | 
| [tirar](../logic-apps/workflow-definition-language-functions-reference.md#take) | Itens de retorno da frente de uma coleção. | 
| [União](../logic-apps/workflow-definition-language-functions-reference.md#union) | Retornar uma coleção que tenha *todos os* os itens de coleções especificadas. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Funções de comparação

Para trabalhar com condições, comparar valores e resultados de expressão ou avaliar os vários tipos de lógica, pode utilizar estas funções de comparação. Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de comparação | Tarefa | 
| ------------------- | ---- | 
| [e](../logic-apps/workflow-definition-language-functions-reference.md#and) | Verifique se todas as expressões forem verdadeiras. | 
| [equals](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Verifique se os dois valores são equivalentes. | 
| [greater](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Verifique se o primeiro valor é maior do que o segundo valor. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Verifique se o primeiro valor é maior que ou igual ao segundo valor. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | Verifique se uma expressão é verdadeira ou falsa. Com base no resultado, devolva um valor especificado. | 
| [menos](../logic-apps/workflow-definition-language-functions-reference.md#less) | Verifique se o primeiro valor é menor que o segundo valor. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Verifique se o primeiro valor é menor ou igual ao segundo valor. | 
| [not](../logic-apps/workflow-definition-language-functions-reference.md#not) | Verifique se uma expressão é falsa. | 
| [ou](../logic-apps/workflow-definition-language-functions-reference.md#or) | Verifique se, pelo menos, uma expressão é verdadeira. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Funções de conversão

Para alterar o tipo ou formato de um valor, pode usar essas funções de conversão. Por exemplo, pode alterar um valor de um valor booleano para um número inteiro. Para saber como as aplicações lógicas processam os tipos de conteúdo durante a conversão, veja [lidar com tipos de conteúdo](../logic-apps/logic-apps-content-type.md). Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de conversão | Tarefa | 
| ------------------- | ---- | 
| [array](../logic-apps/workflow-definition-language-functions-reference.md#array) | Devolva uma matriz de um único especificado de entrada. Para várias entradas, consulte [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Devolva a versão codificada em base64 para uma cadeia de caracteres. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Devolva a versão binária para uma cadeia de caracteres codificada em base64. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Devolva a versão de cadeia de caracteres para uma cadeia de caracteres codificada em base64. | 
| [binário](../logic-apps/workflow-definition-language-functions-reference.md#binary) | Devolva a versão binária para um valor de entrada. | 
| [Bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Devolva a versão booleana para um valor de entrada. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Retornar uma matriz de várias entradas. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Devolve o URI de dados para um valor de entrada. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Devolva a versão binária para um URI de dados. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Devolva a versão de cadeia de caracteres para um URI de dados. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Devolva a versão de cadeia de caracteres para uma cadeia de caracteres codificada em base64. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Devolva a versão binária para um URI de dados. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Devolver uma cadeia de caracteres que substitui o escape de caracteres com versões decodificadas. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Retorne uma cadeia de caracteres que substitui os carateres não seguros de URL com carateres de escape. | 
| [float](../logic-apps/workflow-definition-language-functions-reference.md#float) | Devolver a flutuante ponto número para um valor de entrada. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Devolva a versão de número inteiro para uma cadeia de caracteres. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | Devolve o valor de tipo JavaScript Object Notation (JSON) ou o objeto para uma cadeia ou XML. | 
| [string](../logic-apps/workflow-definition-language-functions-reference.md#string) | Devolva a versão de cadeia de caracteres para um valor de entrada. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Devolve a versão codificada de URI para um valor de entrada ao substituir carateres não seguros de URL com carateres de escape. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Devolva a versão binária para uma cadeia codificada de URI. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Devolva a versão de cadeia de caracteres para uma cadeia codificada de URI. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Devolva a versão XML para uma cadeia de caracteres. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Funções matemáticas

Para trabalhar com números inteiros e floats, pode usar essas funções matemáticas. Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de matemática | Tarefa | 
| ------------- | ---- | 
| [add](../logic-apps/workflow-definition-language-functions-reference.md#add) | Devolva o resultado da adição de dois números. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Retornar o resultado da divisão de dois números. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Devolva o valor mais alto de um conjunto de números ou uma matriz. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Devolva o valor mais baixo de um conjunto de números ou uma matriz. | 
| [MOD](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Devolva o resto da divisão de dois números. | 
| [MUL](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Devolva o produto da multiplicação de dois números. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Devolva um número inteiro aleatório de um intervalo especificado. | 
| [range](../logic-apps/workflow-definition-language-functions-reference.md#range) | Devolva uma matriz de inteiros que começa a partir de um número inteiro especificado. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Devolva o resultado da subtração o segundo número do primeiro número. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Funções de data e hora

Para trabalhar com datas e horas, pode usar essas funções de data e hora.
Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de data ou hora | Tarefa | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Adicione um número de dias a um carimbo. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Adicione um número de horas para um carimbo. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Adicione um número de minutos para um carimbo. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Adicione um número de segundos a um carimbo. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Adiciona um número de unidades de tempo para um carimbo. Consulte também [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Converta um carimbo de Universal tempo coordenada (UTC) para o fuso horário de destino. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Converta um carimbo de fuso horário de origem para o fuso horário de destino. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Converta um carimbo de fuso horário de origem para Universal tempo coordenada (UTC). | 
| [dayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Devolva o dia do componente de mês a partir de um carimbo. | 
| [dayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Devolva o dia do componente da semana de um carimbo. | 
| [dayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Devolva o dia do componente de ano a partir de um carimbo. | 
| [formatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Devolva a data de um carimbo. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Devolva o timestamp atual, bem como as unidades de tempo especificado. Consulte também [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Devolva o carimbo de hora atual menos as unidades de tempo especificado. Consulte também [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Devolva o início do dia para um carimbo. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Devolva o início da hora para um carimbo. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Devolva o início do mês para um carimbo. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Subtrai um número de unidades de tempo de um carimbo. Consulte também [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [ticks](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Devolver o `ticks` valor da propriedade para um carimbo especificado. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Devolva o timestamp atual como uma cadeia de caracteres. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>Funções de fluxo de trabalho

Estas funções de fluxo de trabalho podem ajudá-lo a:

* Obtenha detalhes sobre uma instância de fluxo de trabalho em tempo de execução. 
* Trabalhar com as entradas utilizadas para criar uma instância de aplicações lógicas.
* Referenciam as saídas de acionadores e ações.

Por exemplo, pode referenciar as saídas da ação de um e utilizar esses dados numa ação posterior. Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de fluxo de trabalho | Tarefa | 
| ----------------- | ---- | 
| [ação](../logic-apps/workflow-definition-language-functions-reference.md#action) | Devolva a saída da ação atual em tempo de execução ou valores de outros pares de nome e valor JSON. Consulte também [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Devolver uma ação `body` de saída em tempo de execução. Consulte também [corpo](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Devolva o resultado de uma ação em tempo de execução. Ver [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Devolva o resultado de uma ação em tempo de execução ou valores de outros pares de nome e valor JSON. Consulte também [ação](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Corpo](#body) | Devolver uma ação `body` de saída em tempo de execução. Consulte também [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Criar uma matriz com os valores que correspondem ao nome da chave no *dados de formulário* ou *formulário codificado* saídas da ação. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Devolver um valor único que corresponde ao nome da chave numa ação *dados de formulário* ou *saída de formulário codificado*. | 
| [Item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Quando dentro de uma ação de repetição numa matriz, retorna o item atual na matriz durante a iteração da ação de atual. | 
| [itens](../logic-apps/workflow-definition-language-functions-reference.md#items) | Quando dentro de um para cada ou fazer até o loop, retorno o item atual do ciclo especificado.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Devolva o "URL de retorno de chamada" que chama um acionador ou ação. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Devolve o corpo de uma parte específica na saída de uma ação que tem várias partes. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Devolva o valor para um parâmetro que é descrito na sua definição da aplicação lógica. | 
| [Acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Devolva o resultado de um acionador no tempo de execução ou a partir de outros pares de nome e valor JSON. Consulte também [triggerOutputs](#triggerOutputs) e [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Devolver um acionador `body` de saída em tempo de execução. Ver [acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Devolver um valor único que corresponda um nome de chave na *dados de formulário* ou *formulário codificado* acionar saídas. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Devolve o corpo de uma parte específica na saída de várias partes de um acionador. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Criar uma matriz cujos valores corresponde ao nome da chave no *dados de formulário* ou *formulário codificado* acionar saídas. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Devolva o resultado de um acionador no tempo de execução ou valores de outros pares de nome e valor JSON. Ver [acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [Variáveis](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Devolva o valor para uma variável especificada. | 
| [Fluxo de trabalho](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Devolva todos os detalhes sobre o próprio fluxo de trabalho durante o tempo de execução. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>Funções de análise do URI

Para trabalhar com os identificadores de recurso uniforme (URIs) e obter vários valores de propriedade para estes URIs, pode utilizar estas funções de análise do URI. Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de análise do URI | Tarefa | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Devolver o `host` valor para um identificador de recurso uniforme (URI). | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Devolver o `path` valor para um identificador de recurso uniforme (URI). | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Devolver o `path` e `query` valores para um identificador de recurso uniforme (URI). | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Devolver o `port` valor para um identificador de recurso uniforme (URI). | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Devolver o `query` valor para um identificador de recurso uniforme (URI). | 
| [uriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Devolver o `scheme` valor para um identificador de recurso uniforme (URI). | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>Funções JSON e XML

Para trabalhar com objetos JSON e nós XML, pode usar essas funções de manipulação. Para obter a referência completa sobre cada função, consulte a [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de manipulação | Tarefa | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Adicionar uma propriedade e seu valor ou o par nome / valor, a um objeto JSON e devolver do objeto atualizado. | 
| [Coalesce](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Devolva o primeiro valor não nulo de um ou mais parâmetros. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Remover uma propriedade de um objeto JSON e retornar do objeto atualizado. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Defina o valor de propriedade de um objeto JSON e retornar do objeto atualizado. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Verificar o XML de nós ou valores que correspondem a uma expressão XPath (linguagem de caminho de XML) e devolver os valores ou nós correspondentes. | 
||| 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [ações de linguagem de definição de fluxo de trabalho e acionadores](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Saiba mais sobre programaticamente criar e gerir aplicações lógicas com o [API de REST de fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)
