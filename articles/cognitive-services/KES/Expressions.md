---
title: Estruturados expressões de consulta na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Saiba como utilizar expressões de consulta estruturada no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 070ee311a1153bc9fb59870dce68f385a43b15f1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351710"
---
# <a name="structured-query-expression"></a>Expressão de consulta estruturada
Uma expressão de consulta estruturada Especifica um conjunto de operações a avaliar contra o índice de dados.  É constituída por expressões de consulta do atributo e funções de nível mais elevadas.  Utilize o [ *avaliar* ](evaluateMethod.md) método para calcular os objetos que correspondam a expressão.  Segue-se um exemplo do domínio académico publicações que devolve publicações criadas por Jaime Teevan desde o ano 2013.

`And(Composite(Author.Name=='jaime teevan'),Y>=2013)`

As expressões de consulta estruturada podem ser obtidas a partir do [ *interpretar* ](interpretMethod.md) pedidos, em que o resultado de cada interpretação semântico é uma expressão de consulta estruturada que devolve os objetos de índice correspondente a consulta de linguagem natural a entrada.  Em alternativa, estes podem ser manualmente criadas utilizando a sintaxe descrita nesta secção.

## <a name="attribute-query-expression"></a>Expressão de consulta de atributo
Uma expressão de consulta do atributo identifica um conjunto de objetos com base na correspondência contra um atributo específico.  São suportadas operações correspondentes diferentes dependendo do tipo de atributo e operação indexada especificado no [esquema](SchemaFormat.md):

| Tipo | Operação | Exemplos |
|------|-------------|------------|
| Cadeia | é igual a | Título = "semântica latentes Analysis Services" (canónico + sinónimos) |
| Cadeia | é igual a | Dos Author.Name=='susan t dumais (canónico apenas)|
| Cadeia | starts_with | Título = 's latentes'... |
| Int64/Int32/duplo | é igual a | Ano = 2000 |
| Int64/Int32/duplo | starts_with | Ano = '20'... (qualquer valor decimal começados por "20") |
| Int64/Int32/duplo | is_between | Ano&lt;2000 <br/> Ano&lt;= 2000 <br/> Ano&gt;2000 <br/> Ano&gt;= 2000 <br/> Year=[2010,2012) *(inclui o valor do limite esquerdo apenas: 2010, 2011)* <br/> Ano = [2000,2012] *(inclui os dois valores de limite: 2010, 2011, 2012)* |
| Date | é igual a | Data de nascimento ='1984-05-14' |
| Date | is_between | Data de nascimento&lt;=' 2008/03/14' <br/> PublishDate = ['2000-01-01', ' 2009-12-31'] |
| GUID | é igual a | ID = '602DD052-CC47-4B23-A16A-26B52D30C05B' |


Por exemplo, a expressão "Title ="latentes s"..." corresponde a todas as publicações académicas cujo título começa com a cadeia "latentes s".  Para poder avaliar esta expressão, o título do atributo tem de especificar a operação de "starts_with" no esquema utilizado para criar o índice.

Para atributos com sinónimos associados, uma expressão de consulta pode especificar objetos cujo valor canónico corresponde a uma determinada cadeia utilizando o operador "= =" ou objetos em qualquer um dos seus valores canónicos/sinónimo corresponde ao utilizar o operador "=".  Requerem ambos o operador de "é igual a" especificado na definição do atributo.


## <a name="functions"></a>Funções
Não há um conjunto de funções, permitindo a construção de expressões de consulta mais sofisticadas de consultas de atributo básico incorporado.

### <a name="and-function"></a>As funções e
`And(expr1, expr2)`

Devolve a intersecção de duas expressões de consulta de entrada.

O exemplo seguinte devolve académicas publicações publicadas no ano 2000 sobre a obtenção de informações:

`And(Year=2000, Keyword=='information retrieval')`

### <a name="or-function"></a>Ou uma função
`Or(expr1, expr2)`

Devolve a União das duas expressões de consulta de entrada.

O exemplo seguinte devolve académicas publicações publicadas no ano 2000 sobre a obtenção de informações ou modelação de utilizador:

`And(Year=2000, Or(Keyword='information retrieval', Keyword='user modeling'))`

### <a name="composite-function"></a>Função composta
`Composite(expr)`

Devolve uma expressão que encapsula uma expressão interna é composto por consultas em relação sub atributos de um atributo composto comuns.  O encapsulamento requer o atributo composto de qualquer objeto de dados correspondente para ter, pelo menos, um valor que satisfaça individualmente a expressão interna.  Tenha em atenção que uma expressão de consulta em atributos secundárias de um atributo composto tem de ser encapsulado utilizando a função de Composite() antes de pode ser combinado com outras expressões de consulta.

Por exemplo, a seguinte expressão devolve publicações académicas por "harry shum" durante a ele na "microsoft":

```
Composite(And(Author.Name="harry shum", 
              Author.Affiliation="microsoft"))
```

Por outro lado, a seguinte expressão devolve académicas publicações em que é um dos autores "harry shum" e uma das políticas das é "microsoft":

```
And(Composite(Author.Name="harry shum"), 
    Composite(Author.Affiliation="microsoft"))
```
