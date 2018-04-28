---
title: 'Tutorial: Consulta elástica com o armazém de dados SQL do Azure | Microsoft Docs'
description: Este tutorial utiliza a funcionalidade de consulta elástico a consultar o Azure SQL Data Warehouse da SQL Database do Azure.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/14/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: a31f035b5ec086a046028956c4a9c0de0d6a313d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-use-elastic-query-to-access-data-in-azure-sql-data-warehouse-from-azure-sql-database"></a>Tutorial: Utilize elástico consulta para aceder a dados no Azure SQL Data Warehouse da SQL Database do Azure

Este tutorial utiliza a funcionalidade de consulta elástico a consultar o Azure SQL Data Warehouse da SQL Database do Azure. 

## <a name="prerequisites-for-the-tutorial"></a>Pré-requisitos para o tutorial

Antes de começar o tutorial, tem de ter os seguintes pré-requisitos:

1. Instalar o SQL Server Management Studio (SSMS).
2. Criar um servidor de SQL do Azure com um base de dados do armazém de dados dentro deste servidor.
3. Configure regras de firewall para aceder ao servidor de SQL do Azure.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>Configurar a ligação entre instâncias de armazém de dados do SQL Server e base de dados SQL 

1. Com o SSMS ou de outro cliente de consulta, abra uma nova consulta da base de dados **mestre** no seu servidor lógico.

2. Crie um início de sessão e o utilizador que representa a base de dados do SQL Server para a ligação do armazém de dados.

   ```sql
   CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
   ```

3. Com o SSMS ou de outro cliente de consulta, abra uma nova consulta para o **instância de armazém de dados do SQL Server** no seu servidor lógico.

4. Criar um utilizador na instância de armazém de dados com o início de sessão que criou no passo 2

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Conceder permissões para o utilizador no passo 4 que utilizaria como a base de dados do SQL Server gostaria de executar. Neste exemplo, permissão está apenas a ser concedida selecione num esquema específico, que ilustra como possível podem impedir consultas na base de dados do SQL Server para um domínio específico. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. Com o SSMS ou de outro cliente de consulta, abra uma nova consulta para o **instância de base de dados do SQL Server** no seu servidor lógico.

7. Se já tiver uma, crie uma chave mestra. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. Crie uma credencial com âmbito de base de dados utilizando as credenciais que criou no passo 2.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Crie uma origem de dados externa que aponta para a instância de armazém de dados.

   ```sql
   CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
       (TYPE = RDBMS, 
       LOCATION = '<SERVER NAME>.database.windows.net', 
       DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
       CREDENTIAL = SalesDBElasticCredential, 
   ) ;
   ```

10. Agora, pode criar as tabelas externas que referenciam esta origem de dados externa. Nessas tabelas a utilizar as consultas são enviadas para a instância de armazém de dados a processar e enviadas para a instância de base de dados.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>Consulta elástica da base de dados do SQL ao SQL data warehouse

Os próximos passos, irá criar uma tabela no nosso instância de armazém de dados com vários valores. Em seguida, vamos demonstrar como configurar uma tabela externa para consultar a instância de armazém de dados na instância de base de dados.

1. Com o SSMS ou de outro cliente de consulta, abra uma nova consulta para o **SQL Data Warehouse** no seu servidor lógico.

2. Submeter a consulta seguinte para criar um **OrdersInformation** tabela na sua instância de armazém de dados.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL 
   ,   [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. Com o SSMS ou de outro cliente de consulta, abra uma nova consulta para o **base de dados SQL** no seu servidor lógico.

4. Submeter a consulta seguinte para criar uma definição de tabela externa que aponta para o **OrdersInformation** tabela na instância do armazém de dados.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL
   ,   [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (
        DATA_SOURCE = EnterpriseDwSrc
   ,    SCHEMA_NAME = N'dbo'
   ,    OBJECT_NAME = N'OrderInformation'
   )
   ```

5. Reparar que tem agora uma definição de tabela externa sua **instância de base de dados do SQL Server**.

   ![definição de tabela externa de consulta elástico](media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Submeta a consulta seguinte, o que consulta a instância de armazém de dados. Deverá receber os cinco valores introduzidos no passo 2. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> Tenha em atenção que, apesar de alguns valores, esta consulta demora tempo considerável a devolver. Quando utilizar consulta elástica com o armazém de dados, uma deve considerar os custos gerais de processamento de consultas e movimento através da transmissão. Utilize execução remota de consulta elástico quando a capacidade de computação, a latência, a prioridade.

Parabéns, configurou as noções básicas muito da consulta elástico. 

## <a name="next-steps"></a>Passos Seguintes
Para ver as recomendações, consulte [melhores práticas para utilizar consulta elástico com o Azure SQL Data Warehouse](how-to-use-elastic-query-with-sql-data-warehouse.md).