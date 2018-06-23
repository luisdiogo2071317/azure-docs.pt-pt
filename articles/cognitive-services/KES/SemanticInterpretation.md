---
title: Interpretação semântica na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba como utilizar interpretação semântica no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 022188464eb7269b69f96a058b444167b587387c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351811"
---
# <a name="semantic-interpretation"></a>Interpretação semântica
Interpretação semântica associa saída semântica cada caminho interpretado através de gramática.  Em particular, o serviço avalia a sequência de instruções de `tag` elementos atravessadas pela interpretação para calcular o resultado final.  

Uma instrução pode ser uma atribuição de um literal ou uma variável a outra variável.  -Também pode atribuir a saída de uma função com 0 ou mais parâmetros a uma variável.  Cada parâmetro de função pode ser especificado utilizando um literal ou uma variável.  Se a função não devolve quaisquer dados, a atribuição for omitida.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Uma variável for especificada com um identificador de nome que começa com uma letra e apenas composto por letras (A-Z), números (0-9) e o caráter de sublinhado (\_).  O tipo está implicitamente inferido a partir do literal ou função saída valor atribuído à mesma. 

Segue-se uma lista de tipos de dados atualmente suportados:

|Tipo|Descrição|Exemplos|
|----|----|----|
|Cadeia|Sequência de 0 ou mais carateres|"Olá mundo!"<br/>""|
|Bool|Valor booleano|true<br/>false|
|Int32|número de inteiro com sinal de 32 bits.  -2.1e9 para 2.1e9|123<br/>-321|
|Int64|número de inteiro com sinal de 64 bits. -9.2e18 e 9.2e18|9876543210|
|Valor de duplo|Vírgula flutuante de dupla precisão. 1.7E + /-308 (15 dígitos)|123.456789<br/>1.23456789e2|
|GUID|Identificador exclusivo global|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Consulta|Expressão de consulta que especifica um subconjunto de objetos de dados no índice|ALL()<br/>E (*q1*, *q2*)|

<a name="semantic-functions"></a>
## <a name="semantic-functions"></a>Funções semânticos
Não há um conjunto de funções semânticos incorporado.  Permitir a construção de consultas sofisticadas e permitem controlar context confidencial interpretações de gramática.

### <a name="and-function"></a>As funções e
`query = And(query1, query2);`

Devolve uma consulta composta pela intersecção de dois consultas de entrada.

### <a name="or-function"></a>Ou uma função
`query = Or(query1, query2);`

Devolve uma consulta composta pela União das duas consultas de entrada.

### <a name="all-function"></a>Função all
`query = All();`

Devolve uma consulta que inclui todos os objetos de dados.

No exemplo seguinte, utilizamos a função de All() iteratively criar cópias de segurança uma consulta com base na intersecção do 1 ou mais palavras-chave.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>Nenhuma função
`query = None();`

Devolve uma consulta que inclui não existem objetos de dados.

No exemplo seguinte, utilizamos a função de None() iteratively criar cópias de segurança uma consulta com base na União de palavras-chave 1 ou mais.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Função de consulta
```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Devolve uma consulta que inclui apenas os objetos de dados cujo atributo *attrName* corresponde ao valor *valor* , de acordo com a operação especificada *op*, que está predefinida para "eq".  Normalmente, utiliza um `attrref` elemento para criar uma consulta com base na cadeia de consulta de entrada correspondida.  Se um valor é fornecido ou obtido através de outros meios, a função de Query () pode ser utilizada para criar uma consulta correspondente este valor.

No exemplo seguinte, utilizamos a função de Query () para implementar o suporte para especificar as publicações académicas de um determinado decade.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

<a name="composite-function"/>
### <a name="composite-function"></a>Função composta
`query = Composite(innerQuery);`

Devolve uma consulta que encapsula uma *innerQuery* composto correspondências contra secundárias atributos de um atributo composto comuns *attr*.  O encapsulamento requer o atributo composto *attr* de qualquer objeto de dados correspondente para ter, pelo menos, um valor que satisfaça individualmente a *innerQuery*.  Tenha em atenção que uma consulta em secundárias atributos de um atributo composto tem de ser encapsulado utilizando a função de Composite() antes de pode ser combinado com outras consultas.

Por exemplo, a seguinte consulta devolve publicações académicas por "harry shum" durante a ele na "microsoft":
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

Por outro lado, a seguinte consulta devolve académicas publicações em que é um dos autores "harry shum" e uma das políticas das é "microsoft":
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>Função GetVariable
`value = GetVariable(name, scope);`

Devolve o valor da variável *nome* definido em especificado *âmbito*.  *nome* é um identificador que começa com uma letra e apenas composto por letras (A-Z), números (0-9) e caráter de sublinhado (_).  *âmbito* pode ser definido como "pedido" ou "sistema".  Tenha em atenção que as variáveis definidas em diferentes âmbitos são diferentes entre si, incluindo aqueles definido através de saída de funções de semânticos.

As variáveis de âmbito do pedido são partilhadas entre todos os interpretações dentro do pedido de interpret atual.  Podem ser utilizadas para controlar a pesquisa para interpretações através de gramática.

Variáveis de sistema estão predefinidas pelo serviço e podem ser utilizadas para obter diversas estatísticas sobre o estado atual do sistema.  Segue-se o conjunto de variáveis atualmente suportadas do sistema:

|Nome|Tipo|Descrição|
|----|----|----|
|IsAtEndOfQuery|Bool|VERDADEIRO se a interpretação atual encontrou correspondências todo o texto da consulta de entrada|
|IsBeyondEndOfQuery|Bool|VERDADEIRO se a interpretação atual tem sugerida conclusões para além do texto da consulta de entrada|

### <a name="setvariable-function"></a>Função SetVariable
`SetVariable(name, value, scope);`

Atribui *valor* à variável *nome* especificado em *âmbito*.  *nome* é um identificador que começa com uma letra e apenas composto por letras (A-Z), números (0-9) e caráter de sublinhado (_).  Atualmente, o único valor válido para *âmbito* é "Pedir".  Não existem nenhum variáveis do sistema pode ser definida.

As variáveis de âmbito do pedido são partilhadas entre todos os interpretações dentro do pedido de interpret atual.  Podem ser utilizadas para controlar a pesquisa para interpretações através de gramática.

### <a name="assertequals-function"></a>Função AssertEquals
`AssertEquals(value1, value2);`

Se *value1* e *value2* são equivalentes, a função tem êxito e não tem efeitos secundários.  Caso contrário, a função falha e rejeita a interpretação.

### <a name="assertnotequals-function"></a>Função AssertNotEquals
`AssertNotEquals(value1, value2);`

Se *value1* e *value2* tem não equivalente, a função tem êxito e não tem efeitos secundários.  Caso contrário, a função falha e rejeita a interpretação.


