---
title: 'Azure Cosmos DB: Como consultar utilizando a API do MongoDB? | Microsoft Docs'
description: Saiba como consultar com a API do MongoDB para a base de dados do Azure Cosmos
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 1818476a95ddf373701ad93860b02ea4c2ad761d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a>Azure Cosmos DB: Como consultar com a API para o MongoDB?

A BD do Cosmos Azure [API para o MongoDB](mongodb-introduction.md) suporta [consultas de shell do MongoDB](https://docs.mongodb.com/manual/tutorial/query-documents/). 

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar os dados com o MongoDB

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
## <a id="examplequery1"></a>Consulta de exemplo 1 

Tendo em conta o documento família de exemplo acima, a seguinte consulta indica os documentos em que o campo id corresponde `WakefieldFamily`.

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

A seguinte consulta devolve todos os elementos subordinados em cada família. 

**Consulta**
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

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

A consulta seguinte devolve todas as famílias de que estão registadas. 

**Consulta**
    
    db.families.find( { "isRegistered" : true })
**Resultados** nenhum documento vai ser devolvido. 

## <a id="examplequery4"></a>Consulta de exemplo 4

A consulta seguinte devolve todas as famílias de que não estão registadas. 

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

A consulta seguinte devolve todas as famílias de que não estão registadas e estado é NY. 

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

A consulta seguinte devolve todas as famílias de onde grades subordinados são 8.

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

A consulta seguinte devolve todas as famílias de onde o tamanho da matriz de elementos subordinados é 3.

**Consulta**
  
      db.Family.find( {children: { $size:3} } )

**Resultados**

Não serão possível devolver resultados como não existe mais do que 2 elementos subordinados. Apenas quando o parâmetro é 2 esta consulta será concluída com êxito e devolver o documento completo.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Aprendeu a consultar com MongoDB 

Agora pode avançar para o próximo tutorial para saber como distribuir dados globalmente.

> [!div class="nextstepaction"]
> [Distribuir dados globalmente](tutorial-global-distribution-sql-api.md)

