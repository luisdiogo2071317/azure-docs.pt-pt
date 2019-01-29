---
title: Utilizar .NET Core para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra-lhe como utilizar o .NET Core para criar um programa que se liga a uma base de dados do SQL do Azure e a consulta com instruções Transact-SQL.
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
ms.date: 12/10/2018
ms.openlocfilehash: 4d2acf64f7798f845c2993b108b7074283c40be6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55160020"
---
# <a name="quickstart-use-net-core-c-to-query-an-azure-sql-database"></a>Início rápido: Utilizar o .NET Core (C#) para consultar uma base de dados SQL do Azure

Neste início rápido, vai utilizar [.NET Core](https://www.microsoft.com/net/) e C# código para ligar a uma base de dados SQL do Azure. Em seguida, execute uma instrução de Transact-SQL para consultar dados.

## <a name="prerequisites"></a>Pré-requisitos

Para este tutorial, terá de:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- [.NET core para o seu sistema de operativo](https://www.microsoft.com/net/core) instalado. 

> [!NOTE]
> Este início rápido utiliza a *mySampleDatabase* base de dados. Se pretender utilizar uma base de dados diferente, terá de alterar as referências de base de dados e modificar a `SELECT` consulta o C# código.


## <a name="get-sql-server-connection-information"></a>Obter as informações de ligação do SQL server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

#### <a name="get-adonet-connection-information-optional"></a>Obter as informações de ligação do ADO.NET (opcional)

1. Navegue para o **mySampleDatabase** página e, em **definições**, selecione **cadeias de ligação**.

2. Reveja a cadeia de ligação **ADO.NET** completa.

    ![Cadeia de ligação de ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string2.png)

3. Copiar o **ADO.NET** cadeia de ligação, se pretende usá-lo.
  
## <a name="create-a-new-net-core-project"></a>Criar um novo projeto .NET Core

1. Abra uma linha de comandos e crie uma pasta com o nome **sqltest**. Navegue para esta pasta e execute este comando.

    ```cmd
    dotnet new console
    ```
    Este comando cria a nova aplicação de ficheiros de projeto, incluindo um inicial C# arquivo de código (**Program.cs**), um arquivo de configuração XML (**sqltest. csproj**) e for necessário binários.

2. No editor de texto, abra **sqltest. csproj** e cole o seguinte XML entre o `<Project>` etiquetas. Esse XML adiciona `System.Data.SqlClient` como uma dependência.

    ```xml
    <ItemGroup>
        <PackageReference Include="System.Data.SqlClient" Version="4.6.0" />
    </ItemGroup>
    ```

## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar a base de dados do SQL

1. No editor de texto, abra **Program.cs**.

2. Substitua os conteúdos pelo código seguinte e adicione os valores adequados para o seu servidor, base de dados, nome de utilizador e palavra-passe.

> [!NOTE]
> Para utilizar uma cadeia de ligação do ADO.NET, substitua as 4 linhas no código de configuração do servidor, base de dados, nome de utilizador e palavra-passe com a linha abaixo. Na cadeia de caracteres, defina o nome de utilizador e palavra-passe.
>
>    `builder.ConnectionString="<your_ado_net_connection_string>";`

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

                builder.DataSource = "<your_server.database.windows.net>"; 
                builder.UserID = "<your_username>";            
                builder.Password = "<your_password>";     
                builder.InitialCatalog = "<your_database>";
         
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
            Console.WriteLine("\nDone. Press enter.");
            Console.ReadLine(); 
        }
    }
}
```

## <a name="run-the-code"></a>Executar o código

1. Na linha de comandos, execute os seguintes comandos.

   ```cmd
   dotnet restore
   dotnet run
   ```

2. Certifique-se de que as 20 linhas superiores são devolvidas.

   ```text
   Query data example:
   =========================================

   Road Frames HL Road Frame - Black, 58
   Road Frames HL Road Frame - Red, 58
   Helmets Sport-100 Helmet, Red
   Helmets Sport-100 Helmet, Black
   Socks Mountain Bike Socks, M
   Socks Mountain Bike Socks, L
   Helmets Sport-100 Helmet, Blue
   Caps AWC Logo Cap
   Jerseys Long-Sleeve Logo Jersey, S
   Jerseys Long-Sleeve Logo Jersey, M
   Jerseys Long-Sleeve Logo Jersey, L
   Jerseys Long-Sleeve Logo Jersey, XL
   Road Frames HL Road Frame - Red, 62
   Road Frames HL Road Frame - Red, 44
   Road Frames HL Road Frame - Red, 48
   Road Frames HL Road Frame - Red, 52
   Road Frames HL Road Frame - Red, 56
   Road Frames LL Road Frame - Black, 58
   Road Frames LL Road Frame - Black, 60
   Road Frames LL Road Frame - Black, 62

   Done. Press enter.
   ```
3. Escolher **Enter** para fechar a janela de aplicativo.

## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao .NET Core com Windows/Linux/macOS, utilizando a linha de comandos](/dotnet/core/tutorials/using-with-xplat-cli).
- Saiba como [ligar e consultar uma base de dados SQL do Azure com o .NET Framework e o Visual Studio](sql-database-connect-query-dotnet-visual-studio.md).  
- Saiba como [conceber a sua base de dados SQL do Azure primeiro com o SSMS](sql-database-design-first-database.md) ou [ criar uma base de dados SQL do Azure e estabelecer ligação com o C# e o ADO.NET](sql-database-design-first-database-csharp.md).
- Para obter mais informações sobre o .NET, veja a [Documentação .NET](https://docs.microsoft.com/dotnet/).
