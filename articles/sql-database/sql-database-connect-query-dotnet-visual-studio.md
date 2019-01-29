---
title: Utilizar o Visual Studio com o .NET e C# para consultar a base de dados SQL do Azure | Documentos da Microsoft
description: Utilizar o Visual Studio para criar um C# aplicações que se liga a uma base de dados do SQL do Azure e o consulta com instruções Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: dotnet
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/11/2018
ms.openlocfilehash: 93249b7d274ce9d7928dfa46eb339da68c92b785
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163301"
---
# <a name="quickstart-use-net-and-c-in-visual-studio-to-connect-to-and-query-an-azure-sql-database"></a>Início rápido: Use o .NET e C# no Visual Studio para ligar e consultar uma base de dados SQL do Azure

Este início rápido mostra como utilizar o [.NET framework](https://www.microsoft.com/net/) e C# código no Visual Studio para consultar uma base de dados SQL do Azure com instruções Transact-SQL.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) da Comunidade, Professional ou Enterprise edition.

## <a name="get-sql-server-connection-information"></a>Obter as informações de ligação do SQL server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-the-sql-database"></a>Criar código para consultar a base de dados SQL

1. No Visual Studio, selecione **arquivo** > **New** > **projeto**. 
   
1. Na **novo projeto** caixa de diálogo, selecione **Visual C#** e, em seguida, selecione **aplicação de consola (.NET Framework)**.
   
1. Introduza *sqltest* para o nome do projeto e, em seguida, selecione **OK**. O novo projeto é criado. 
   
1. Selecione **Project** > **gerir pacotes NuGet**. 
   
1. Na **Gestor de pacotes NuGet**, selecione a **procurar** separador, em seguida, procure e selecione **SqlClient**.
   
1. Sobre o **SqlClient** página, selecione **instalar**. 
   - Se lhe for pedido, selecione **OK** para continuar com a instalação. 
   - Se um **aceitação da licença** for apresentada a janela, selecione **aceito**.
   
1. Quando a instalação estiver concluída, pode fechar **Gestor de pacotes NuGet**. 
   
1. No editor de código, substitua a **Program.cs** conteúdo com o código a seguir. Substitua os valores pelas `<server>`, `<username>`, `<password>`, e `<database>`.
   
   >[!IMPORTANT]
   >O código neste exemplo utiliza os dados de AdventureWorksLT de exemplo, o que pode escolher como origem ao criar a sua base de dados. Se a sua base de dados tiver dados diferentes, utilize a consulta SELECT tabelas da sua própria base de dados. 
   
   ```csharp
   using System;
   using System.Data.SqlClient;
   using System.Text;
   
   namespace sqltest
   {
       class Program
       {
           static void Main(string[] args)
           {
               try 
               { 
                   SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                   builder.DataSource = "<server>.database.windows.net"; 
                   builder.UserID = "<username>";            
                   builder.Password = "<password>";     
                   builder.InitialCatalog = "<database>";
   
                   using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                   {
                       Console.WriteLine("\nQuery data example:");
                       Console.WriteLine("=========================================\n");
                       
                       connection.Open();       
                       StringBuilder sb = new StringBuilder();
                       sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                       sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                       sb.Append("JOIN [SalesLT].[Product] p ");
                       sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                       String sql = sb.ToString();
   
                       using (SqlCommand command = new SqlCommand(sql, connection))
                       {
                           using (SqlDataReader reader = command.ExecuteReader())
                           {
                               while (reader.Read())
                               {
                                   Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                               }
                           }
                       }                    
                   }
               }
               catch (SqlException e)
               {
                   Console.WriteLine(e.ToString());
               }
               Console.ReadLine();
           }
       }
   }
   ```

## <a name="run-the-code"></a>Executar o código

1. Para executar a aplicação, selecione **depurar** > **iniciar depuração**, ou selecione **iniciar** na barra de ferramentas ou pressione **F5**.
1. Certifique-se de que as linhas de categoria/produto de 20 melhores da sua base de dados são devolvidas e, em seguida, feche a janela da aplicação.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [ligar e consultar uma base de dados SQL do Azure com o .NET Core](sql-database-connect-query-dotnet-core.md) em Windows/Linux/macOS.  
- Saiba mais sobre a [Introdução ao .NET Core com Windows/Linux/macOS, utilizando a linha de comandos](/dotnet/core/tutorials/using-with-xplat-cli).
- Aprenda a [Criar a sua primeira base de dados SQL do Azure com o SSMS](sql-database-design-first-database.md) ou a [Criar a sua primeira base de dados SQL do Azure com o .NET](sql-database-design-first-database-csharp.md).
- Para obter mais informações sobre o .NET, veja a [Documentação .NET](https://docs.microsoft.com/dotnet/).
- Exemplo de lógica de repetição: [Ligar de forma resiliente ao SQL Server com ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n].


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

