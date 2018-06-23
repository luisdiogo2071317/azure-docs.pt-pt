---
title: Método de pesquisa na API de conhecimento académico da Graph | Microsoft Docs
description: Utilize o método de pesquisa de gráfico a API de conhecimento académico para devolver um conjunto de entidades académicas com base nos padrões de gráfico específicos nos serviços cognitivos da Microsoft.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: d811db117c934c0d41fbfea1220d241cc022e4a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351391"
---
# <a name="graph-search-method"></a>Método de pesquisa do gráfico

O **pesquisa gráfico** REST API é utilizada para devolver um conjunto de entidades académicas com base nos padrões de gráfico fornecido.  A resposta é um conjunto de caminhos de gráfico que satisfaçam as restrições de utilizador especificado. Um caminho de gráfico é uma sequência de nós de gráfico e contornos sob a forma de intercalado _v0 ', ' e0, v1, e1,..., vn_, onde _v0_ é o nó inicial do caminho.
<br>

**Ponto final REST:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>Parâmetros do Pedido  
Nome     | Valor | Necessário?  | Descrição
-----------|-----------|---------|--------
**Modo**       | Cadeia de texto | Sim | Nome de modo que pretende utilizar. O valor é *json* ou *lambda*.

O método de pesquisa do gráfico tem de ser chamado através de um pedido POST de HTTP. O pedido post deve incluir o cabeçalho de tipo de conteúdo: **application/json**.

##### <a name="json-search"></a>Pesquisa JSON 

Para o *json* pesquisa, o corpo da mensagem é um objeto JSON. O objeto JSON descreve um padrão de caminho com restrições de utilizador especificado (consulte o [especificação do objeto JSON](JSONSearchSyntax.md) para *json* pesquisa).


##### <a name="lambda-search"></a>Pesquisa de lambda

Para o *lambda* pesquisa, o corpo da mensagem é uma cadeia de texto simples. O corpo da mensagem é uma cadeia de consulta de lambda LIKQ, que é uma instrução única c# (consulte o [especificação de cadeia de consulta](LambdaSearchSyntax.md) para *lambda* pesquisa). 

<br>
## <a name="response-json"></a>Resposta (JSON)
Nome | Descrição
-------|-----   
**resultados** | Uma matriz de 0 ou mais entidades que corresponde à expressão de consulta. Cada entidade contém os valores dos atributos do pedido. Este campo está presente, se o pedido foi processado com êxito.
**error** | Códigos de estado HTTP. Este campo está presente se falhar o pedido.
**message** | Mensagem de erro. Este campo está presente se falhar o pedido.

Se uma consulta não pode ser processada no _800 ms_, um _tempo limite_ vai ser devolvido o erro. 

<br>
#### <a name="example"></a>Exemplo:

##### <a name="json-search"></a>Pesquisa JSON
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
Para o *json* procurar, se de que pretende obter papers cujos títulos contêm "graph motor" e escrito por "bin shao", pode especificar a consulta da seguinte forma.

```JSON
{
  "path": "/paper/AuthorIDs/author",
  "paper": {
    "type": "Paper",
    "NormalizedTitle": "graph engine",
    "select": [
      "OriginalTitle"
    ]
  },
  "author": {
    "return": {
      "type": "Author",
      "Name": "bin shao"
    }
  }
}
```

O resultado de uma consulta é uma matriz de caminhos de gráfico. Um caminho de gráfico é uma matriz de objetos do nó correspondentes a nós especificado no caminho de consulta. Estes objetos do nó tem pelo menos uma propriedade *CellID*, que representa o ID de entidade. Outras propriedades podem ser obtidas especificando os nomes de propriedade através o operador de selecionar um [ *objecto de acção transversal*](JSONSearchSyntax.md).

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668,
        "OriginalTitle": "Trinity: a distributed graph engine on a memory cloud"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2171539317,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2411554868,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 73304046,
        "OriginalTitle": "The Trinity graph engine"
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
 ```

##### <a name="lambda-search"></a>Pesquisa de lambda 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
Para o *lambda* pesquisa, se quisermos obter o autor IDs de um determinado documento, mas pode escrever uma consulta como a seguinte.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

O resultado de uma *lambda* consulta de pesquisa também é uma matriz de caminhos do gráfico:

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2142490828
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2116756368
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
```
 
## <a name="graph-schema"></a>Esquema de gráfico

Esquema de gráfico é útil para escrever consultas de pesquisa do gráfico. Ele é apresentado na figura seguinte.

![Esquema de gráfico académicas da Microsoft](./Images/AcademicGraphSchema.png)
