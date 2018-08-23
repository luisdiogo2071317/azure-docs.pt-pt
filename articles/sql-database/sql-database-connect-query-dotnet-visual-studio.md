---
title: Utilizar o .Visual Studio e o .NET para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra-lhe como utilizar o Visual Studio para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções Transact-SQL.
services: sql-database
ms.workload: azure-vs
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: devcenter, vs-azure
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: ceb7d64ddd1923cdd22428df6cbc86676b685b4f
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42147098"
---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>Utilizar o .NET (C#) com o Visual Studio para ligar e consultar uma base de dados SQL do Azure

Este início rápido demonstra como utilizar o [.NET Framework](https://www.microsoft.com/net/) para criar um programa C# com o Visual Studio para ligar a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, certifique-se de que tem o seguinte:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Uma [regra de firewall ao nível do servidor](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) para o endereço IP público do computador que utilizar para este início rápido.

- Uma instalação do [Visual Studio Community 2017, Visual Studio Professional 2017 ou Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>Informações de ligação do servidor SQL

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

#### <a name="for-adonet"></a>Para ADO.NET

1. Continue ao clicar em **Mostrar cadeias de ligação da base de dados**.

2. Reveja a cadeia de ligação **ADO.NET** completa.

    ![Cadeia de ligação de ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Tem de ter implementada uma regra de firewall para o endereço IP público do computador no qual executar este tutorial. Se estiver num computador diferente ou tiver um endereço IP público diferente, crie uma [regra de firewall ao nível do servidor com o portal do Azure](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Criar um novo projeto do Visual Studio

1. No Visual Studio, escolha **Ficheiro**, **Novo**, **Projeto**. 
2. Na caixa de diálogo **Novo Projeto**, expanda **Visual C#**.
3. Selecione **Aplicação de Consola** e introduza *sqltest* no nome do projeto.
4. Clicar em **OK** para criar e abrir o novo projeto no Visual Studio
4. No Explorador de Soluções, clique com o botão direito do rato em **sqltest** e clique em **Gerir Pacotes NuGet**. 
5. Em **Procurar**, procure ```System.Data.SqlClient``` e, quando o encontrar, selecione-o.
6. Na página **System.Data.SqlClient**, clique em **Instalar**.
7. Quando a instalação estiver concluída, reveja as alterações e, em seguida, clique em **OK** para fechar a janela **Pré-visualização**. 
8. Se for apresentada uma janela **Aceitação de Licença**, clique em **Aceito**.

## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar a base de dados do SQL
1. Mudar para a (ou abrir se necessário) **Program.cs**

2. Substitua os conteúdos de **Program.cs** pelo código seguinte e adicione os valores corretos para o seu servidor, a sua base de dados, o seu utilizador e a sua palavra-passe.

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
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

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

1. Prima **F5** para executar a aplicação.
2. Confirme que as 20 linhas superiores são devolvidas e, em seguida, feche a janela da aplicação.

## <a name="next-steps"></a>Passos seguintes

- Aprenda a [ligar e consultar uma base de dados SQL do Azure com o .NET core](sql-database-connect-query-dotnet-core.md) no Linux/Windows/macOS.  
- Saiba mais sobre a [Introdução ao .NET Core com Windows/Linux/macOS, utilizando a linha de comandos](/dotnet/core/tutorials/using-with-xplat-cli).
- Aprenda a [Criar a sua primeira base de dados SQL do Azure com o SSMS](sql-database-design-first-database.md) ou a [Criar a sua primeira base de dados SQL do Azure com o .NET](sql-database-design-first-database-csharp.md).
- Para obter mais informações sobre o .NET, veja a [Documentação .NET](https://docs.microsoft.com/dotnet/).
- [Exemplo de lógica de repetição: ligar de forma resiliente ao SQL Server com o ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

