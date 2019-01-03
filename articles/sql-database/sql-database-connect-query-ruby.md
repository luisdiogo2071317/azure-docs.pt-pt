---
title: Utilizar Ruby para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra-lhe como utilizar Ruby para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 66819cbd65f6f044d0dac68326eb5890476964b6
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993914"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Início rápido: Utilizar Ruby para consultar uma base de dados SQL do Azure

Este início rápido demonstra como usar [Ruby](https://www.ruby-lang.org) para ligar a uma data de base de dados e consulta de SQL do Azure com instruções Transact-SQL.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, terá dos seguintes pré-requisitos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Uma [regra de firewall ao nível do servidor](sql-database-get-started-portal-firewall.md) para o endereço IP público do computador que utilizar para este início rápido.
  
- Software de Ruby e relacionado para o seu sistema operativo:
  
  - **macOS**: Instale o Homebrew, o rbenv e o ruby-build, o Ruby, o FreeTDS e o TinyTDS. Veja os passos 1.2, 1.3, 1.4, 1,5 e 2.1 no [aplicações Ruby criar usando o SQL Server no macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Instale os pré-requisitos para Ruby, o rbenv e o ruby-build, o Ruby, o FreeTDS e o TinyTDS. Veja os passos 1.2, 1.3, 1.4, 1,5 e 2.1 no [aplicações Ruby criar usando o SQL Server no Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Instale Ruby, Ruby Devkit e TinyTDS. Ver [configurar o ambiente de desenvolvimento para o desenvolvimento de Ruby](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>Obter as informações de ligação do SQL server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-code-to-query-your-sql-database"></a>Criar código para consultar a base de dados SQL

1. Num editor de texto ou de código, crie um novo ficheiro designado *sqltest.rb*.
   
1. Adicione o seguinte código. Substitua os valores da sua base de dados SQL do Azure para `<server>`, `<database>`, `<username>`, e `<password>`.
   
   >[!IMPORTANT]
   >O código neste exemplo utiliza os dados de AdventureWorksLT de exemplo, o que pode escolher como origem ao criar a sua base de dados. Se a sua base de dados tiver dados diferentes, utilize a consulta SELECT tabelas da sua própria base de dados. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>Executar o código

1. No prompt de comando, execute o seguinte comando:

   ```bash
   ruby sqltest.rb
   ```
   
1. Certifique-se de que as linhas de categoria/produto de 20 melhores da sua base de dados são devolvidas. 

## <a name="next-steps"></a>Passos Seguintes
- [Conceber a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md).
- [Repositório do GitHub para TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Comunicar problemas ou fazer perguntas sobre o TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues).
- [Controladores Ruby para SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
