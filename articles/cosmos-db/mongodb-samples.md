---
title: Utilize a API MongoDB do Azure Cosmos DB para criar uma aplicação node. js
description: Um tutorial que cria uma base de dados online através das APIs do Azure Cosmos DB para MongoDB.
keywords: exemplos do mongodb
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: sample
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 994371f716a2171622c6c534329764bcf7ac585c
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53534774"
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Crie um Azure Cosmos DB: API para a aplicação do MongoDB com node. js
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Node.js para MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Este exemplo mostra como criar um Azure Cosmos DB: API para a aplicação de consola do MongoDB com node. js.

Para utilizar este exemplo, tem de:

* [Criar](create-mongodb-dotnet.md#create-account) um Azure Cosmos DB: API para a conta do MongoDB.
* Obtenha as informações da [cadeia de ligação](connect-mongodb-account.md) do MongoDB.

## <a name="create-the-app"></a>Criar a aplicação

1. Crie um ficheiro *app.js* e copie e cole o código abaixo.

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
    
    **Opcional**: Se estiver a utilizar o **MongoDB node. js 2.2 driver**, substitua o seguinte trecho de código:

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
    
    Deve ser substituído por:

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
    
2. Modifique as seguintes variáveis no ficheiro *app.js* conforme as definições da sua conta (Saiba como encontrar a sua [cadeia de ligação](connect-mongodb-account.md)):

    > [!IMPORTANT]
    > O **controlador do MongoDB Node.js 3.0** precisa de carateres especiais de codificação na palavra-passe do Cosmos DB. Certifique-se de que codifica os carateres "=" como %3D
    >
    > Exemplo: A palavra-passe *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv = =* codifica para *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv % 3D % 3D*
    >
    > O **controlador do MongoDB Node.js 2.2** não precisa de carateres especiais de codificação na palavra-passe do Cosmos DB.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Abra o seu terminal favorito, execute **npm install mongodb --save** e, em seguida, execute a aplicação com **node app.js**

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [usar o MongoChef](mongodb-mongochef.md) do Azure Cosmos DB: API para a conta do MongoDB.
