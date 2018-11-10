---
title: Consultar dados de uma conta da API para Cassandra do Azure Cosmos DB
description: Este artigo mostra como consultar dados de uma conta da API para Cassandra do Azure Cosmos DB através de uma aplicação Java.
services: cosmos-db
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: a06e7e6159953bfeffa966759d29b91bbcbafd37
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739214"
---
# <a name="tutorial-query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Tutorial: consultar dados de uma conta da API para Cassandra do Azure Cosmos DB

Este tutorial mostra como consultar dados de uma conta da API para Cassandra do Azure Cosmos DB através de uma aplicação Java. A aplicação Java utiliza o [controlador Java](https://github.com/datastax/java-driver) e consulta dados de utilizador, como o ID de utilizador, o nome de utilizador e a cidade do utilizador. 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Consultar dados de uma tabela Cassandra
> * Executar a aplicação

## <a name="prerequisites"></a>Pré-requisitos

* Este artigo pertence a um tutorial com várias partes. Antes de começar, certifique-se de que conclui os passos anteriores para criar a conta, o keyspace e a tabela da API para Cassandra e [carrega os dados de exemplo para a tabela](cassandra-api-load-data.md). 

## <a name="query-data"></a>Consultar dados

Utilize os seguintes passos para consultar dados da sua conta de API para Cassandra:

1. Abra o ficheiro `UserRepository.java` na pasta `src\main\java\com\azure\cosmosdb\cassandra`. Anexe o seguinte bloco de código. Este código fornece três métodos: 

   * Para consultar todos os utilizadores na base de dados
   * Para consultar um utilizador específico, filtrado por ID de utilizador
   * Para eliminar uma tabela.

   ```java
   /**
   * Select all rows from user table
   */
   public void selectAllUsers() {

     final String query = "SELECT * FROM uprofile.user";
     List<Row> rows = session.execute(query).all();

     for (Row row : rows) {
        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
     }
   }

   /**
   * Select a row from user table
   *
   * @param id user_id
   */
   public void selectUser(int id) {
      final String query = "SELECT * FROM uprofile.user where user_id = 3";
      Row row = session.execute(query).one();

      LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
   }

   /**
   * Delete user table.
   */
   public void deleteTable() {
     final String query = "DROP TABLE IF EXISTS uprofile.user";
     session.execute(query);
   }
   ```

2. Abra o ficheiro `UserProfile.java` na pasta `src\main\java\com\azure\cosmosdb\cassandra`. Esta classe contém o método principal que chama os métodos createKeyspace, createTable e inserir dados definidos anteriormente. Agora, anexe o código seguinte, que consulta todos os utilizadores ou um utilizador específico:

   ```java
   LOGGER.info("Select all users");
   repository.selectAllUsers();

   LOGGER.info("Select a user by id (3)");
   repository.selectUser(3);

   LOGGER.info("Delete the users profile table");
   repository.deleteTable();
   ```

## <a name="run-the-java-app"></a>Executar a aplicação Java
1. Abra uma linha de comandos ou janela de terminal. Cole o seguinte bloco de código. 

   Este código altera o diretório (cd) para o caminho da pasta onde criou o projeto. Em seguida, executa o comando `mvn clean install` para gerar o ficheiro `cosmosdb-cassandra-examples.jar` na pasta de destino. Por fim, executa a aplicação Java.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Agora, no portal do Azure, abra o **Data Explorer** e verifique se a tabela de utilizador foi eliminada.

## <a name="next-steps"></a>Passos seguintes

* Neste tutorial, ficou a saber como consultar dados da conta da API para Cassandra do Azure Cosmos DB. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [Migrar dados para a conta da API para Cassandra](cassandra-import-data.md)


