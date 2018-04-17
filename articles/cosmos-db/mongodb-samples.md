---
title: Utilizar APIs de MongoDB para criar uma aplicação de base de dados do Azure Cosmos | Microsoft Docs
description: Um tutorial que cria uma base de dados online utilizando as APIs de BD do Azure Cosmos para MongoDB.
keywords: Exemplos do mongodb
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: anhoh
ms.openlocfilehash: 81eff479c94af938918e6a221d45184ca1a84aef
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Criar uma base de dados do Azure Cosmos: API para a aplicação do MongoDB utilizando Node.js
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Node.js para MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
>

Este exemplo mostra como criar uma base de dados do Azure Cosmos: API para a aplicação de consola do MongoDB com o Node.js.

Para utilizar este exemplo, tem de:

* [Criar](create-mongodb-dotnet.md#create-account) uma base de dados do Azure Cosmos: API para a conta do MongoDB.
* Obter o MongoDB [cadeia de ligação](connect-mongodb-account.md) informações.

## <a name="create-the-app"></a>Criar a aplicação

1. Criar um *app.js* de ficheiros e copie e cole o código abaixo.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    **Opcional**: Se estiver a utilizar o **MongoDB Node.js 2.2 controlador**, substitua o seguinte fragmento de código:

    Original:

    ```nodejs
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    Deve ser substituído com:

    ```nodejs
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```
    
2. Modificar as seguintes variáveis no *app.js* ficheiro por definições da sua conta (Saiba como localizar o [cadeia de ligação](connect-mongodb-account.md)):

    > [!IMPORTANT]
    > O **MongoDB Node.js 3.0 controlador** requer a codificação de carateres especiais na palavra-passe de BD do Cosmos. Certifique-se codificar carateres '=' como % 3D
    >
    > Exemplo: A palavra-passe *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv = =* codifica para *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv % 3D % 3D*
    >
    > O **MongoDB Node.js 2.2 controlador** não necessita de codificação de carateres especiais na palavra-passe de BD do Cosmos.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Abra o seu terminal favorito, execute **npm instalar mongodb – guardar**, em seguida, executar a aplicação com **app.js de nó**

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [utilizar MongoChef](mongodb-mongochef.md) com a base de dados do Azure Cosmos: API para a conta do MongoDB.
