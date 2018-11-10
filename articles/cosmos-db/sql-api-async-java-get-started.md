---
title: Criar uma aplicação Java com o SDK Java Async para gerir os dados da API SQL do Azure Cosmos DB | Microsoft Docs
description: Este tutorial mostra como utilizar contas da API de SQL do Azure Cosmos DB para armazenar e aceder a dados com uma aplicação Async Java.
keywords: tutorial nosql, base de dados online, aplicação de consola java
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: sngun
ms.openlocfilehash: 66e937e92528e2f0a1fca9d9aac78f7265eef4f7
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741237"
---
# <a name="tutorial-build-a-java-app-with-async-java-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Criar uma aplicação Java com o SDK Java Async para gerir os dados da API SQL do Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Este tutorial mostra como criar uma aplicação Java com o SDK Java Async para armazenar e aceder a dados da API SQL do Azure Cosmos DB.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar e ligar a uma conta do Azure Cosmos DB
> * Configurar a sua Solução
> * Criação de uma coleção
> * Criação de documentos JSON
> * Consulta da coleção

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que tem os recursos seguintes:

* Uma conta ativa do Azure. Se não tiver uma, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Development Kit (JDK) 8+](https://aka.ms/azure-jdks).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Passo 1: Criar uma conta do Azure Cosmos DB
Vamos criar uma conta do Azure Cosmos DB. Se já tiver uma conta que pretende utilizar, pode avançar diretamente para [Clonar o projeto GitHub](#GitClone). Se estiver a utilizar o Emulador do Azure Cosmos DB, siga os passos em [Azure Cosmos DB Emulator](local-emulator.md) (Emulador do Azure Cosmos DB) para configurar o emulador e avance para [Clonar o projeto GitHub](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>Passo 2: Clone o repositório do GitHub

Pode começar clonando o repositório do GitHub para [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) (Começar a utilizar o Azure Cosmos DB e Java). Por exemplo, a partir de um diretório local, execute o seguinte para obter o projeto exemplo localmente.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started
```

O diretório contém uma pasta `pom.xml` e `src/main/java/com/microsoft/azure/cosmosdb/sample` que contém código de origem do Java, incluindo `Main.java`. O projeto contém o código necessário para realizar operações com o Azure Cosmos DB, como a criação de documentos e a consulta de dados dentro de uma coleção. O `pom.xml` inclui uma dependência no [ SDK Java Azure Cosmos DB no Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>Passo 3: Ligar a uma conta do Azure Cosmos DB

Em seguida, regresse ao [portal do Azure](https://portal.azure.com) para obter o seu ponto final e a chave mestra primária. O ponto final e a chave primária do Azure Cosmos DB são necessários para que a sua aplicação saiba onde ligar e para que o Azure Cosmos DB confie na ligação da sua aplicação. O ficheiro `AccountSettings.java` contém a chave primária e os valores URI. 

No portal do Azure, navegue até à sua conta do Azure Cosmos DB e, em seguida, clique em **Chaves**. Copie o URI e a CHAVE PRIMÁRIA do portal e cole-os no ficheiro `AccountSettings.java`. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos DB account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos DB account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Captura de ecrã para obter chaves do portal][keys]

## <a name="step-4-initialize-the-client-object"></a>Passo 4: Inicializar o objeto de cliente

Inicialize o objeto de cliente com o URI de anfitrião e os valores de chave primária definidos no ficheiro "AccountSettings.java"

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>Passo 5: Criar uma base de dados

Pode criar a sua [base de dados](sql-api-resources.md#databases) do Azure Cosmos DB com o método createDatabaseIfNotExists() da classe DocumentClient. Uma base de dados é o contentor lógico do armazenamento de documentos JSON particionado em coleções.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a id="CreateColl"></a>Passo 6: Criar uma coleção

> [!WARNING]
> **createCollection** cria uma nova coleção com débito reservado, tendo repercussões sobre os preços. Para obter mais detalhes, visite a nossa [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 
Pode criar uma coleção com o método createDocumentCollectionIfNotExists() da classe DocumentClient. Uma coleção é um contentor de documentos JSON e a lógica da aplicação associada JavaScript.

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a id="CreateDoc"></a>Passo 7: Criar documentos JSON

Pode criar um [documento](sql-api-resources.md#documents) com o método createDocument da classe DocumentClient. Os documentos são conteúdos JSON (arbitrários) definidos pelo utilizador. Podemos agora inserir um ou mais documentos. O ficheiro "src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java" define a família de documentos JSON 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a id="Query"></a>Passo 8: Consultar recursos do Azure Cosmos DB

O Azure Cosmos DB suporta [consultas](sql-api-sql-query.md) extensas de documentos JSON armazenados em cada coleção. O código de exemplo seguinte mostra como consultar documentos no Azure Cosmos DB utilizando sintaxe SQL com o método [queryDocuments](/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments).

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a id="Run"></a>Passo 9: Executar a sua aplicação de consola Java

Para executar a aplicação a partir da consola, navegue para a pasta do projeto e compile com o Maven:

```bash
mvn package
```

Executar `mvn package` transfere a biblioteca do Azure Cosmos DB mais recente a partir do Maven e produz `GetStarted-0.0.1-SNAPSHOT.jar`. Em seguida, execute a aplicação executando:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```

Parabéns! Concluiu este tutorial NoSQL e a sua aplicação de consola Java está a funcionar!

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a compilar uma aplicação Java com o SDK Java Async para gerir dados da API SQL do Azure Cosmos DB. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [Compilar uma aplicação de consola Node.js com o JavaScript SDK e o Azure Cosmos DB](sql-api-nodejs-get-started.md)

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
