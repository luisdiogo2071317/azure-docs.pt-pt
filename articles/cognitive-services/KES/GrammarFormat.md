---
title: Formato de gramática na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba mais sobre o formato de gramática no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 27202379b8c36696a380049336229cac040b0108
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351883"
---
# <a name="grammar-format"></a>Formato de gramática
A gramática é um ficheiro XML que especifica o conjunto ponderado de consultas de linguagem natural a que o serviço pode interpretar, bem como a forma como estas consultas de linguagem natural são convertidas em expressões de consulta de semântica.  A sintaxe de gramática baseia-se no [SRGS](http://www.w3.org/TR/speech-grammar/), uma norma de W3C para gramáticas de reconhecimento de voz, com as extensões para suportar a integração de índice de dados e as funções de semânticos.

O seguinte descreve cada um dos elementos diferenças sintáticos que podem ser utilizados numa gramática.  Consulte [neste exemplo](#example) para uma gramática completa que demonstra a utilização destes elementos no contexto.

### <a name="grammar-element"></a>gramática elemento 
O `grammar` elemento é o elemento de nível superior na especificação de gramática XML.  Necessários `root` atributo especifica o nome da regra raiz que define o ponto de partida a gramática.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>importar o elemento
O `import` elemento importa uma definição de esquema a partir de um ficheiro externo para ativar as referências de atributo. O elemento tem de ser um elemento subordinado de nível superior `grammar` elemento e aparecer antes de qualquer `attrref` elementos. Necessários `schema` atributo especifica o nome de um ficheiro de esquema localizado no mesmo diretório do ficheiro XML de gramática. Necessários `name` elemento Especifica o esquema alias que subsequentes `attrref` elementos utilizam quando referenciar atributos definidos neste esquema.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>regra de elemento
O `rule` elemento define uma regra de gramática, uma unidade estrutural que especifica um conjunto de expressões de consulta que o sistema pode interpretar.  O elemento tem de ser um elemento subordinado de nível superior `grammar` elemento.  Necessários `id` atributo especifica o nome da regra, o que é referenciado a partir de `grammar` ou `ruleref` elementos.

A `rule` elemento define um conjunto de expansions legais.  Tokens de texto correspondem diretamente a consulta de entrada.  `item` elementos especifique repete-se e alter probabilidades de interpretação.  `one-of` elementos indicam escolhas alternativas.  `ruleref` elementos de ativar a construção de expansions mais complexas do que aqueles mais simples.  `attrref` elementos permitem correspondências com valores de atributo do índice.  `tag` elementos de especificar a semântica da interpretação e é podem alterar a probabilidade de interpretação.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>exemplo de elemento
O opcional `example` elemento Especifica expressões de exemplo que podem ser aceites pelo que o contém `rule` definição.  Isto pode ser utilizado para documentação e/ou automatizada de teste.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>item de elemento
O `item` elemento agrupa uma sequência de construções de gramática.  Pode ser utilizado para indicar repetições da sequência de expansão ou especificar alternativas em conjunto com o `one-of` elemento.

Quando um `item` elemento não é um subordinado de um `one-of` elemento, pode especificar repetição da sequência de incluídos atribuindo a `repeat` atributo para um valor de contagem.  Um valor de contagem de "*n*" (onde *n* é um número inteiro) indica que a sequência tem de ocorrer exatamente *n* vezes.  Um valor de contagem de "*m*-*n*" permite que a sequência de aparecer entre *m* e *n* exceder o tempo, inclusivamente.  Um valor de contagem de "*m*-" Especifica que a sequência tem de aparecer, pelo menos, *m* vezes.  O opcional `repeat-logprob` atributo pode ser utilizado para alterar a probabilidade de interpretação para cada repetição adicional para além de mínimo.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Quando `item` elementos são apresentados como elementos subordinados de um `one-of` elemento, definem o conjunto de alternativas de expansão.  Esta utilização, a opção do `logprob` atributo Especifica a probabilidade de registo relativo entre as diferentes opções.  Dada uma probabilidade *p* entre 0 e 1, pode ser calculada a probabilidade de registo correspondente como registo (*p*), onde é log() o a função de registo natural.  Se não for especificado, `logprob` por predefinição, 0, o que não altera a probabilidade de interpretação.  Tenha em atenção que a probabilidade de registo é sempre um valor de vírgula flutuante negativo ou 0.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>uma-do elemento
O `one-of` elemento Especifica expansions alternativos entre um dos subordinadas `item` elementos.  Apenas `item` elementos podem aparecer dentro de um `one-of` elemento.  Probabilidades relativas entre as diferentes opções podem ser especificadas através do `logprob` atributo em cada subordinado `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>ruleref elemento
O `ruleref` elemento Especifica expansions válidos através de referências para outro `rule` elemento.  Através da utilização de `ruleref` elementos, as expressões complexas mais podem ser criadas a partir de regras mais simples.  Necessários `uri` atributo indica o nome do elemento referenciado `rule` utilizando a sintaxe "#*rulename*".  Para capturar o resultado da regra referenciado semântico, utilize a opção `name` atributo para especificar o nome de uma variável para o qual é atribuída a saída de semântica.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>attrref elemento
O `attrref` elemento referencia um atributo de índice, permitindo a correspondência com os valores de atributo observado no índice.  Necessários `uri` atributo especifica o nome do esquema de índice e o nome de atributo utilizando a sintaxe "*schemaName*#*attrName*".  Tem de existir um precedente `import` elemento que importa o esquema com o nome *schemaName*.  O nome de atributo é o nome de um atributo definido no esquema correspondente.

Para além de correspondência de intervenção do utilizador, o `attrref` elemento também devolve um objeto da consulta estruturada como saída que seleciona o subconjunto dos objetos no índice correspondência do valor de entrada.  Utilize a opção `name` atributo para especificar o nome da variável onde a saída de objeto de consulta deve ser armazenada.  O objeto da consulta pode ser composto com outros objetos de consulta para formar mais as expressões complexas.  Consulte [interpretação semântica](SemanticInterpretation.md) para obter mais detalhes.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Consulta de conclusão 
Para suportar conclusões de consulta ao interpretar consultas do utilizador parcial, cada atributo referenciado tem de incluir "starts_with" como uma operação na definição do esquema.  Indicado um prefixo de consulta do utilizador, `attrref` irá corresponder a todos os valores no índice que concluir o prefixo e produzir a cada valor completa como uma interpretação separada da gramática.  

Exemplos:
* Correspondência `<attrref uri="academic#Keyword" name="keyword"/>` contra a consulta de prefixo "dados" gera uma interpretação para papers sobre "base de dados", uma interpretação para papers sobre "extração de dados", etc.
* Correspondência `<attrref uri="academic#Year" name="year"/>` contra a consulta o prefixo "200" gera uma interpretação para papers "2000", uma interpretação para papers no "2001", etc.

#### <a name="matching-operations"></a>Correspondência de operações
Para além de correspondência exata, selecione de atributo tipos também o prefixo de suporte e inequality corresponde ao através o opcional `op` atributo.  Se nenhum objeto no índice tem um valor que corresponde a, o caminho de gramática é bloqueado e o serviço não irá gerar qualquer interpretações atravessar através deste caminho de gramática.   O `op` atributo as predefinições para "eq".

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

A tabela seguinte lista suportado `op` valores para cada tipo de atributo.  Utilizar requer que a operação de índice correspondente a ser incluído na definição do atributo de esquema.

| Tipo de Atributo | Valor de OP | Descrição | Operação do índice
|----|----|----|----|
| Cadeia | EQ | Correspondência exata da cadeia | é igual a |
| Cadeia | starts_with | Correspondência de prefixo de cadeia | starts_with |
| Int32, Int64, duplo | EQ |  Correspondência de igualdade numérico | é igual a |
| Int32, Int64, duplo | lt, le, gt, ge | Correspondência de inequality numérica (<, < =, >, > =) | is_between |
| Int32, Int64, duplo | starts_with | Correspondência de prefixo do nome de valor na notação decimal | starts_with |

Exemplos:
* `<attrref uri="academic#Year" op="lt" name="year"/>` corresponde à cadeia de entrada "2000" e devolve todos os papers publicados antes do ano 2000, exclusivamente.
* `<attrref uri="academic#Year" op="lt" name="year"/>` não corresponde à cadeia de entrada "20" porque não existem nenhum papers no índice publicado antes do ano 20.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` corresponde à cadeia de entrada "dados" e devolve um papers interpretação único sobre "base de dados", "extração de dados", etc.  Este é um caso de utilização raro.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` corresponde a cadeia de entrada "20" e devolve no papers único interpretação publicados no 200 299, 2000-2999, etc.  Este é um caso de utilização raro.

### <a name="tag-element"></a>etiqueta de elemento
O `tag` elemento Especifica como um caminho através de gramática devem ser interpretadas.  Contém uma sequência de instruções terminada por ponto e vírgula.  Uma instrução pode ser uma atribuição de um literal ou uma variável a outra variável.  -Também pode atribuir a saída de uma função com 0 ou mais parâmetros a uma variável.  Cada parâmetro de função pode ser especificado utilizando um literal ou uma variável.  Se a função não devolve quaisquer dados, a atribuição for omitida.  Âmbito da variável é local para a regra que contêm.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Cada `rule` a gramática tem uma variável predefinida com o nome "out", que representa o resultado da regra de semântico.  O valor é calculado através da avaliação de cada uma das instruções semânticos atravessadas pelo caminho através de `rule` corresponder o utilizador a entrada de consulta.  O valor atribuído à variável "out" no final da avaliação é o resultado da regra de semântico.  O resultado semântico do interpretar uma consulta do utilizador em relação a gramática é o resultado da regra raiz semântico.

Algumas instruções podem alterar a probabilidade de um caminho de interpretação, introduzindo um desvio do registo semiaditivas probabilidade.  Algumas instruções poderão rejeitar a interpretação Folders se especificado condições não forem satisfeitas.

Para obter uma lista de funções de semânticos suportadas, consulte [funções semântica](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Probabilidade de interpretação
A probabilidade de um caminho de interpretação através de gramática está a probabilidade acumulada de registo de todos os o `<item>` elementos e funções semânticos encontradas ao longo do processo.  Descreve a probabilidade relativa de correspondência de uma sequência de entrada específica.

Dada uma probabilidade *p* entre 0 e 1, pode ser calculada a probabilidade de registo correspondente como registo (*p*), onde log() é a função de registo natural.  Utilizar probabilidades de registo permite que o sistema acumular a probabilidade de joint de um caminho de interpretação através da simple adição.  Também evita insuficiência de vírgula flutuante comuns para os cálculos de probabilidade joint.  Tenha em atenção que por predefinição, a probabilidade de registo é sempre um valor de vírgula flutuante negativo ou 0, onde os valores maiores indicam probabilidade superior.

<a name="example"></a>
## <a name="example"></a>Exemplo
Segue-se um exemplo XML do domínio académico publicações que demonstra os vários elementos de uma gramática:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="academic.schema" name="academic"/>
  
  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>
    
    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>
  
    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>
        
      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>
        
        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>
        
        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>
  
  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="lt" name="year"/></item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="gt" name="year"/></item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```
