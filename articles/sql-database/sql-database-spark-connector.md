---
title: Spark conector com a base de dados SQL do Azure e o SQL Server | Microsoft Docs
description: Saiba como utilizar o conector de Spark SQL Database do Azure e SQL Server
services: sql-database
author: allenwux
manager: craigg
ms.service: sql-database
ms.custom: ''
ms.topic: article
ms.date: 04/23/2018
ms.author: xiwu
ms.openlocfilehash: 393af463c4145e1d865c14f2ace7d5123ab12cfa
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Acelerar a análise de macrodados em tempo real com o conector de Spark SQL Database do Azure e SQL Server

O conector de Spark SQL Database do Azure e SQL Server permite bases de dados SQL, incluindo a SQL Database do Azure e o SQL Server, ao atuar como sink de dados de origem ou de saída dados de entrada para as tarefas do Spark. Permite-lhe utilizar dados transacionais em tempo real na análise de macrodados e manter os resultados de consultas ad hoc ou relatórios. Em comparação com o conector de JDBC incorporadas, este conector fornece a capacidade de em massa a inserção de dados para bases de dados SQL. -Pode superam o desempenho dos inserção da linha por linha com o x 10 para um desempenho mais rápido x 20. O conector de Spark SQL Database do Azure e SQL Server também suporta a autenticação do AAD. Permite-lhe ligar em segurança na base de dados SQL do Azure, a partir de Databricks do Azure utilizando a sua conta do AAD. Fornece interfaces semelhantes com o conector JDBC incorporado. É fácil migrar as tarefas do Spark existentes para utilizar este conector de novo.

## <a name="download"></a>Transferência
Para começar, transfira o Spark para o conector de base de dados SQL a partir do [repositório azure-sqldb-spark](https://github.com/Azure/azure-sqldb-spark) no GitHub.

## <a name="official-supported-versions"></a>Versões suportadas do oficial

| Componente                            |Versão                  |
| :----------------------------------- | :---------------------- |
| Apache Spark                         |2.0.2 ou posterior           |
| Scala                                |2.10 ou posterior            |
| Microsoft JDBC Driver para SQL Server |6.2 ou posterior             |
| Microsoft SQL Server                 |SQL Server 2008 ou posterior |
| Base de Dados SQL do Azure                   |Suportadas                |

O conector de Spark SQL Database do Azure e SQL Server utiliza o controlador de JDBC da Microsoft para o SQL Server mover dados entre nós de trabalho de Spark e bases de dados SQL:
 
O fluxo de dados é o seguinte:
1. O nó mestre do Spark estabelece ligação ao SQL Server ou SQL Database do Azure e carrega dados a partir de uma tabela específica ou utilizando uma consulta SQL específica
2. O nó mestre do Spark distribui dados a nós de trabalho de transformação. 
3. O nó de trabalho estabelece ligação ao SQL Server ou SQL Database do Azure e escreve dados na base de dados. Utilizador pode optar por utilizar a inserção da linha por linha ou efetuar a inserção em massa.

O diagrama seguinte ilustra o fluxo de dados.

   ![arquitetura](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>Criar o Spark para o conector de base de dados SQL
Atualmente, o projeto de conector utiliza maven. Para criar o conector sem dependências, pode executar:

- pacote limpa mvn
- Transferir as versões mais recentes do JAR da pasta de versão
- Incluir o Spark de BD do SQL Server JAR

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Ligar o Spark BD SQL através do conector
Pode ligar à SQL Database do Azure ou SQL Server das tarefas do Spark, ler ou escrever dados. Também pode executar uma consulta DML ou DDL numa SQL database do Azure ou a base de dados do SQL Server.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Ler dados a partir da base de dados do Azure SQL ou SQL Server

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients"
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Leitura de dados do SQL Database do Azure ou SQL Server com a consulta especificada do SQL Server
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDb(config)
collection.show()
```

### <a name="write-data-to-azure-sql-database-or-sql-server"></a>Escrever dados na base de dados do Azure SQL ou SQL Server
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._
 
// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients"
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-database-or-sql-server"></a>Executar consulta DML ou DDL na SQL Database do Azure ou SQL Server
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.SqlDBQuery(config)
```

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>Ligar o Spark a base de dados do SQL do Azure através da autenticação do AAD
Pode ligar à base de dados do SQL do Azure através da autenticação do Azure Active Directory (AAD). Utilize autenticação do AAD para gerir centralmente identidades de utilizadores de base de dados e como alternativa à autenticação do SQL Server.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Ligar utilizando o modo de autenticação ActiveDirectoryPassword
#### <a name="setup-requirement"></a>Requisitos de configuração
Se estiver a utilizar o modo de autenticação ActiveDirectoryPassword, terá de transferir [azure-activedirectory-library-para-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e as respetivas dependências e inclui-las no caminho de compilação de Java.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username ",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

### <a name="connecting-using-access-token"></a>Ligar utilizando o Token de acesso
#### <a name="setup-requirement"></a>Requisitos de configuração
Se estiver a utilizar o modo de autenticação baseada em tokens de acesso, terá de transferir [azure-activedirectory-library-para-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) e as respetivas dependências e inclui-las no caminho de compilação de Java.

Consulte [utilização do Azure autenticação do Active Directory para autenticação com base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) para saber como obter acesso token para a base de dados SQL do Azure.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token ",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Escrever dados na base de dados SQL do Azure ou SQL Server utilizando a inserção em massa
O conector de jdbc tradicional escreve dados na base de dados SQL do Azure ou SQL Server utilizando a inserção da linha por linha. Pode utilizar o Spark para o conector de BD SQL escrever dados na base de dados do SQL Server utilizando a inserção em massa. Melhora significativamente o desempenho de escrita ao carregar grandes conjuntos de dados ou de carregamento de dados em tabelas em que é utilizado um índice de arquivo de colunas.

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/** 
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "databaseName"      -> "zeqisql",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>Passos Seguintes
Se ainda não o fez, transfira o conector de Spark SQL Database do Azure e SQL Server a partir [repositório do GitHub sqldb-azure-spark](https://github.com/Azure/azure-sqldb-spark) e explorar os recursos adicionais no repositório:

-   [Blocos de notas do exemplo Databricks do Azure](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Scripts de exemplo (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

Pode também querer rever o [Apache Spark SQL, DataFrames e conjuntos de dados guia](http://spark.apache.org/docs/latest/sql-programming-guide.html) e [documentação do Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

