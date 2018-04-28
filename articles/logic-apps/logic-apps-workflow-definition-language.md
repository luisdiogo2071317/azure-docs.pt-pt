---
title: Esquema de linguagem de definição de fluxo de trabalho - Azure Logic Apps | Microsoft Docs
description: Escrever as definições de fluxo de trabalho personalizado para o Azure Logic Apps com a linguagem de definição de fluxo de trabalho
services: logic-apps
author: ecfan
manager: SyntaxC4
editor: ''
documentationcenter: ''
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 04/25/2018
ms.author: estfan
ms.openlocfilehash: 7c253fd83bcc1f1dde93ac6ef0c26da1fa1a9a4b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="logic-apps-workflow-definitions-with-the-workflow-definition-language-schema"></a>Definições de fluxo de trabalho de aplicações lógicas com o esquema de linguagem de definição de fluxo de trabalho

Quando criar um fluxo de trabalho de aplicação lógica com [Azure Logic Apps](../logic-apps/logic-apps-overview.md), definição subjacente do seu fluxo de trabalho descreve a lógica real, que é executada para a sua aplicação lógica. Esta descrição segue uma estrutura que tenha definido e validadas pelo esquema de linguagem de definição de fluxo de trabalho, que utiliza [JavaScript Object Notation (JSON)](https://www.json.org/) formato. 
  
## <a name="workflow-definition-structure"></a>Estrutura de definição de fluxo de trabalho

Uma definição de fluxo de trabalho tem pelo menos um acionador que cria uma instância da sua aplicação lógica, além de uma ou mais ações que executa a sua aplicação lógica. 

Segue-se a estrutura de alto nível para uma definição de fluxo de trabalho:  
  
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
| definição | Sim | O elemento inicial para a sua definição de fluxo de trabalho | 
| $schema | Apenas quando externamente que referencia uma definição de fluxo de trabalho | A localização para o ficheiro de esquema JSON que descreve a versão de idioma de definição de fluxo de trabalho, o que pode encontrar aqui: <p>`https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json`</p> |   
| contentVersion | Não | O número de versão para a sua definição de fluxo de trabalho, o que é "1.0.0.0" por predefinição. Para ajudar a identificar e confirme a definição correta quando implementar um fluxo de trabalho, especifique um valor a utilizar. | 
| parâmetros | Não | As definições para um ou mais parâmetros transmitir dados para o seu fluxo de trabalho <p><p>Parâmetros máximos: 50 | 
| Acionadores | Não | As definições para um ou mais acionadores que instanciar o seu fluxo de trabalho. Pode definir mais do que um acionador, mas apenas com a linguagem de definição de fluxo de trabalho, não visualmente através do Designer de aplicações lógicas. <p><p>Acionadores máximas: 10 | 
| ações | Não | As definições para uma ou mais ações executar no tempo de execução do fluxo de trabalho <p><p>Ações máximas: 250 | 
| saídas | Não | As definições para as saídas que devolvem a partir de um fluxo de trabalho a executar <p><p>Saídas máximas: 10 |  
|||| 

## <a name="parameters"></a>Parâmetros

No `parameters` secção, defina os parâmetros que aceitam entradas para o fluxo de trabalho em tempo de execução. Antes de poder utilizar estes parâmetros noutras secções de fluxo de trabalho, certifique-se de que todos os parâmetros nestas secções, declarar.

Segue-se a estrutura geral de uma definição do parâmetro:  

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
| tipo | Sim | int, vírgula flutuante, cadeia, securestring, bool, array, um objeto JSON, secureobject <p><p>**Tenha em atenção**: para todas as palavras-passe, chaves e segredos, utilize o `securestring` e `secureobject` tipos porque o `GET` estes tipos não devolvido pela operação. | O tipo para o parâmetro |
| defaultValue | Não | Igual a `type` | O valor do parâmetro predefinido quando é especificado nenhum valor quando cria uma instância de fluxo de trabalho | 
| allowedValues | Não | Igual a `type` | Uma matriz com valores que pode aceitar o parâmetro |  
| do IdP | Não | Objeto JSON | Quaisquer outros detalhes de parâmetro, por exemplo, o nome ou uma descrição para a sua aplicação lógica ou tempo de criação de dados utilizadas pelo Visual Studio ou outras ferramentas legível |  
||||
  
## <a name="triggers-and-actions"></a>Acionadores e ações  

Na definição de fluxo de trabalho, o `triggers` e `actions` secções definem as chamadas que ocorrem durante a execução do fluxo de trabalho. Para obter mais informações sobre estas secções e a sintaxe, consulte [acionadores de fluxo de trabalho e ações](../logic-apps/logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>Saídas 

No `outputs` secção, definir os dados que o fluxo de trabalho pode devolver quando terminar em execução. Por exemplo, para controlar um estado específico ou um valor de cada ser executada, especificar que a saída de fluxo de trabalho devolve os dados. 

Segue-se a estrutura geral de uma definição de saída: 

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
| <*nome da chave*> | Sim | Cadeia | O nome da chave para a saída do valor de retorno |  
| tipo | Sim | int, vírgula flutuante, cadeia, securestring, bool, array, um objeto JSON | O tipo para o valor de retorno de saída | 
| valor | Sim | Igual a `type` | O valor de retorno de saída |  
||||| 

Para obter o resultado de um fluxo de trabalho a executar, reveja o histórico de execução da aplicação lógica e os detalhes no portal do Azure ou utilize o [API de REST do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows). Também pode passar o resultado para sistemas externos, por exemplo, o Power BI para que pode criar dashboards. 

> [!NOTE]
> Ao responder a pedidos recebidos a partir da API REST do serviço, não utilize `outputs`. Em alternativa, utilize o `Response` tipo de ação. Para obter mais informações, consulte [acionadores de fluxo de trabalho e ações](../logic-apps/logic-apps-workflow-actions-triggers.md).

<a name="expressions"></a>

## <a name="expressions"></a>Expressões

Com JSON, pode ter valores literais que existe no momento da conceção, por exemplo:

```json
"customerName": "Sophia Owen", 
"rainbowColors": ["red", "orange", "yellow", "green", "blue", "indigo", "violet"], 
"rainbowColorsCount": 7 
```

Também pode ter valores que não existem até que o tempo de execução. Para representar estes valores, pode utilizar *expressões*, quais são avaliadas em tempo de execução. Uma expressão é uma sequência que pode conter um ou mais [funções](#functions), [operadores](#operators), variáveis, valores explícitos ou constantes. Na sua definição de fluxo de trabalho, pode utilizar uma expressão em qualquer lugar um valor de cadeia JSON por lhe o prefixo de expressão com no início de sessão (@). Quando a avaliação de uma expressão que representa um valor JSON, o corpo da expressão é extraído removendo a @ caráter e sempre os resultados no outro valor JSON. 

Por exemplo, para definida anteriormente `customerName` propriedade, pode obter o valor da propriedade utilizando o [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) funcionar numa expressão e atribuir esse valor para o `accountName` propriedade:

```json
"customerName": "Sophia Owen", 
"accountName": "@parameters('customerName')"
```

*Cadeia interpolação* também permite utilizar várias expressões no interior de cadeias que são eliminadas pelo @ caráter e chavetas ({}). Segue-se a sintaxe:

```json
@{ "<expression1>", "<expression2>" }
```

O resultado é sempre uma cadeia, tornar esta capacidade semelhante para o `concat()` funcione, por exemplo: 

```json
"customerName": "First name: @{parameters('firstName')} Last name: @{parameters('lastName')}"
```

Se tiver uma cadeia literal, que começa com o caráter, @ prefixo de caráter com outra caráter como um caráter de escape @ @: @ @

Estes exemplos mostram como são avaliadas as expressões:

| Valor JSON | Resultado |
|------------|--------| 
| "Sophia Owen" | Devolver os seguintes carateres: 'Sophia Owen' |
| "matriz [1]" | Devolver os seguintes carateres: 'array [1]' |
| "@@" | Estes carateres ser devolvido como uma cadeia de carateres de um: ' @' |   
| " @" | Estes carateres ser devolvido como uma cadeia de dois carateres: ' @' |
|||

Para obter estes exemplos, suponha que definir "myBirthMonth" igual a "Janeiro" e "myAge" igual ao número 42:  
  
```json
"myBirthMonth": "January",
"myAge": 42
```

Estes exemplos mostram como são avaliadas as expressões seguintes:

| Expressão de JSON | Resultado |
|-----------------|--------| 
| "@parameters('myBirthMonth')" | Devolver esta cadeia: "Janeiro" |  
| "@{parameters('myBirthMonth')}" | Devolver esta cadeia: "Janeiro" |  
| "@parameters('myAge')" | Devolver este número: 42 |  
| "@{parameters('myAge')}" | Devolver este número como uma cadeia de carateres: "42" |  
| "A minha idade é @{parameters('myAge')}" | Devolver esta cadeia: "a minha idade é 42" |  
| "@concat('Está na minha idade', string(parameters('myAge')))" | Devolver esta cadeia: "a minha idade é 42" |  
| "A minha idade é @ @ {parameters('myAge')}" | Esta cadeia, que inclui a expressão de retorno: "a minha idade é @{parameters('myAge')}' | 
||| 

Quando estiver a trabalhar visualmente no Designer de aplicações lógica, pode criar expressões através do construtor de expressão, por exemplo: 

![Designer de aplicações lógicas > builder de expressão](./media/logic-apps-workflow-definition-language/expression-builder.png)

Quando tiver terminado, a expressão é apresentado para a propriedade correspondente na sua definição de fluxo de trabalho, por exemplo, o `searchQuery` propriedade aqui:

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

No [expressões](#expressions) e [funções](#functions), operadores realizam tarefas específicas, tais como uma propriedade ou um valor numa matriz de referência. 

| Operador | Tarefa | 
|----------|------|
| ' | Para utilizar um literal de cadeia como entrada ou em expressões e funções, molde a cadeia apenas com aspas único, por exemplo, `'<myString>'`. Não utilize aspas (""), que estão em conflito com a formatação JSON em torno de uma expressão completa. Por exemplo: <p>**Sim**: length('Hello') </br>**Não**: length("Hello") <p>Quando passar matrizes ou números, não precisa de pontuação de encapsulamento de aplicações. Por exemplo: <p>**Sim**: comprimento ([1, 2, 3]) </br>**Não**: comprimento ("[1, 2, 3]") | 
| [] | Para fazer referência a um valor na posição específica (índice) numa matriz, utilize parênteses Retos. Por exemplo, para obter o segundo item numa matriz: <p>`myArray[1]` | 
| . | Para fazer referência a uma propriedade de um objeto, utilize o operador de ponto. Por exemplo, para obter o `name` propriedade para um `customer` objeto JSON: <p>`"@parameters('customer').name"` | 
| ? | Para efeitos de referência nulas propriedades de um objeto sem um erro de tempo de execução, utilize o operador de ponto de interrogação. Por exemplo, para lidar com saídas nulo a partir de um acionador, pode utilizar esta expressão: <p>`@coalesce(trigger().outputs?.body?.<someProperty>, '<property-default-value>')` | 
||| 

<a name="functions"></a>

## <a name="functions"></a>Funções

Alguns expressões obter os valores de ações de tempo de execução que não possam ainda existir quando uma aplicação lógica começa a ser executada. Referência ou trabalhar com estes valores em expressões, pode utilizar *funções*. Por exemplo, pode utilizar funções de bibliotecas para cálculos, tais como o [Add ()](../logic-apps/workflow-definition-language-functions-reference.md#add) função, que devolve a soma de números inteiros ou floats. 

Seguem-se apenas algumas tarefas de exemplo que pode realizar com as funções: 

| Tarefa | Sintaxe de função | Resultado | 
| ---- | --------------- | -------------- | 
| Devolva uma cadeia no formato em minúsculas. | toLower ('<*texto*>') <p>Por exemplo: toLower('Hello') | "Olá" | 
| Devolve um identificador exclusivo global (GUID). | GUID() |"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" | 
|||| 

Este exemplo mostra como pode obter o valor da `customerName` parâmetro e atribua o valor para o `accountName` propriedade utilizando o [parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) função numa expressão:

```json
"accountName": "@parameters('customerName')"
```

Seguem-se algumas formas gerais que pode utilizar as funções em expressões de:

| Tarefa | Sintaxe de função numa expressão | 
| ---- | -------------------------------- | 
| Realizar o trabalho com um item através da transmissão de que o item a uma função. | "@<*functionName*> (<*item*>)" | 
| 1. Obter o *parameterName*do valor utilizando o aninhada `parameters()` função. </br>2. Realizar o trabalho com o resultado transferindo esse valor a *functionName*. | "@<*functionName*> (parâmetros ('<*parameterName*>'))" | 
| 1. Obter o resultado da função aninhada interna *functionName*. </br>2. O resultado de passar para a função externa *functionName2*. | "@<*functionName2*> (<*functionName*> (<*item*>))" | 
| 1. Obter o resultado do *functionName*. </br>2. Uma vez que o resultado é um objeto com a propriedade *propertyName*, obter esse valor da propriedade. | "@<*functionName*>(<*item*>). <*propertyName*>" | 
||| 

Por exemplo, o `concat()` função pode efetuar dois ou mais valores de cadeia como parâmetros. Esta função combina essas cadeias numa cadeia. Pode passar no literais de cadeia, por exemplo, "Sophia" e "Owen", para que obtenha uma cadeia de combinado de "SophiaOwen":

```json
"customerName": "@concat('Sophia', 'Owen')"
```

Em alternativa, pode obter valores de cadeia de parâmetros. Este exemplo utiliza o `parameters()` função em cada `concat()` parâmetro e o `firstName` e `lastName` parâmetros. Em seguida, transmite as cadeias de resultantes para o `concat()` funcionar para que obtenha uma cadeia combinada, por exemplo, "SophiaOwen":

```json
"customerName": "@concat(parameters('firstName'), parameters('lastName'))"
```

Qualquer forma, os dois exemplos atribua o resultado para o `customerName` propriedade. 

Para obter informações detalhadas sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).
Em alternativa, saber mais sobre as funções com base no respetivo objetivo geral.

<a name="string-functions"></a>

### <a name="string-functions"></a>Funções de cadeia

Para trabalhar com cadeias, pode utilizar estas funções de cadeia bem como algumas [funções de coleção](#collection-functions). Funções de cadeia só funcionam em cadeias. 

| Função de cadeia | Tarefa | 
| --------------- | ---- | 
| [concat](../logic-apps/workflow-definition-language-functions-reference.md#concat) | Combinar dois ou mais cadeias e a cadeia combinada de retorno. | 
| [endsWith](../logic-apps/workflow-definition-language-functions-reference.md#endswith) | Verifique se uma cadeia termina com a subcadeia especificada. | 
| [guid](../logic-apps/workflow-definition-language-functions-reference.md#guid) | Gere um identificador exclusivo global (GUID) como uma cadeia. | 
| [indexOf](../logic-apps/workflow-definition-language-functions-reference.md#indexof) | Devolve a posição inicial para uma subcadeia. | 
| [lastIndexOf](../logic-apps/workflow-definition-language-functions-reference.md#lastindexof) | Devolve a posição final de uma subcadeia. | 
| [replace](../logic-apps/workflow-definition-language-functions-reference.md#replace) | Substituir uma subcadeia com a cadeia especificada e devolve a cadeia atualizada. | 
| [dividir](../logic-apps/workflow-definition-language-functions-reference.md#split) | Devolva uma matriz que tenha todos os carateres de uma cadeia e separa cada caráter pelo caráter delimitador específico. | 
| [startsWith](../logic-apps/workflow-definition-language-functions-reference.md#startswith) | Verificar se uma cadeia começa com uma subcadeia específica. | 
| [substring](../logic-apps/workflow-definition-language-functions-reference.md#substring) | Devolva carateres a partir de uma cadeia, a partir da posição especificada. | 
| [toLower](../logic-apps/workflow-definition-language-functions-reference.md#toLower) | Devolva uma cadeia no formato em minúsculas. | 
| [toUpper](../logic-apps/workflow-definition-language-functions-reference.md#toUpper) | Devolva uma cadeia no formato maiúsculas. | 
| [trim](../logic-apps/workflow-definition-language-functions-reference.md#trim) | Remover espaços em branco à direita e à esquerda de uma cadeia e devolver a cadeia atualizada. | 
||| 

<a name="collection-functions"></a>

### <a name="collection-functions"></a>Funções de coleção

Para trabalhar com coleções, geralmente, matrizes, cadeias e por vezes, dicionários, pode utilizar estas funções de coleção. 

| Função de coleção | Tarefa | 
| ------------------- | ---- | 
| [contém](../logic-apps/workflow-definition-language-functions-reference.md#contains) | Verifique se uma coleção tem um item específico. |
| [empty](../logic-apps/workflow-definition-language-functions-reference.md#empty) | Verifique se uma coleção está vazia. | 
| [first](../logic-apps/workflow-definition-language-functions-reference.md#first) | Devolva o primeiro item de uma coleção. | 
| [intersection](../logic-apps/workflow-definition-language-functions-reference.md#intersection) | Devolver uma coleção que tenha *apenas* os itens comuns entre coleções especificadas. | 
| [aderir](../logic-apps/workflow-definition-language-functions-reference.md#join) | Devolver uma cadeia com *todos os* os itens a partir de uma matriz, separados por caráter especificado. | 
| [last](../logic-apps/workflow-definition-language-functions-reference.md#last) | Devolva o último item de uma coleção. | 
| [comprimento](../logic-apps/workflow-definition-language-functions-reference.md#length) | Devolva o número de itens numa cadeia ou matriz. | 
| [skip](../logic-apps/workflow-definition-language-functions-reference.md#skip) | Remover itens da frente de uma coleção e devolver *todas as outras* itens. | 
| [tirar](../logic-apps/workflow-definition-language-functions-reference.md#take) | Itens de retorno do início de uma coleção. | 
| [União](../logic-apps/workflow-definition-language-functions-reference.md#union) | Devolver uma coleção que tenha *todos os* os itens às coleções especificadas. | 
||| 

<a name="comparison-functions"></a>

### <a name="comparison-functions"></a>Funções de comparação

Para trabalhar com condições, compare os valores e resultados de expressão ou avaliar vários tipos de lógica, pode utilizar estas funções de comparação. Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de comparação | Tarefa | 
| ------------------- | ---- | 
| [E](../logic-apps/workflow-definition-language-functions-reference.md#and) | Verifique se todas as expressões forem verdadeiras. | 
| [equals](../logic-apps/workflow-definition-language-functions-reference.md#equals) | Verifique se os dois valores são equivalentes. | 
| [greater](../logic-apps/workflow-definition-language-functions-reference.md#greater) | Verifique se o primeiro valor é maior do que o segundo valor. | 
| [greaterOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#greaterOrEquals) | Verifique se o primeiro valor é maior que ou igual ao valor segundo. | 
| [if](../logic-apps/workflow-definition-language-functions-reference.md#if) | Certifique-se uma expressão true ou false. Com base no resultado, devolva um valor especificado. | 
| [menor](../logic-apps/workflow-definition-language-functions-reference.md#less) | Verifique se o primeiro valor é menor que o segundo valor. | 
| [lessOrEquals](../logic-apps/workflow-definition-language-functions-reference.md#lessOrEquals) | Verifique se o primeiro valor é menor ou igual ao valor segundo. | 
| [not](../logic-apps/workflow-definition-language-functions-reference.md#not) | Verifique se uma expressão é falsa. | 
| [ou](../logic-apps/workflow-definition-language-functions-reference.md#or) | Verifique se, pelo menos, uma expressão for verdadeira. |
||| 

<a name="conversion-functions"></a>

### <a name="conversion-functions"></a>Funções de conversão

Para alterar o tipo ou formato de um valor, pode utilizar estas funções de conversão. Por exemplo, pode alterar um valor de um booleano para um número inteiro. Para saber como Logic Apps processa os tipos de conteúdo durante a conversão, consulte [processar tipos de conteúdo](../logic-apps/logic-apps-content-type.md). Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de conversão | Tarefa | 
| ------------------- | ---- | 
| [array](../logic-apps/workflow-definition-language-functions-reference.md#array) | Devolva uma matriz de um único especificado de entrada. Várias entradas, consulte [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray). | 
| [base64](../logic-apps/workflow-definition-language-functions-reference.md#base64) | Devolva a versão para uma cadeia com codificação base64. | 
| [base64ToBinary](../logic-apps/workflow-definition-language-functions-reference.md#base64ToBinary) | Devolva a versão binária para uma cadeia com codificação base64. | 
| [base64ToString](../logic-apps/workflow-definition-language-functions-reference.md#base64ToString) | Devolva a versão de cadeia para uma cadeia com codificação base64. | 
| [Binário](../logic-apps/workflow-definition-language-functions-reference.md#binary) | Devolva a versão binária para um valor de entrada. | 
| [bool](../logic-apps/workflow-definition-language-functions-reference.md#bool) | Devolva a versão booleana para um valor de entrada. | 
| [createArray](../logic-apps/workflow-definition-language-functions-reference.md#createArray) | Devolva uma matriz de várias entradas. | 
| [dataUri](../logic-apps/workflow-definition-language-functions-reference.md#dataUri) | Devolva o URI de dados para um valor de entrada. | 
| [dataUriToBinary](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToBinary) | Devolva a versão binária para um URI de dados. | 
| [dataUriToString](../logic-apps/workflow-definition-language-functions-reference.md#dataUriToString) | Devolva a versão de cadeia para um URI de dados. | 
| [decodeBase64](../logic-apps/workflow-definition-language-functions-reference.md#decodeBase64) | Devolva a versão de cadeia para uma cadeia com codificação base64. | 
| [decodeDataUri](../logic-apps/workflow-definition-language-functions-reference.md#decodeDataUri) | Devolva a versão binária para um URI de dados. | 
| [decodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#decodeUriComponent) | Devolve uma cadeia que substitui o escape carateres com versões descodificadas. | 
| [encodeUriComponent](../logic-apps/workflow-definition-language-functions-reference.md#encodeUriComponent) | Devolveu uma cadeia que substitui carateres não seguro URL com carateres de escape. | 
| [float](../logic-apps/workflow-definition-language-functions-reference.md#float) | Devolver uma vírgula flutuante número para um valor de entrada. | 
| [int](../logic-apps/workflow-definition-language-functions-reference.md#int) | Devolva a versão de número inteiro de uma cadeia. | 
| [json](../logic-apps/workflow-definition-language-functions-reference.md#json) | Devolva o valor de tipo JavaScript Object Notation (JSON) ou o objeto para uma cadeia ou XML. | 
| [string](../logic-apps/workflow-definition-language-functions-reference.md#string) | Devolva a versão de cadeia para um valor de entrada. | 
| [uriComponent](../logic-apps/workflow-definition-language-functions-reference.md#uriComponent) | Devolva a versão codificado de URI para um valor de entrada, substituindo carateres não seguro URL com carateres de escape. | 
| [uriComponentToBinary](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToBinary) | Devolva a versão binária para uma cadeia com codificação URI. | 
| [uriComponentToString](../logic-apps/workflow-definition-language-functions-reference.md#uriComponentToString) | Devolva a versão de cadeia para uma cadeia com codificação URI. | 
| [xml](../logic-apps/workflow-definition-language-functions-reference.md#xml) | Devolva a versão XML para uma cadeia. | 
||| 

<a name="math-functions"></a>

### <a name="math-functions"></a>Funções de bibliotecas

Para trabalhar com números inteiros e floats, pode utilizar estas funções de bibliotecas. Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de bibliotecas | Tarefa | 
| ------------- | ---- | 
| [add](../logic-apps/workflow-definition-language-functions-reference.md#add) | Devolve o resultado da adição de dois números. | 
| [div](../logic-apps/workflow-definition-language-functions-reference.md#div) | Devolve o resultado da divisão de dois números. | 
| [max](../logic-apps/workflow-definition-language-functions-reference.md#max) | Devolva o valor mais alto de um conjunto de números ou uma matriz. | 
| [min](../logic-apps/workflow-definition-language-functions-reference.md#min) | Devolva o valor mais baixo de um conjunto de números ou uma matriz. | 
| [MOD](../logic-apps/workflow-definition-language-functions-reference.md#mod) | Devolve o resto da divisão de dois números. | 
| [MUL](../logic-apps/workflow-definition-language-functions-reference.md#mul) | Devolva o produto de multiplicando dois números. | 
| [rand](../logic-apps/workflow-definition-language-functions-reference.md#rand) | Devolva um número inteiro aleatório a partir de um intervalo especificado. | 
| [range](../logic-apps/workflow-definition-language-functions-reference.md#range) | Devolva uma matriz de número inteiro que é iniciado a partir de um número inteiro especificado. | 
| [sub](../logic-apps/workflow-definition-language-functions-reference.md#sub) | Devolve o resultado de subtraindo o segundo número do primeiro número. | 
||| 

<a name="date-time-functions"></a>

### <a name="date-and-time-functions"></a>Funções de data e hora

Para trabalhar com as datas e horas, pode utilizar estas funções de data e hora.
Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de data ou hora | Tarefa | 
| --------------------- | ---- | 
| [addDays](../logic-apps/workflow-definition-language-functions-reference.md#addDays) | Adicione um número de dias para um carimbo. | 
| [addHours](../logic-apps/workflow-definition-language-functions-reference.md#addHours) | Adicione um número de horas para um carimbo. | 
| [addMinutes](../logic-apps/workflow-definition-language-functions-reference.md#addMinutes) | Adicione um número de minutos para um carimbo. | 
| [addSeconds](../logic-apps/workflow-definition-language-functions-reference.md#addSeconds) | Adicione um número de segundos para um carimbo. |  
| [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime) | Adicione um número de unidades de tempo para um carimbo. Consulte também [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime). | 
| [convertFromUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertFromUtc) | Converta um carimbo de Universal tempo coordenada (UTC) com o fuso horário de destino. | 
| [convertTimeZone](../logic-apps/workflow-definition-language-functions-reference.md#convertTimeZone) | Converter um carimbo de fuso horário a origem com o fuso horário de destino. | 
| [convertToUtc](../logic-apps/workflow-definition-language-functions-reference.md#convertToUtc) | Converta um carimbo de origem fuso horário Universal tempo coordenada (UTC). | 
| [DayOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#dayOfMonth) | Devolva o dia do componente de mês de um carimbo. | 
| [DayOfWeek](../logic-apps/workflow-definition-language-functions-reference.md#dayOfWeek) | Devolva o dia do componente da semana de um carimbo. | 
| [DayOfYear](../logic-apps/workflow-definition-language-functions-reference.md#dayOfYear) | Devolva o dia do componente de ano a partir de um carimbo. | 
| [FormatDateTime](../logic-apps/workflow-definition-language-functions-reference.md#formatDateTime) | Devolva a data a partir de um carimbo. | 
| [getFutureTime](../logic-apps/workflow-definition-language-functions-reference.md#getFutureTime) | Devolva o timestamp atual e as unidades de tempo especificado. Consulte também [addToTime](../logic-apps/workflow-definition-language-functions-reference.md#addToTime). | 
| [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime) | Devolva o timestamp atual menos as unidades de tempo especificado. Consulte também [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime). | 
| [startOfDay](../logic-apps/workflow-definition-language-functions-reference.md#startOfDay) | Devolva o início do dia para um carimbo. | 
| [startOfHour](../logic-apps/workflow-definition-language-functions-reference.md#startOfHour) | Devolva o início da hora para um carimbo. | 
| [startOfMonth](../logic-apps/workflow-definition-language-functions-reference.md#startOfMonth) | Devolva o início do mês para um carimbo. | 
| [subtractFromTime](../logic-apps/workflow-definition-language-functions-reference.md#subtractFromTime) | Subtrair um número de unidades de tempo de um carimbo. Consulte também [getPastTime](../logic-apps/workflow-definition-language-functions-reference.md#getPastTime). | 
| [batimentos](../logic-apps/workflow-definition-language-functions-reference.md#ticks) | Devolver o `ticks` valor da propriedade para um carimbo especificado. | 
| [utcNow](../logic-apps/workflow-definition-language-functions-reference.md#utcNow) | Devolva o timestamp atual como uma cadeia. | 
||| 

<a name="workflow-functions"></a>

### <a name="workflow-functions"></a>Funções de fluxo de trabalho

Estas funções de fluxo de trabalho podem ajudá-lo:

* Obter detalhes sobre uma instância de fluxo de trabalho em tempo de execução. 
* Trabalhar com as entradas utilizadas para instanciar as logic apps.
* Referencia as saídas de acionadores e ações.

Por exemplo, pode referenciar as saídas de uma ação e utilizar esses dados numa ação posterior. Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de fluxo de trabalho | Tarefa | 
| ----------------- | ---- | 
| [Ação](../logic-apps/workflow-definition-language-functions-reference.md#action) | Devolva o resultado da ação atualmente em tempo de execução ou valores a partir de outros pares de nome e valor JSON. Consulte também [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody) | Devolver uma ação `body` saída no tempo de execução. Consulte também [corpo](../logic-apps/workflow-definition-language-functions-reference.md#body). | 
| [actionOutputs](../logic-apps/workflow-definition-language-functions-reference.md#actionOutputs) | Devolve o resultado de uma ação no tempo de execução. Consulte [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions). | 
| [ações](../logic-apps/workflow-definition-language-functions-reference.md#actions) | Devolva o resultado de uma ação no tempo de execução ou valores a partir de outros pares de nome e valor JSON. Consulte também [ação](../logic-apps/workflow-definition-language-functions-reference.md#action).  | 
| [Corpo](#body) | Devolver uma ação `body` saída no tempo de execução. Consulte também [actionBody](../logic-apps/workflow-definition-language-functions-reference.md#actionBody). | 
| [formDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues) | Criar uma matriz com os valores que correspondem a um nome de chave na *dados do formulário* ou *codificado de formulário* saídas de ação. | 
| [formDataValue](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) | Devolver um valor único que corresponda a um nome de chave uma ação *dados do formulário* ou *codificado de formulário saída*. | 
| [item](../logic-apps/workflow-definition-language-functions-reference.md#item) | Quando dentro de uma ação repetida através de uma matriz, devolva o item atual na matriz durante iteração atual a ação. | 
| [Itens](../logic-apps/workflow-definition-language-functions-reference.md#items) | Quando dentro de uma para cada ou efetue até ciclo, devolva o item atual do ciclo especificado.| 
| [listCallbackUrl](../logic-apps/workflow-definition-language-functions-reference.md#listCallbackUrl) | Devolva o "URL de chamada de retorno" que chama um acionador ou ação. | 
| [multipartBody](../logic-apps/workflow-definition-language-functions-reference.md#multipartBody) | Devolva o corpo de uma parte específica no resultado de uma ação que tem várias partes. | 
| [parameters](../logic-apps/workflow-definition-language-functions-reference.md#parameters) | Devolva o valor para um parâmetro que é descrito a definição da aplicação lógica. | 
| [Acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger) | Devolve o resultado de um acionador em tempo de execução, ou a partir de outros pares de nome e valor JSON. Consulte também [triggerOutputs](#triggerOutputs) e [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody). | 
| [triggerBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) | Devolver um acionador `body` saída no tempo de execução. Consulte [acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [triggerFormDataValue](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue) | Devolver um valor único correspondente um nome de chave na *dados do formulário* ou *codificado de formulário* acionar saídas. | 
| [triggerMultipartBody](../logic-apps/workflow-definition-language-functions-reference.md#triggerMultipartBody) | Devolva o corpo de uma parte específica na saída de várias partes de um acionador. | 
| [triggerFormDataMultiValues](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues) | Criar uma matriz cujos valores corresponderem a um nome de chave na *dados do formulário* ou *codificado de formulário* acionar saídas. | 
| [triggerOutputs](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) | Devolva o resultado de um acionador em tempo de execução ou valores a partir de outros pares de nome e valor JSON. Consulte [acionador](../logic-apps/workflow-definition-language-functions-reference.md#trigger). | 
| [variáveis](../logic-apps/workflow-definition-language-functions-reference.md#variables) | Devolva o valor para uma variável especificada. | 
| [fluxo de trabalho](../logic-apps/workflow-definition-language-functions-reference.md#workflow) | Devolva todos os detalhes sobre o fluxo de trabalho durante o tempo de execução. | 
||| 

<a name="uri-parsing-functions"></a>

### <a name="uri-parsing-functions"></a>Análise de funções URI

Para trabalhar com identificadores de recurso uniforme (URI) e obter vários valores de propriedade para estes URIs, pode utilizar estes URI análise funções. Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Analisar a função URI | Tarefa | 
| -------------------- | ---- | 
| [uriHost](../logic-apps/workflow-definition-language-functions-reference.md#uriHost) | Devolver o `host` valor para um identificador de recurso uniforme (URI). | 
| [uriPath](../logic-apps/workflow-definition-language-functions-reference.md#uriPath) | Devolver o `path` valor para um identificador de recurso uniforme (URI). | 
| [uriPathAndQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriPathAndQuery) | Devolver o `path` e `query` valores para um identificador de recurso uniforme (URI). | 
| [uriPort](../logic-apps/workflow-definition-language-functions-reference.md#uriPort) | Devolver o `port` valor para um identificador de recurso uniforme (URI). | 
| [uriQuery](../logic-apps/workflow-definition-language-functions-reference.md#uriQuery) | Devolver o `query` valor para um identificador de recurso uniforme (URI). | 
| [UriScheme](../logic-apps/workflow-definition-language-functions-reference.md#uriScheme) | Devolver o `scheme` valor para um identificador de recurso uniforme (URI). | 
||| 

<a name="manipulation-functions"></a>

### <a name="json-and-xml-functions"></a>Funções de JSON e XML

Para trabalhar com objetos JSON e nós XML, pode utilizar estas funções de manipulação. Para obter a referência completa sobre cada função, consulte o [artigo de referência alfabética](../logic-apps/workflow-definition-language-functions-reference.md).

| Função de manipulação de | Tarefa | 
| --------------------- | ---- | 
| [addProperty](../logic-apps/workflow-definition-language-functions-reference.md#addProperty) | Adicionar uma propriedade e o valor ou o par nome / valor, para um objeto JSON e retorno do objeto atualizado. | 
| [Unir](../logic-apps/workflow-definition-language-functions-reference.md#coalesce) | Devolva o primeiro valor não nulo a partir de um ou mais parâmetros. | 
| [removeProperty](../logic-apps/workflow-definition-language-functions-reference.md#removeProperty) | Remover uma propriedade de um objeto JSON e retorno do objeto atualizado. | 
| [setProperty](../logic-apps/workflow-definition-language-functions-reference.md#setProperty) | Defina o valor da propriedade de um objeto JSON e devolver o objeto atualizado. | 
| [XPath](../logic-apps/workflow-definition-language-functions-reference.md#xpath) | Verifique o XML para nós ou valores que correspondem a uma expressão de XPath (linguagem XML) e devolver valores ou nós correspondente. | 
||| 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [ações de linguagem de definição de fluxo de trabalho e é acionado](../logic-apps/logic-apps-workflow-actions-triggers.md)
* Saiba mais sobre programaticamente criar e gerir as logic apps com o [API de REST do fluxo de trabalho](https://docs.microsoft.com/rest/api/logic/workflows)
