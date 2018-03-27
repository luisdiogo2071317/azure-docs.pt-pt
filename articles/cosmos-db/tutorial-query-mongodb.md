---
title: 'Azure Cosmos DB: como fazer consultas utilizando a API do MongoDB? | Microsoft Docs'
description: Saiba como fazer consultas com a API do MongoDB na Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: mimig1
manager: jhubbard
editor: ''
tags: ''
ms.assetid: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 03/16/2018
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 7c51a2a1cace89305b971d5fb0f56c360cbf93cb
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-mongodb-api"></a>Tutorial: fazer consultas na Azure Cosmos DB com a API do MongoDB

A API da [Cosmos Azure DB do MongoDB](mongodb-introduction.md) suporta [consultas de shell do MongoDB](https://docs.mongodb.com/manual/tutorial/query-documents/). 

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar dados com o MongoDB

## <a name="sample-document"></a>Documento de exemplo

As consultas neste artigo utilizam o documento de exemplo seguinte.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a id="examplequery1"></a> Consulta de exemplo 1 

Tendo em conta o documento de família de exemplo acima, a seguinte consulta devolve os documentos em que o campo ID corresponde a `WakefieldFamily`.

**Consulta**
    
    db.families.find({ id: “WakefieldFamily”})

**Resultados**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Consulta de exemplo 2 

A seguinte consulta devolve todos os elementos subordinados na família. 

**Consulta**
    
    db.families.find( { id: “WakefieldFamily” }, { children: true } )

**Resultados**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Consulta de exemplo 3 

A consulta seguinte devolve todas as famílias que estão registadas. 

**Consulta**
    
    db.families.find( { "isRegistered" : true })
**Resultados** nenhum documento vai ser devolvido. 

## <a id="examplequery4"></a>Consulta de exemplo 4

A consulta seguinte devolve todas as famílias que não estão registadas. 

**Consulta**
    
    db.families.find( { "isRegistered" : false })
**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Consulta de exemplo 5

A consulta seguinte devolve todas as famílias que não estão registadas e o estado é NY. 

**Consulta**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Consulta de exemplo 6

A consulta seguinte devolve todas as famílias em que as classificações de elementos subordinados são 8.

**Consulta**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Consulta de exemplo 7

A consulta seguinte devolve todas as famílias em que a matriz de elementos subordinados é 3.

**Consulta**
  
      db.Family.find( {children: { $size:3} } )

**Resultados**

Não são devolvidos resultados, porque não temos mais de 2 elementos subordinados. Esta consulta só será concluída com êxito e devolve o documento completo se o parâmetro for 2.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Aprendeu a fazer consultas utilizando o MongoDB 

Agora pode avançar para o tutorial seguinte para saber como distribuir dados globalmente.

> [!div class="nextstepaction"]
> [Distribuir dados globalmente](tutorial-global-distribution-sql-api.md)

