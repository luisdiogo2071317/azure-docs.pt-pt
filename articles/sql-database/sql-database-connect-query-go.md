---
title: Utilizar o Go para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: "Utilize o Go para criar um programa que se liga a uma Base de Dados SQL do Azure utilize declarações Transact-SQL para consultar e modificar dados."
services: sql-database
documentationcenter: 
author: David-Engel
manager: craigg
editor: MightyPen
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: go
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: v-daveng
ms.openlocfilehash: 248f4d0b4b54ce2a2ca005379a590d895e9b4b3e
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2017
---
# <a name="use-go-to-query-an-azure-sql-database"></a>Utilizar o Go para consultar uma base de dados SQL do Azure

Este início rápido explica como utilizar o [Go](https://godoc.org/github.com/denisenkom/go-mssqldb) para ligar a uma base de dados SQL do Azure. As declarações Transact-SQL para consultar e modificar dados também são explicadas.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial de início rápido, certifique-se de que tem os seguintes pré-requisitos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Uma [regra de firewall ao nível do servidor](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) para o endereço IP público do computador que utilizar para este tutorial de início rápido.

- Ter instalado o Go e software relacionado para o seu sistema operativo:

    - **MacOS**: instalar o Homebrew e o GoLang. Veja o [Passo 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/mac/).
    - **Ubuntu**: instalar o GoLang. Veja o [Passo 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/ubuntu/).
    - **Windows**: instalar o GoLang. Veja o [Passo 1.2](https://www.microsoft.com/sql-server/developer-get-started/go/windows/).    

## <a name="sql-server-connection-information"></a>Informações de ligação do servidor SQL

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-go-project-and-dependencies"></a>Criar projeto Go e dependências

1. A partir do terminal, crie uma nova pasta de projeto denominada **SqlServerSample**. 

   ```bash
   mkdir SqlServerSample
   ```

2. Altere o diretório para **SqlServerSample** e instale o controlador do SQL Server para Go:

   ```bash
   cd SqlServerSample
   go get github.com/denisenkom/go-mssqldb
   go install github.com/denisenkom/go-mssqldb
   ```

## <a name="create-sample-data"></a>Criar dados de exemplo

1. Com o seu editor de texto favorito, crie um ficheiro denominado **CreateTestData.sql** na pasta **SqlServerSample**. Copie e cole o seguinte código T-SQL no mesmo. Este código cria um esquema e uma tabela, e insere alguns linhas.

   ```sql
   CREATE SCHEMA TestSchema;
   GO

   CREATE TABLE TestSchema.Employees (
     Id       INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
     Name     NVARCHAR(50),
     Location NVARCHAR(50)
   );
   GO

   INSERT INTO TestSchema.Employees (Name, Location) VALUES
     (N'Jared',  N'Australia'),
     (N'Nikita', N'India'),
     (N'Tom',    N'Germany');
   GO

   SELECT * FROM TestSchema.Employees;
   GO
   ```

2. Ligue à base de dados com o sqlcmd e execute o script SQL para criar o esquema e a tabela, e inserir algumas linhas. Substitua os valores adequados para o servidor, a base de dados, o nome de utilizador e a palavra-passe.

   ```bash
   sqlcmd -S your_server.database.windows.net -U your_username -P your_password -d your_database -i ./CreateTestData.sql
   ```

## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar a base de dados do SQL

1. Crie um ficheiro denominado **sample.go** na pasta **SqlServerSample**.

2. Abra o ficheiro e substitua o conteúdo pelo seguinte código. Adicone os valores adequados para o servidor, a base de dados, o nome de utilizador e a palavra-passe. Este exemplo utiliza os métodos de contexto GoLang para garantir que existe uma ligação ativa ao servidor da base de dados.

   ```go
   package main

   import (
       _ "github.com/denisenkom/go-mssqldb"
       "database/sql"
       "context"
       "log"
       "fmt"
   )

   var db *sql.DB

   var server = "your_server.database.windows.net"
   var port = 1433
   var user = "your_username"
   var password = "your_password"
   var database = "your_database"

   func main() {
       // Build connection string
       connString := fmt.Sprintf("server=%s;user id=%s;password=%s;port=%d;database=%s;",
           server, user, password, port, database)

       var err error

       // Create connection pool
       db, err = sql.Open("sqlserver", connString)
       if err != nil {
           log.Fatal("Error creating connection pool:", err.Error())
       }
       fmt.Printf("Connected!\n")

       // Create employee
       createId, err := CreateEmployee("Jake", "United States")
       fmt.Printf("Inserted ID: %d successfully.\n", createId)

       // Read employees
       count, err := ReadEmployees()
       fmt.Printf("Read %d rows successfully.\n", count)

       // Update from database
       updateId, err := UpdateEmployee("Jake", "Poland")
       fmt.Printf("Updated row with ID: %d successfully.\n", updateId)

       // Delete from database
       rows, err := DeleteEmployee("Jake")
       fmt.Printf("Deleted %d rows successfully.\n", rows)
   }

   func CreateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()
       var err error

       if db == nil {
           log.Fatal("What?")
       }

       // Check if database is alive.
       err = db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("INSERT INTO TestSchema.Employees (Name, Location) VALUES (@Name,@Location);")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))

       if err != nil {
           log.Fatal("Error inserting new row: " + err.Error())
           return -1, err
       }

       return result.LastInsertId()
   }

   func ReadEmployees() (int, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("SELECT Id, Name, Location FROM TestSchema.Employees;")

       // Execute query
       rows, err := db.QueryContext(ctx, tsql)
       if err != nil {
           log.Fatal("Error reading rows: " + err.Error())
           return -1, err
       }

       defer rows.Close()

       var count int = 0

       // Iterate through the result set.
       for rows.Next() {
           var name, location string
           var id int

           // Get values from row.
           err := rows.Scan(&id, &name, &location)
           if err != nil {
               log.Fatal("Error reading rows: " + err.Error())
               return -1, err
           }

           fmt.Printf("ID: %d, Name: %s, Location: %s\n", id, name, location)
           count++
       }

       return count, nil
   }

   // Update an employee's information
   func UpdateEmployee(name string, location string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("UPDATE TestSchema.Employees SET Location = @Location WHERE Name= @Name")

       // Execute non-query with named parameters
       result, err := db.ExecContext(
           ctx,
           tsql,
           sql.Named("Location", location),
           sql.Named("Name", name))
       if err != nil {
           log.Fatal("Error updating row: " + err.Error())
           return -1, err
       }

       return result.LastInsertId()
   }

   // Delete an employee from database
   func DeleteEmployee(name string) (int64, error) {
       ctx := context.Background()

       // Check if database is alive.
       err := db.PingContext(ctx)
       if err != nil {
           log.Fatal("Error pinging database: " + err.Error())
       }

       tsql := fmt.Sprintf("DELETE FROM TestSchema.Employees WHERE Name=@Name;")

       // Execute non-query with named parameters
       result, err := db.ExecContext(ctx, tsql, sql.Named("Name", name))
       if err != nil {
           fmt.Println("Error deleting row: " + err.Error())
           return -1, err
       }

       return result.RowsAffected()
   }
   ```

## <a name="run-the-code"></a>Executar o código

1. Na linha de comandos, execute os comandos seguintes:

   ```bash
   go run sample.go
   ```

2. Verificar a saída:

   ```text
   Connected!
   Inserted ID: 4 successfully.
   ID: 1, Name: Jared, Location: Australia
   ID: 2, Name: Nikita, Location: India
   ID: 3, Name: Tom, Location: Germany
   ID: 4, Name: Jake, Location: United States
   Read 4 rows successfully.
   Updated row with ID: 4 successfully.
   Deleted 1 rows successfully.
   ```

## <a name="next-steps"></a>Passos seguintes

- [Criar a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md)
- [Controlador Go para Microsoft SQL Server](https://github.com/denisenkom/go-mssqldb)
- [Report issues or ask questions](https://github.com/denisenkom/go-mssqldb/issues) (Comunicar problemas ou fazer perguntas)

