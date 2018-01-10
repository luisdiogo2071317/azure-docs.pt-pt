---
title: Utilizando a estrutura de Mongoose com base de dados do Azure Cosmos | Microsoft Docs
description: "Saiba como ligar uma aplicação Node.js Mongoose BD do Cosmos do Azure"
services: cosmos-db
documentationcenter: 
author: romitgirdhar
manager: jhubbard
editor: 
ms.assetid: de5eea58-ee7c-4609-b1c9-4af3e61a5883
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: rogirdh
ms.openlocfilehash: 9878936b5dd76730633dec16b1c3a3eaac78e95a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Utilizando a estrutura de Mongoose com base de dados do Azure Cosmos

Este tutorial demonstra como utilizar o [Mongoose Framework](http://mongoosejs.com/) ao armazenar dados na base de dados do Azure Cosmos. Podemos utilizar a API do MongoDB para a base de dados do Azure Cosmos nestas instruções. Os dos familiarizado, o Mongoose é uma arquitetura de modelação de objeto para o MongoDB no Node.js e fornece uma solução simples, com base no esquema para modelar os seus dados de aplicação.

O Azure Cosmos DB é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft. Pode criar e consultar rapidamente o documento, a chave/valor e as bases de dados de gráficos, que beneficiam de capacidades de escalamento horizontal e distribuição global no centro do Azure Cosmos DB.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Versão [Node.js](https://nodejs.org/) v0.10.29 ou superior.

## <a name="create-an-azure-cosmos-db-account"></a>Criar uma conta do Azure Cosmos DB

Vamos criar uma conta do Azure Cosmos DB. Se já tiver uma conta que pretende utilizar, pode avançar diretamente para [configurar a sua aplicação Node.js](#SetupNode). Se estiver a utilizar o emulador de BD do Cosmos do Azure, siga os passos indicados em [emulador de BD do Azure Cosmos](local-emulator.md) para configurar o emulador e avançar diretamente para [configurar a sua aplicação Node.js](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Configurar a sua aplicação Node.js

>[!Note]
> Se gostaria de instruções apenas o código de exemplo em vez do programa de configuração da própria aplicação, clone o [exemplo](https://github.com/Azure-Samples/Mongoose_CosmosDB) utilizado para este tutorial e criar a sua aplicação Node.js Mongoose na base de dados do Azure Cosmos.

1. Para criar uma aplicação Node.js na pasta à sua escolha, execute o seguinte comando numa linha de comandos do nó.

    ```npm init```

    Responda às questões e o projeto será pronto para começar.

1. Adicione um novo ficheiro para a pasta e dê-lhe nome ```index.js```.
1. Instalar os pacotes necessários utilizando um do ```npm install``` opções:
    * Mongoose:```npm install mongoose --save```
    * Dotenv (se gostaria de carregar os segredos a partir de um ficheiro de .env):```npm install dotenv --save```
    
    >[!Note]
    > O ```--save``` sinalizador adiciona a dependência para o ficheiro Package. JSON.

1. Importe as dependências no seu ficheiro index.js.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Adicione a cadeia de ligação de base de dados do Cosmos e nome de base de dados do Cosmos para o ```.env``` ficheiro.

    ```JavaScript
    COSMOSDB_CONNSTR={Your MongoDB Connection String Here}
    COSMOSDB_DBNAME={Your DB Name Here}
    ```

1. Ligar à base de dados do Cosmos do Azure utilizando a estrutura de Mongoose adicionando o seguinte código ao fim do index.js.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb"); //Creates a new DB, if it doesn't already exist

    var db = mongoose.connection;
    db.on('error', console.error.bind(console, 'connection error:'));
    db.once('open', function() {
    console.log("Connected to DB");
    });
    ```
    >[!Note]
    > Aqui, as variáveis de ambiente são carregadas com process.env. {variableName} utilizando o pacote de npm 'dotenv'.

    Assim que estiver ligado à base de dados do Azure Cosmos, agora pode começar a configurar modelos de objetos no Mongoose.
    
## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Avisos para utilizar o Mongoose com base de dados do Azure Cosmos

Para cada modelo de que criar o Mongoose cria uma nova coleção de MongoDB por baixo de bastidores. No entanto, tendo em conta o modelo de faturação por coleção do Azure Cosmos DB, poderá não ter a forma mais económica para aceder, se já tem vários modelos de objetos que estão preenchidos sparsely.

Estas instruções abrangem ambos os modelos. Teremos primeiro de saber as instruções no armazenamento de um tipo de dados por coleção. Este é o comportamento de defacto para o Mongoose.

O mongoose também tem um conceito chamado [Discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators são um mecanismo de herança de esquema. Dão-lhe ter vários modelos com esquemas de sobreposição por cima da mesma coleção de MongoDB subjacente.

Pode armazenar vários modelos de dados na mesma coleção e, em seguida, utilizar uma cláusula de filtro no momento da consulta para obter apenas os dados necessários.

### <a name="one-collection-per-object-model"></a>Uma coleção por modelo de objeto

É o comportamento de Mongoose predefinido para criar uma coleção de MongoDB sempre que criar um modelo de objeto. Esta secção explicar como fazer com o MongoDB para a base de dados do Azure Cosmos. Este método é recomendado com BD do Cosmos Azure quando tiver de modelos de objeto com grandes quantidades de dados. Esta é a predefinição operativo modelo para o Mongoose, por isso, poderá estar familiarizado com esta opção, se estiver familiarizado com o Mongoose.

1. Abra o ```index.js``` novamente.

1. Crie a definição de esquema para 'Família'.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Crie um objeto para 'Família'.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Por fim, vamos guardar o objeto de base de dados do Azure Cosmos. Esta ação cria uma coleção, por baixo bastidores.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Agora, vamos criar outro esquema e objeto. Este tempo, vamos criar um 'Férias destinos', que poderão estar interessadas as famílias de.
    1. Tal como a hora da última, vamos criar o esquema
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Criar um objeto de exemplo (pode adicionar vários objetos a este esquema) e guarde-o.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Agora, vai no portal do Azure, reparar em duas coleções criadas na base de dados do Azure Cosmos.

    ![Tutorial node.js - captura de ecrã do portal do Azure, que mostra uma conta de base de dados do Azure Cosmos, com o nome da coleção realçado - base de dados Node][alldata]

1. Por fim, vamos de leitura de dados da base de dados do Azure Cosmos. Uma vez que estamos a utilizar o modelo de sistema operativo Mongoose predefinição, as leituras são os mesmos que quaisquer outras operações de leitura com o Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Utilizar Mongoose discriminators para armazenar dados numa única coleção

Neste método, utilizamos [Mongoose Discriminators](http://mongoosejs.com/docs/discriminators.html) para ajudar a otimizar os custos de cada coleção de BD do Cosmos do Azure. Discriminators permitem-lhe definir um differentiating 'Key', que lhe permite armazenar, diferenciar e filtre os modelos de objetos diferentes.

Aqui, vamos criar um objeto de base do modelo, definir uma chave differentiating e adicione 'Família' e 'VacationDestinations' como uma extensão para o modelo de base.

1. Vamos configurar a base de configuração e defina a chave de discriminador.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Em seguida, vamos definir o modelo de objeto comuns

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Agora, iremos definir o modelo 'Família'. Repare que estamos a utilizar ```commonModel.discriminator``` em vez de ```mongoose.model```. Além disso, estamos a adicionar também a base de configuração para o esquema de mongoose. Por isso, aqui, o discriminatorKey é ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Da mesma forma, vamos adicionar outra esquema, neste momento para 'VacationDestinations'. Aqui, é o DiscriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Por fim, vamos criar objetos para o modelo e guardá-lo.
    1. Vamos adicionar objetos ao modelo de 'Família'.
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Em seguida, vamos adicionar objetos ao modelo de 'VacationDestinations' e guardá-lo.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Agora, se regressar ao portal do Azure, poderá notar que tem apenas uma coleção chamada ```alldata``` com dados de 'Família' e 'VacationDestinations'.

    ![Tutorial node.js - captura de ecrã do portal do Azure, que mostra uma conta de base de dados do Azure Cosmos, com o nome da coleção realçado - base de dados Node][mutiple-coll]

1. Além disso, tenha em atenção que cada objeto tem outro atributo denominado como ```__type```, que ajuda a diferenciar entre os dois modelos de objeto diferentes.

1. Por fim, vamos ler os dados armazenados na base de dados do Azure Cosmos. O mongoose encarrega-se de filtragem com base no modelo de dados. Por isso, terá de fazer nada diferentes durante a leitura de dados. Especifique apenas o seu modelo (neste caso, ```Family_common```) e os identificadores de Mongoose filtragem 'DiscriminatorKey'.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Como pode ver, é fácil de trabalhar com Mongoose discriminators. Por isso, se tiver uma aplicação que utiliza o framework o Mongoose, este tutorial é uma forma de obter a aplicação de cópia de segurança e em execução na API MongoDB na base de dados do Azure Cosmos sem necessidade de existência de demasiadas alterações.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o MongoDB operações, operadores, fases, comandos e opções suportadas pela API do Azure Cosmos DB MongoDB no [suporte da API do MongoDB para funcionalidades do MongoDB e sintaxe](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png