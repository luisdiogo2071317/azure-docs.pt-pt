---
title: Criar uma conta, uma base de dados e uma tabela da API para Cassandra do Azure Cosmos DB com uma aplicação Java
description: Este artigo mostra como criar uma conta da API para Cassandra, adicionar uma base de dados (também referida como um keyspace) e uma tabela a essa conta com uma aplicação Java.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: e5b412042d0a872443c0b30c774dfd4926555a5a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221672"
---
# <a name="create-an-azure-cosmos-db-cassandra-api-account-database-and-a-table-by-using-a-java-application"></a>Criar uma conta, uma base de dados e um tabela da API para Cassandra do Azure Cosmos DB com uma aplicação Java

Este tutorial descreve como utilizar uma aplicação Java para criar uma conta da API para Cassandra no Azure Cosmos DB, adicionar uma base de dados (também referida como um keyspace) e adicionar uma tabela. A aplicação Java utiliza o [controlador Java](https://github.com/datastax/java-driver) para criar uma base de dados de utilizador que contém detalhes como o ID do utilizador, o nome do utilizador e a cidade do utilizador.  

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar uma conta de base de dados Cassandra
> * Obter a cadeia de ligação da conta
> * Criar projeto Maven e dependências
> * Adicionar uma base de dados e uma tabela
> * Executar a aplicação

## <a name="prerequisites"></a>Pré-requisitos 

* Se não tiver uma subscrição do Azure, crie uma  [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  antes de começar. Em alternativa, pode  [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/)  sem uma subscrição do Azure, sem encargos nem compromissos. 

* Obter a versão mais recente do [Kit de Desenvolvimento Java (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 

* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) o arquivo binário [Maven](http://maven.apache.org/) 
  - No Ubuntu, pode executar  `apt-get install maven`  para instalar o Maven. 

## <a name="create-a-database-account"></a>Criar uma conta de base de dados 

1. Inicie sessão no  [portal do Azure](https://portal.azure.com/). 

2. Selecione **Criar um recurso** > **Bases de Dados** > **Azure Cosmos DB**. 

3. No painel  **Nova conta** , introduza as definições para a nova conta do Azure Cosmos DB. 

   |Definição   |Valor sugerido  |Descrição  |
   |---------|---------|---------|
   |ID   |   Introduza um nome exclusivo    | Introduza um nome exclusivo para identificar esta conta do Azure Cosmos DB. <br/><br/>Uma vez que cassandra.cosmosdb.azure.com é anexado ao ID que indica para criar o seu ponto de contacto, utilize um ID exclusivo, mas identificável.         |
   |API    |  Cassandra   |  A API determina o tipo de conta a criar. <br/> Selecione **Cassandra**, pois neste artigo vai criar uma base de dados de colunas largas, que pode ser consultada com a sintaxe CQL.  |
   |Subscrição    |  A sua subscrição        |  Selecione a subscrição do Azure que quer utilizar para esta conta do Azure Cosmos DB.        |
   |Grupo de Recursos   | Introduzir um nome    |  Selecione  **Criar Novo** e, em seguida, introduza um novo nome de grupo de recursos para a sua conta. Para simplicidade, pode utilizar o mesmo nome do ID.    |
   |Localização    |  Selecione a região mais próxima dos seus utilizadores    |  Selecione a localização geográfica na qual vai alojar a sua conta do Azure Cosmos DB. Utilize a localização mais próxima dos seus utilizadores para lhes dar o acesso mais rápido aos dados.    |

   ![Criar conta com o portal](./media/create-cassandra-api-account-java/create-account.png)

4. Em seguida, selecione **Criar**. <br/>A criação da conta demora alguns minutos. Depois de o recurso ser criado, pode ver a notificação **Implementação concluída com êxito** no canto direito do portal.

## <a name="get-the-connection-details-of-your-account"></a>Obter os detalhes de ligação da conta  

Obtenha as informações da cadeia de ligação do portal do Azure e copie-as para o ficheiro de configuração Java. Isto permite à aplicação comunicar com a base de dados alojada. 

1. No  [portal do Azure](http://portal.azure.com/), navegue até à sua conta do Cosmos DB. 

2. Abra o painel  **Cadeia de Ligação**.  

3. Copie os valores de **PONTO DE CONTACTO**, **PORTA**, **NOME DE UTILIZADOR** e  **PALAVRA-PASSE PRINCIPAL** para utilizá-los nos passos seguintes.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Criar o projeto Maven, as dependências e as classes de utilitários 

O projeto Java de exemplo que vai utilizar neste artigo está alojado no GitHub. Pode transferi-lo do repositório [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started). 

Depois de transferir os ficheiros, atualize as informações da cadeia de ligação dentro do ficheiro `java-examples\src\main\resources\config.properties` e execute-o.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_host = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Em alternativa, também pode criar o exemplo do zero.  

1. No terminal ou na linha de comandos, crie um novo projeto Maven com o nome Cassandra-demo. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Localize a pasta `cassandra-demo`. Com um editor de texto, abra o ficheiro `pom.xml` que foi gerado. 

   Adicione as dependências de Cassandra e crie os plug-ins exigidos pelo seu projeto, conforme mostrado no ficheiro [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml).  

3. Na pasta `cassandra-demo\src\main`, crie uma nova pasta com o nome `resources`.  Na pasta de recursos, adicione os ficheiros config.properties e log4j.properties:

   - O ficheiro [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) armazena os valores de ponto final e a chave da ligação API para Cassandra do Azure Cosmos DB. 
   
   - O ficheiro [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) define o nível de registo necessário ao interagir com a API para Cassandra.  

4. Em seguida, navegue para a pasta `src/main/java/com/azure/cosmosdb/cassandra/`. Na pasta cassandra, crie outra pasta com o nome `utils`. A nova pasta armazena as classes de utilitários necessárias para ligar à conta da API para Cassandra. 

   Adicione a classe [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) para criar o cluster e abrir e fechar as sessões de Cassandra. O cluster liga à API para Cassandra do Azure Cosmos DB e devolve uma sessão para acesso. Utilize a classe [Configurações](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) para ler as informações da cadeia de ligação do ficheiro config.properties. 

5. O exemplo de Java cria uma base de dados com as informações do utilizador, como o ID do utilizador, o nome do utilizador e a cidade do utilizador. Terá de definir os métodos obter e definir, para aceder aos detalhes do utilizador na função principal.
 
   Crie uma classe [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) na pasta `src/main/java/com/azure/cosmosdb/cassandra/` com os métodos obter e definir. 

## <a name="add-a-database-and-a-table"></a>Adicionar uma base de dados e uma tabela  

Esta secção descreve como adicionar uma base de dados (keyspace) e uma tabela, com a Linguagem CQL (Cassandra Query Language). Para saber mais sobre a sintaxe CQL para estes comandos, consulte a sintaxe de consulta [criar keyspace](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) e [criar tabela](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable). 

1. Na pasta `src\main\java\com\azure\cosmosdb\cassandra`, crie uma nova pasta com o nome `repository`. 

2. Em seguida, crie a classe Java `UserRepository` e adicione o seguinte código à mesma: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Localize a pasta `src\main\java\com\azure\cosmosdb\cassandra` e crie uma nova subpasta com o nome `examples`.

4. Em seguida, crie a classe Java `UserProfile`. Esta classe contém o método principal que chama os métodos createKeyspace e createTable definidos anteriormente: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Executar a aplicação 

1. Abra uma linha de comandos ou janela de terminal. Cole o seguinte bloco de código. 

   Este código altera o diretório (cd) para o caminho da pasta onde criou o projeto. Em seguida, executa o comando `mvn clean install` para gerar o ficheiro `cosmosdb-cassandra-examples.jar` na pasta de destino. Por fim, executa a aplicação Java.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   A janela de terminal apresenta notificações quando o keyspace e a tabela estão criados. 
   
2. Agora, no portal do Azure, abra o **Data Explorer** para confirmar que o keyspace e a tabela foram criados.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como criar uma conta, uma base de dados e uma tabela da API para Cassandra do Azure Cosmos DB com uma aplicação Java. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [carregar dados de exemplo para a tabela da API para Cassandra](cassandra-api-load-data.md).
