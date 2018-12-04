---
title: Carregar dados de exemplo para uma tabela do Cassandra API do Azure Cosmos DB através de uma aplicação de Java
description: Este artigo mostra como carregar dados de exemplo de utilizador para uma tabela na conta da API para Cassandra do Azure Cosmos DB através de uma aplicação Java.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: ac297a04936785c76b7651cb5b78080baf4f684c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836258"
---
# <a name="tutorial-load-sample-data-into-an-azure-cosmos-db-cassandra-api-table"></a>Tutorial: carregar dados de exemplo para uma tabela da API para Cassandra do Azure Cosmos DB

Este tutorial mostra como carregar dados de exemplo de utilizador para uma tabela na conta da API para Cassandra do Azure Cosmos DB através de uma aplicação Java. A aplicação Java utiliza o [controlador Java](https://github.com/datastax/java-driver) e carrega dados de utilizador, como o ID de utilizador, o nome de utilizador e a cidade do utilizador. 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Carregar dados para uma tabela Cassandra
> * Executar a aplicação

## <a name="prerequisites"></a>Pré-requisitos

* Este artigo pertence a um tutorial com várias partes. Antes de começar este documento, certifique-se de que [cria a conta, um keyspace e uma tabela da API para Cassandra](create-cassandra-api-account-java.md).   

## <a name="load-data-into-the-table"></a>Carregar dados para a tabela

Utilize os seguintes passos para carregar dados para a sua tabela da API para Cassandra:

1. Abra o ficheiro “UserRepository.java” na pasta “src\main\java\com\azure\cosmosdb\cassandra” e anexe os campos user_id, user_name e user_bcity à tabela:

   ```java
   /**
   * Insert a row into user table
   *
   * @param id   user_id
   * @param name user_name
   * @param city user_bcity
   */
   public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
   }

   /**
   * Create a PrepareStatement to insert a row to user table
   *
   * @return PreparedStatement
   */
   public PreparedStatement prepareInsertStatement() {
      final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
   return session.prepare(insertStatement);
   }
   ```
 
2. Abra o ficheiro “UserProfile.java” na pasta “src\main\java\com\azure\cosmosdb\cassandra”. Esta classe contém o método principal que chama os métodos createKeyspace e createTable definidos anteriormente. Agora, anexe o seguinte código para inserir alguns dados de exemplo na tabela da API para Cassandra.

   ```java
   //Insert rows into user table
   PreparedStatement preparedStatement = repository.prepareInsertStatement();
     repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
     repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
     repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
     repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
     repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
   ```

## <a name="run-the-app"></a>Executar a aplicação

Abra a linha de comandos ou janela de terminal e altere o caminho da pasta onde criou o projeto. Execute o comando “mvn clean install” para gerar o ficheiro ccosmosdb-cassandra-examples.jar na pasta de destino e executar a aplicação. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Agora, pode abrir o Data Explorer no portal do Azure para verificar se as informações de utilizador foram adicionadas à tabela.
    
## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como carregar dados de exemplo para a conta da API para Cassandra do Azure Cosmos DB. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [Consultar dados da conta da API para Cassandra](cassandra-api-query-data.md)
