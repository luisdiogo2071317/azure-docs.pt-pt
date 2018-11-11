---
title: Tutorial do Node.js para a API SQL do Azure Cosmos DB | Microsoft Docs
description: Um tutorial do Node.js que demonstra como ligar e consultar o Azure Cosmos DB com a API SQL
services: cosmos-db
author: deborahc
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: 7eeb4fdba8272d9bc2b67d8a33dd7b1d210f5e47
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278604"
---
# <a name="tutorial-build-a-nodejs-console-app-with-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Compilar uma aplicação de consola Node.js com o JavaScript SDK para gerir os dados da API SQL do Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Este tutorial mostra-lhe como compilar uma aplicação de consola Node.js para criar recursos do Azure Cosmos DB e consultá-los.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar e ligar a uma conta do Azure Cosmos DB
> * Configurar a sua aplicação
> * Criar uma base de dados
> * Criar um contentor
> * Adicionar itens ao contentor
> * Realizar operações de CRUD nos itens, no contentor e na base de dados

Não tem tempo para criar a aplicação? Não se preocupe! A solução completa está disponível em [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ). Veja a secção [Obter a solução completa](#GetSolution) deste artigo para instruções rápidas.

## <a name="prerequisites"></a>Pré-requisitos 

Certifique-se de que tem os recursos seguintes:

* Uma conta ativa do Azure. Se não tiver uma, pode inscrever-se numa [Avaliação Gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Versão do [Node.js](https://nodejs.org/) v6.0.0 ou superior.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Passo 1: Criar uma conta do Azure Cosmos DB

Vamos criar uma conta do Azure Cosmos DB. Se já tiver uma conta que pretende utilizar, pode avançar diretamente para [Configurar a aplicação Node.js](#SetupNode). Se estiver a utilizar o Emulador do Azure Cosmos DB, siga os passos em [Emulador do Azure Cosmos DB](local-emulator.md) para configurar o emulador e avance para [Configurar a aplicação Node.js](#SetupNode). 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Passo 2: Configurar a aplicação Node.js

1. Abra o seu terminal favorito.
2. Localize a pasta ou o diretório onde pretende guardar a sua aplicação Node.js.
3. Crie dois ficheiros JavaScript vazios com os seguintes comandos:

   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. Instale o módulo @azure/cosmos através do npm. Utilize o seguinte comando:
   * ```npm install @azure/cosmos --save```

## <a id="Config"></a>Passo 3: Configurar as configurações da sua aplicação

1. Abra ```config.js``` no seu editor de texto favorito.

1. Copie e cole o fragmento de código abaixo e defina as propriedades ```config.endpoint``` e ```config.primaryKey``` como o URI e a chave primária do ponto final do Azure Cosmos DB. Pode encontrar ambas as configurações no [portal do Azure](https://portal.azure.com).

   ![Captura de ecrã para obter chaves do portal do Azure][keys]

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.primaryKey = "~your primary key here~";
   ``` 

1. Copie e cole os dados de ```database```, ```container``` e ```items``` no objeto ```config```, por baixo do local onde define as propriedades ```config.endpoint``` e ```config.primaryKey```. Se já tiver dados que pretende armazenar na sua base de dados, pode utilizar a [ferramenta de Migração de Dados](import-data.md) do Azure Cosmos DB em vez de definir os dados aqui.

   ```nodejs
   var config = {}

   config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
   config.primaryKey = "~your primary key here~";

   config.database = {
      "id": "FamilyDatabase"
   };

   config.container = {
     "id": "FamilyContainer"
   };

   config.items = {
      "Andersen": {
          "id": "Anderson.1",
          "lastName": "Andersen",
          "parents": [{
            "firstName": "Thomas"
        }, {
                "firstName": "Mary Kay"
            }],
        "children": [{
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [{
                "givenName": "Fluffy"
            }]
        }],
        "address": {
            "state": "WA",
            "county": "King",
            "city": "Seattle"
        }
    },
    "Wakefield": {
        "id": "Wakefield.7",
        "parents": [{
            "familyName": "Wakefield",
            "firstName": "Robin"
        }, {
                "familyName": "Miller",
                "firstName": "Ben"
            }],
        "children": [{
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [{
                "givenName": "Goofy"
            }, {
                    "givenName": "Shadow"
                }]
        }, {
                "familyName": "Miller",
                "firstName": "Lisa",
                "gender": "female",
                "grade": 1
            }],
        "address": {
            "state": "NY",
            "county": "Manhattan",
            "city": "NY"
        },
        "isRegistered": false
      }
   };
   ```

   Tenha em conta que, se estiver familiarizado com a versão anterior do SDK do JavaScript, poderá estar habituado a ver os termos “coleção” e “documento”. Uma vez que o Azure Cosmos DB suporta [vários modelos de API](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities), a versão 2.0+ do SDK JavaScript utiliza os termos genéricos “contentor” e “item”. Os contentores podem ser uma coleção, um grafo ou uma tabela. Os itens podem ser um documento, um vértice/aresta ou uma linha e são o conteúdo dentro dos contentores. 

1. Por fim, exporte o seu objeto ```config``` para poder referenciá-lo no ficheiro ```app.js```.

   ```nodejs
        },
        "isRegistered": false
       }
   };

   // ADD THIS PART TO YOUR CODE
   module.exports = config;
   ```

## <a id="Connect"></a>Passo 4: Ligar a uma conta do Azure Cosmos DB

1. Abra o seu ficheiro ```app.js``` vazio no editor de texto. Copie e cole o código abaixo para importar o módulo ```@azure/cosmos``` e o módulo ```config``` que criou recentemente.

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');
   ```

1. Copie e cole o código para utilizar ```config.endpoint``` e ```config.primaryKey``` guardados anteriormente e criar um CosmosClient novo.

   ```nodejs
   const url = require('url');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
   ```

Agora que tem o código para inicializar o cliente do Azure Cosmos DB, vamos ver como utilizar os recursos do Azure Cosmos DB.

## <a name="step-5-create-a-database"></a>Passo 5: Criar uma base de dados

1. Copie e cole o código abaixo para definir o ID da base de dados e o ID do contentor. Estes IDs vão permitir ao cliente do Azure Cosmos DB encontrar a base de dados e o contentor corretos.

   ```nodejs
   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   ```

   As [bases de dados](sql-api-resources.md#databases) podem ser criadas com as funções [createIfNotExists](/javascript/api/%40azure/cosmos/databases) ou [create](/javascript/api/%40azure/cosmos/databases) da classe **Bases de dados**. As bases de dados são os contentores lógicos dos itens particionados em contentores. 

2. Copie e cole os métodos **createDatabase** e **readDatabase** no ficheiro app.js, abaixo da definição de ```databaseId``` e de ```containerId```. A função **createDatabase** cria uma base de dados nova com o ID ```FamilyDatabase```, especificado a partir do objeto ```config```, se ainda não existir. A função **readDatabase** lê a definição da base de dados para garantir que a mesma existe.

   ```nodejs
   /**
    * Create the database if it does not exist
    */
   async function createDatabase() {
       const { database } = await client.databases.createIfNotExists({ id: databaseId });
       console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
      const { body: databaseDefinition } = await client.database(databaseId).read();
      console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }
   ```

3. Copie e cole o código por baixo do local onde definiu as funções **createDatabase** e **readDatabase** para adicionar a função de programa auxiliar **exit**, que vai imprimir a mensagem de saída. 

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. Copie e cole o código abaixo de onde definiu a função **exit** para chamar as funções **createDatabase** e **readDatabase**.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   Neste momento, o código em ```app.js``` deve ser semelhante ao código a seguir:

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

    /**
    * Create the database if it does not exist
    */
    async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando: 

   ```bash 
   node app.js
   ```

Parabéns! Criou uma base de dados do Azure Cosmos DB com êxito.

## <a id="CreateContainer"></a>Passo 6: Criar um contentor

> [!WARNING]
> Chamar a função **createContainer** cria um contentor novo, que tem implicações no preço. Para obter mais detalhes, visite a nossa [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/).

Os contentores podem ser criados com as funções [createIfNotExists](/javascript/api/%40azure/cosmos/containers) ou [create](/javascript/api/%40azure/cosmos/containers) da classe **Contentores**.  Os contentores consistem em itens (que, no caso da API SQL, são documentos JSON) e na lógica da aplicação JavaScript associada.

1. Copie e cole as funções **createContainer** e **readContainer** abaixo da função **readDatabase**, no ficheiro app.js. A função **createContainer** cria um contentor novo com ```containerId```, especificado a partir do objeto ```config```, se ainda não existir. A função **readContainer** lê a definição do contentor para verificar se o mesmo existe.

   ```nodejs
   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }
   ```

1. Copie e cole o código abaixo da chamada para **readDatabase** par executar as funções **createContainer** e **readContainer**.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())

     // ADD THIS PART TO YOUR CODE
     .then(() => createContainer())
     .then(() => readContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

   Neste momento, o código em ```app.js``` deve ser semelhante a:

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

   /**
   * Create the database if it does not exist
   */
   async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
     console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
     const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
     console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando: 

   ```bash 
   node app.js
   ```

Parabéns! Criou um contentor do Azure Cosmos DB com êxito.

## <a id="CreateItem">Passo 7: Criar um item</a>

Os itens podem ser criados com a função [create](/javascript/api/%40azure/cosmos/items) da classe **Itens**. Quando é utilizada a API SQL, os itens são projetados como documentos, que são conteúdos JSON definidos pelo utilizador (arbitrários). Pode agora inserir um item no Azure Cosmos DB.

1. Copie e cole a função **createFamilyItem** abaixo da função **readContainer**. A função **createFamilyItem** cria os itens que contêm os dados JSON guardados no objeto ```config```. Vamos confirmar que não existe nenhum item com o mesmo ID antes de o criarmos.

   ```nodejs
   /**
   * Create family item if it does not exist
   */
  async function createFamilyItem(itemBody) {
     try {
         // read the item to see if it exists
         const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
         console.log(`Item with family id ${itemBody.id} already exists\n`);
     }
     catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
     }
   };
   ```

1. Copie e cole o código por baixo da chamada para **readContainer** para executar a função **createFamilyItem**.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando: 

   ```bash 
   node app.js
   ```

Parabéns! Criou um item do Azure Cosmos DB com êxito.


## <a id="Query"></a>Passo 8: Consultar recursos do Azure Cosmos DB
O Azure Cosmos DB suporta [consultas extensas](sql-api-sql-query.md) em documentos JSON armazenados em cada contentor. O seguinte código de exemplo mostra uma consulta que pode executar nos documentos do seu contentor.

1. Copie e cole a função **queryContainer** abaixo da função **createFamilyItem**, no ficheiro app.js. O Azure Cosmos DB suporta consultas de tipo SQL, conforme mostrado abaixo. Para obter mais informações sobre a criação de consultas complexas, consulte o artigo o [Query Playground](https://www.documentdb.com/sql/demo) e a [documentação sobre consultas](sql-api-sql-query.md).

   ```nodejs
   /**
   * Query the container using SQL
    */
   async function queryContainer() {
     console.log(`Querying container:\n${config.container.id}`);

     // query to return all children in a family
     const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. Copie e cole o código por baixo das chamadas para **createFamilyItem** para executar a função **queryContainer**.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))

     // ADD THIS PART TO YOUR CODE
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando:

   ```bash 
   node app.js
   ```

Parabéns! Consultou os seus itens do Azure Cosmos DB com êxito.

## <a id="ReplaceItem"></a>Passo 9: Substituir um item
O Azure Cosmos DB suporta a substituição do conteúdo dos itens.

1. Copie e cole a função **replaceFamilyItem** abaixo da função **queryContainer**, no ficheiro app.js. Repare que alterámos a propriedade “grade” de uma criança do valor original de 5 para 6.

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
      const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
   };
   ```

1. Copie e cole o código por baixo da chamada para **queryContainer** para executar a função **replaceFamilyItem**. Além disso, adicione o código para chamar novamente **queryContainer**, para confirmar que o item foi alterado com êxito.

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     .then(() => queryContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => replaceFamilyItem(config.items.Andersen))
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando:

   ```bash 
   node app.js
   ```

Parabéns! Substituiu um item do Azure Cosmos DB com êxito.

## <a id="DeleteItem"></a>Passo 10: Eliminar um item

O Azure Cosmos DB suporta a eliminação de itens JSON.

1. Copie e cole a função **deleteFamilyItem** por baixo da função **replaceFamilyItem**.

   ```nodejs
  /**
  * Delete the item by ID.
  */
   async function deleteFamilyItem(itemBody) {
      await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. Copie e cole o código por baixo da chamada da segunda **queryContainer** para executar a função **deleteFamilyItem**.

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer
      ())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
      .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando: 

   ```bash 
   node app.js
   ```

Parabéns! Eliminou um item do Azure Cosmos DB com êxito.

## <a id="DeleteDatabase"></a>Passo 11: Eliminar a base de dados

Eliminar a base de dados criada remove a base de dados e todos os recursos subordinados (contentores, itens, etc.).

1. Copie e cole a função **cleanup** por baixo da função **deleteFamilyItem** para remover a base de dados e todos os recursos subordinados.

   ```nodejs
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. Copie e cole o código por baixo da chamada para **deleteFamilyItem** para executar a função **cleanup**.

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())
      .then(() => deleteFamilyItem(config.items.Andersen))

      // ADD THIS PART TO YOUR CODE
      .then(() => cleanup())
      // ENDS HERE

      .then(() => { exit(`Completed successfully`); })
      .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

## <a id="Run"></a>Passo 12: Executar a sua aplicação Node.js em conjunto!

Em conjunto, o código deverá ser semelhante a:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

No seu terminal, localize o seu ficheiro ```app.js``` e execute o comando: 

```bash 
node app.js
```

Deverá ver o resultado da sua aplicação Introdução. A saída deve corresponder ao texto de exemplo abaixo.

   ```
    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit
   ```

Parabéns! Concluiu o tutorial de Node.js e tem a sua primeira aplicação de consola do Azure Cosmos DB!

## <a id="GetSolution"></a>Obter a solução completa do tutorial Node.js

Se não tiver tempo de completar os passos deste tutorial, ou apenas pretender transferir o código, pode obtê-lo a partir do [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ).

Para executar a solução de introdução que contém todo o código neste artigo, precisa dos seguintes passos:

* Uma [conta do Azure Cosmos DB][create-account].
* A solução [Getting Started](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started), disponível no GitHub.

Instale o módulo **@azure/cosmos** através do npm. Utilize o seguinte comando:

* ```npm install @azure/cosmos --save```

Em seguida, no ficheiro ```config.js```, atualize os valores config.endpoint e config.primaryKey, conforme descrito em [Passo 3: Definir as configurações da sua aplicação](#Config). 

Em seguida, no seu terminal, localize o ficheiro ```app.js``` e execute o comando: 

```bash 
node app.js
```

E já está tudo concluído! 

## <a name="next-steps"></a>Passos seguintes

* Pretende um exemplo de Node.js mais complexo? Consulte [Build a Node.js web application using Azure Cosmos DB](sql-api-nodejs-application.md) (Criar uma aplicação Web Node.js com o Azure Cosmos DB).
* Saiba como [monitorizar uma conta do Azure Cosmos DB](monitor-accounts.md).
* Execute consultas no nosso conjunto de dados de exemplo no [Query Playground](https://www.documentdb.com/sql/demo).

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png