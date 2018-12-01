---
title: Utilizar o PHP para consultar base de dados SQL do Azure | Documentos da Microsoft
description: Como utilizar o PHP para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções T-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: be3ac9fab6c89c65ad9673811e108cefe2c80d00
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724252"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Início Rápido: Utilizar o PHP para consultar uma base de dados SQL do Azure

Este artigo demonstra como usar [PHP](http://php.net/manual/en/intro-whatis.php) para ligar a uma base de dados SQL do Azure. Pode, em seguida, utilizar instruções T-SQL para consultar dados.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este exemplo, certifique-se de que tem os seguintes pré-requisitos:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- R [regra de firewall ao nível do servidor](sql-database-get-started-portal-firewall.md) para o endereço IP público do computador que está a utilizar

- Software relacionado com o PHP instalado para o seu sistema operativo:

    - **MacOS**, instale o Homebrew e o PHP, o controlador ODBC e o SQLCMD, em seguida, instale o controlador PHP para SQL Server. Veja os [Passos 1.2, 1.3 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).

    - **Ubuntu**, instale o PHP e outros pacotes necessários, em seguida, instale o controlador PHP para SQL Server. Veja os [Passos 1.2 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).

    - **Windows**, instalar o PHP para IIS Express e o Chocolatey, em seguida, instale o controlador ODBC e o SQLCMD. Veja os [Passos 1.2 e 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-database-connection"></a>Obter ligação de base de dados

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>Adicione o código para consultar base de dados

1. No seu editor de texto favorito, crie um novo ficheiro, *sqltest.php*.  

1. Substitua o respetivo conteúdo pelo seguinte código. Em seguida, adicione os valores adequados para o seu servidor, base de dados, utilizador e palavra-passe.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
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

1. No prompt de comando, execute a aplicação.

   ```bash
   php sqltest.php
   ```

1. Certifique-se de que as primeiras 20 linhas são devolvidas e feche a janela de aplicação.

## <a name="next-steps"></a>Passos Seguintes

- [Criar a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md)

- [Controlador Microsoft PHP para SQL Server](https://github.com/Microsoft/msphpsql/)

- [Report issues or ask questions](https://github.com/Microsoft/msphpsql/issues) (Comunicar problemas ou fazer perguntas)

- [Exemplo de lógica de repetição: ligar de forma resiliente ao SQL Server com PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
