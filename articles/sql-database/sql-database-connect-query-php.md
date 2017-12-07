---
title: Utilizar o PHP para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: "Este tópico mostra-lhe como utilizar o PHP para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: quickstart
ms.date: 11/29/2017
ms.author: carlrab
ms.openlocfilehash: b45acf8a7abdee070c6db2c5d7f4c108a073b1bb
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2017
---
# <a name="use-php-to-query-an-azure-sql-database"></a>Utilizar o PHP para consultar uma base de dados SQL do Azure

Este tutorial de início rápido demonstra como utilizar o [PHP](http://php.net/manual/en/intro-whatis.php) para criar um programa que se liga a uma base de dados SQL do Azure e como utilizar instruções Transact-SQL para consultar dados.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial de início rápido, certifique-se de que tem o seguinte:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Uma [regra de firewall ao nível do servidor](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) para o endereço IP público do computador que utilizar para este tutorial de início rápido.

- Ter instalado o PHP e software relacionado para o seu sistema operativo:

    - **MacOS**: instale o Homebrew e o PHP, instale o controlador ODBC e o SQLCMD e, em seguida, instale o Controlador PHP para SQL Server. Veja os [Passos 1.2, 1.3 e 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).
    - **Ubuntu**: instale o PHP e outros pacotes necessários e, em seguida, instale o Controlador do PHP para SQL Server. Veja os [Passos 1.2 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: instale a mais recente versão do PHP para IIS Express, a versão mais recente do Microsoft Drivers for SQL Server no IIS Express, o Chocolatey, o controlador ODBC e o SQLCMD. Veja os [Passos 1.2 e 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>Informações de ligação do servidor SQL

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar a base de dados do SQL

1. No seu editor de texto favorito, crie um novo ficheiro, **sqltest.php**.  

2. Substitua os conteúdos pelo código seguinte e adicione os valores corretos para o seu servidor, a sua base de dados, o seu utilizador e a sua palavra-passe.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Executar o código

1. Na linha de comandos, execute os comandos seguintes:

   ```php
   php sqltest.php
   ```

2. Confirme que as 20 linhas superiores são devolvidas e, em seguida, feche a janela da aplicação.

## <a name="next-steps"></a>Passos seguintes
- [Criar a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md)
- [Controlador Microsoft PHP para SQL Server](https://github.com/Microsoft/msphpsql/)
- [Report issues or ask questions](https://github.com/Microsoft/msphpsql/issues) (Comunicar problemas ou fazer perguntas)
- [Exemplo de lógica de repetição: ligar de forma resiliente ao SQL com o PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

