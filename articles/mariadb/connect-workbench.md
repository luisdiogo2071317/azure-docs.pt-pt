---
title: Ligar à Azure Database for MariaDB a partir de MySQL Workbench
description: Este Início Rápido indica os passos para utilizar o MySQL Workbench para se ligar e consultar dados da Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: b3a4d00381361b5299e86b959d9775318ae81e88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998251"
---
# <a name="azure-database-for-mariadb-use-mysql-workbench-to-connect-and-query-data"></a>Azure Database for MariaDB: Utilizar MySQL Workbench para se ligar e consultar dados
Este início rápido demonstra como se pode ligar a uma Azure Database for MariaDB através da aplicação MySQL Workbench. 

## <a name="prerequisites"></a>Pré-requisitos
Este guia de início rápido utiliza os recursos criados em qualquer um destes guias como ponto de partida:
- [Criar uma Azure Database for MariaDB Server através do portal do Azure](./quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Criar uma Azure Database for MariaDB Server através da CLI do Azure](./quickstart-create-mariadb-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>Instalar MySQL Workbench
Transfira e instale o MySQL Workbench no seu computador a partir do [site do MySQL](https://dev.mysql.com/downloads/workbench/).

## <a name="get-connection-information"></a>Obter informações da ligação
Obtenha as informações de ligação necessárias para se ligar à Azure Database for MariaDB. Necessita do nome do servidor e das credenciais de início de sessão totalmente qualificados.

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).

2. No menu esquerdo do portal do Azure, clique em **Todos os recursos** e, em seguida, procure o servidor que acabou de criar, (por exemplo, **mydemoserver**).

3. Clique no nome do servidor.

4. No painel **Descrição geral** do servidor, tome nota do **Nome do servidor** e do **Nome de início de sessão de administrador do servidor**. Caso se esqueça da sua palavra-passe, também pode repor a palavra-passe neste painel.
 ![Nome da Azure Database for MariaDB server](./media/connect-workbench/1_server-overview-name-login.png)

## <a name="connect-to-server-using-mysql-workbench"></a>Ligar ao servidor através de MySQL Workbench 
Para se ligar a Azure Database for MariaDB Server através de MySQL Workbench, utilize o formato seguinte:

1.  Inicie a aplicação MySQL Workbench no computador. 

2.  Na caixa de diálogo **Configurar Ligação Nova**, introduza as informações seguintes no separador **Parâmetros**:

    ![configurar ligação nova](./media/connect-workbench/2-setup-new-connection.png)

    | **Definição** | **Valor sugerido** | **Descrição do campo** |
    |---|---|---|
    |   Nome da Ligação | Ligação de Demonstração | Especifique uma etiqueta para esta ligação. |
    | Método de Ligação | Standard (TCP/IP) | Standard (TCP/IP) é suficiente. |
    | Nome de anfitrião | *server name* | Especifique o valor de nome de servidor que foi utilizado quando criou a Azure Database for MariaDB anteriormente. O nosso servidor de exemplo é mydemoserver.mariadb.database.azure.com. Utilize o nome de domínio completamente qualificado (\*.mariadb.database.azure.com), conforme mostrado no exemplo. Siga os passos na secção anterior para obter as informações da ligação, se não se lembrar do nome do servidor.  |
    | Porta | 3306 | Utilize sempre a porta 3306 para se ligar à Azure Database for MariaDB. |
    | Nome de utilizador |  *nome de início de sessão de administrador do servidor* | Introduza o nome de utilizador de início de sessão de administrador do servidor que foi fornecido quando criou a Azure Database for MariaDB anteriormente. O nosso nome de utilizador de exemplo é myadmin@mydemoserver. Siga os passos na secção anterior para obter as informações da ligação, se não se lembrar do nome de utilizador. O formato é *username@servername*.
    | Palavra-passe | a sua palavra-passe | Clique no botão **Armazenar no Cofre...** para guardar a palavra-passe. |

3.   Clique em **Testar Ligação** para testar se todos os parâmetros estão configurados corretamente. 

4.   Clique em **OK** para guardar a ligação. 

5.   Na listagem das **Ligações do MySQL**, clique no mosaico correspondente ao seu servidor e, em seguida, aguarde que a ligação seja estabelecida.

        É aberto um novo separador do SQL com um editor em branco, onde pode escrever as suas consultas.
    
        > [!NOTE]
        > Por predefinição, a segurança da ligação SSL é necessária e imposta na Azure Database for MariaDB server. Apesar de, normalmente, não ser precisa nenhuma configuração adicional com certificados SSL para o MySQL Workbench ligar ao seu servidor, recomendamos vincular a certificação AC SSL com o MySQL Workbench. Se precisar de desativar o SSL, visite o portal do Azure e clique na página de segurança da Ligação para desativar o botão para ativar/desativar Impor ligação SLL.

## <a name="create-table-insert-read-update-and-delete-data"></a>Criar tabela, inserir, ler, atualizar e eliminar dados
1. Copie e cole o código de SQL de exemplo num separador de SQL em branco para ilustrar alguns dados de exemplo.

    Este código cria uma base de dados vazia com o nome quickstartdb e, em seguida, cria uma tabela de exemplo com o nome de inventário. Insere algumas linhas e, em seguida, lê as mesmas. Altera os dados com uma instrução de atualização e lê as linhas novamente. Por fim, elimina uma linha e, em seguida, lê as linhas novamente.
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    A captura de ecrã mostra um exemplo do código de SQL no SQL Workbench e o resultado após ter sido executado.
    
    ![Separador do SQL MySQL Workbench para executar o código do SQL de exemplo](media/connect-workbench/3-workbench-sql-tab.png)

2. Para executar o código do SQL de exemplo, clique no ícone de relâmpago na barra de ferramentas do separador **Ficheiro SQL**.
3. Repare nos três resultados em separadores na secção **Grelha de Resultado** no meio da página. 
4. Repare na lista de **Resultados** na parte inferior da página. É apresentado o estado de cada comando. 

Agora, ligou-se à Azure Database for MariaDB com o MySQL Workbench e consultou dados com linguagem SQL.

<!--
## Next steps
> [!div class="nextstepaction"]
> [Migrate your database using Export and Import](./concepts-migrate-import-export.md)
-->