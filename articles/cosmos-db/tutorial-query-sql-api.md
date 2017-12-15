---
title: Como a consulta com o SQL na base de dados do Azure Cosmos? | Microsoft Docs
description: Saiba como consultar com SQL na base de dados do Azure Cosmos
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: ffef6ec2120a80d907449470efb7b4ab6dca8037
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure Cosmos DB: Como consultar o SQL Server a utilizar?

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

A BD do Cosmos Azure [API do SQL Server](documentdb-introduction.md) suporta consultar documentos utilizando o SQL Server. Este artigo fornece um documento de exemplo e dois exemplos de consultas SQL e os resultados.

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar os dados com o SQL Server

## <a name="sample-document"></a>Documento de exemplo

As consultas de SQL Server neste artigo utilizam o documento de exemplo seguinte.

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
## <a name="where-can-i-run-sql-queries"></a>Onde posso executar consultas do SQL Server?

Pode executar consultas utilizando o Explorador de dados no portal do Azure, através de [REST API e SDKs](sql-api-sdk-dotnet.md)e até mesmo [Query playground](https://www.documentdb.com/sql/demo), que é executada consultas num conjunto de dados de exemplo.

Para obter mais informações sobre as consultas SQL, consulte:
* [Consulta SQL e a sintaxe do SQL Server](sql-api-sql-query.md)

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que tem uma conta de base de dados do Azure Cosmos e a coleção. Não tem qualquer um desses? Concluir o [início rápido de 5 minutos](create-mongodb-nodejs.md) ou [tutorial programador](tutorial-develop-mongodb.md) para criar uma conta e a coleção.

## <a name="example-query-1"></a>Consulta de exemplo 1

Tendo em conta o documento família de exemplo acima, a seguinte consulta SQL devolve os documentos em que o campo id corresponde ao `WakefieldFamily`. Uma vez que é um `SELECT *` instrução, o resultado da consulta é o documento JSON concluído:

**Consulta**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Resultados**

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

## <a name="example-query-2"></a>Consulta de exemplo 2

A seguinte consulta devolve todos os nomes de especificado de elementos subordinados na família cujo id corresponde ao `WakefieldFamily` ordenadas pelo respetivo nível.

**Consulta**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Resultados**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Aprendeu a consulta utilizando o SQL Server  

Agora pode avançar para o próximo tutorial para saber como distribuir dados globalmente.

> [!div class="nextstepaction"]
> [Distribuir dados globalmente](tutorial-global-distribution-sql-api.md)

