---
title: Sintaxe de pesquisa JSON na API de conhecimento académico | Microsoft Docs
description: Saiba mais sobre a sintaxe de pesquisa JSON, que pode utilizar a API de conhecimento académico nos serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a4b9cf535dae60258d71c43bba6f9eec1444bd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351367"
---
# <a name="json-search-syntax"></a>Sintaxe de pesquisa JSON

```javascript
/* Query Object:
   Suppose we have a query path /v0/e0/v1/e1/...
   A query object contains constraints to be applied to graph nodes in a path.
   Constraints are given in <"node identifier", {constraint object}> key-value pairs: 
*/
{
    /* query path can also be set in the query object */
    path: "/v0/e0/v1/e1/.../vn/",
    v0: { /* A Constraint Object */ },
    v1: { /* A Constraint Object */ },
}
```

Os nomes de nó num caminho de consulta (_v0, v1,..._ ) servir como identificadores de nó que podem ser referenciados no objeto da consulta; os nomes de periferia (_e0, e1,..._ ) no caminho representam os tipos de margens correspondentes. Podemos utilizar um asterisco _*_ como um nome de nó ou contorno (exceto para o nó inicial, que deve ser dada) para declarar que existem não existem restrições nesse elemento. Por exemplo, um caminho de consulta `/v0/*/v1/e1/*/` obtém os caminhos do gráfico sem restringir o tipo de limite _(v0, v1)_. Entretanto, a consulta não tem restrições no destino (o último nó) do caminho está.

Quando um caminho contém apenas um nó, diga _v0_, a consulta devolverá simplesmente todas as entidades que satisfaçam as restrições. Um objeto de restrição aplicado para o nó inicial é chamado um *iniciar objeto da consulta*, cuja especificação é dado da seguinte forma.

```javascript
/* Starting Query Object:
   Specifies constraints on the starting node
*/
{
    /* "match" operator searches for entities with the specified properties. 
       All properties defined in the graph schema be queried such as "Name" and "NormalizedTitle".
     */
    "match": { 
        "Name" : "some name",
        ...
    },
    /* "id" operator directly specifies the IDs of the starting nodes. 
       When it is present, the "match" operator is ignored. 
     */
    "id": [ id1, id2, id3... ],
    /* "type" operator limits results to a certain type of entities,
       for example, "Author" or "Paper".
     */
    "type": "Author",
    /* "select" operator pulls properties from the database and 
       returns them to the client.
     */
    "select": ["PaperRank", ...]
}
```

Quando um caminho contém mais do que apenas um nó inicial, o processador de consultas irá efetuar uma transversal de gráfico a seguir o padrão do caminho especificado. Quando chega de um nó, as ações de utilizador especificado transversal serão acionadas, ou seja, se parar o nó atual e devolver ou continuar a explorar o gráfico. Não se for especificada nenhuma ação transversal, serão executadas ações de predefinição. Para um nó intermédio, a ação predefinida é continuar a explorar o gráfico. Para o último nó de um caminho, a ação predefinida é parar e devolver. Um objeto de restrição que especifica ações transversal é chamado um *objecto de acção transversal*. É atribuída a sua especificação da seguinte forma:

```javascript
/* Traversal Action Object:
   Specifies graph traversal actions on a node (except for the starting node).
 */
{
    /* Set the continue condition here. */
    continue: { 
        /* simple property exact match */
        "property_key" : "property_value", 
        /* Advanced query operators */
        /* -- Numerical comparisons */
        "property_key_2" : { "gt" /* or simply ">" */ : 123 },
        /* -- Substring query */
        "property_key_3" : { "substring" : "456" },
        /* -- CellID query */
        "id": [ 111, 222, 333... ],
        /* -- Entity type query */
        "type": "cell_type",
        /* -- Property existance query */
        "has" : "property_key_4",
        /* -- Logical operators */
        /* ---- Note that, by default the operators are combined with AND semantics */
        
        /* -- OR operator */
        "or": {
          /* Query operators... */
        },
        /* -- NOT operator */
        "not": {
            /* Query operators... */
        }
    },
    /* Set the return condition here. */
    return: {
        /* Same operators as the continue object */
    },
    /* The selected properties to return. */
    select: ["property_key_1", ...]
}
```

O corpo da mensagem de um *json* consulta deve conter, pelo menos, um *caminho* padrão. Objetos de ação passam são opcionais. Seguem-se dois exemplos.

```JSON
{
  "path": "/series/ConferenceInstanceIDs/conference/FieldOfStudyIDs/field",
  "series": {
    "type": "ConferenceSeries",
    "FullName": "graph",
    "select": [ "FullName", "ShortName" ]
  },
  "conference": {
    "type": "ConferenceInstance",
    "select": [ "FullName" ]
  },
  "field": {
    "type": "FieldOfStudy",
    "select": [ "Name" ],
    "return": { "Name": { "substring" : "World Wide Web" } }
  }
}
```

```JSON
{
  "path": "/author/PaperIDs/paper",
  "author": {
    "type": "Author",
    "select": [ "DisplayAuthorName" ],
    "match": { "Name": "leslie lamport" }
  },
  "paper": {
    "type": "Paper",
    "select": [ "OriginalTitle", "CitationCount" ],
    "return": { "CitationCount": { "gt": 100 } }
  }
}
```

