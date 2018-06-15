---
title: Criar utilizadores na base de dados do Azure para o servidor de PostgreSQL
description: Este artigo descreve como pode criar novas contas de utilizador para interagir com uma base de dados do Azure para o servidor de PostgreSQL.
services: postgresql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 87a73929185112190d5dd6698e014db225ebc08e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
ms.locfileid: "29575640"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Criar utilizadores na base de dados do Azure para o servidor de PostgreSQL 
Este artigo descreve como pode criar utilizadores numa base de dados do Azure para o servidor de PostgreSQL.

## <a name="the-server-admin-account"></a>A conta de administrador do servidor
Quando criou a sua base de dados do Azure pela primeira vez para PostgreSQL, que forneceu um nome de utilizador de administrador do servidor e a palavra-passe. Para obter mais informações, pode seguir o [início rápido](quickstart-create-server-database-portal.md) para ver a abordagem de passo a passo. Uma vez que o nome de utilizador de administrador do servidor é um nome personalizado, pode localizar o nome de utilizador de administrador de servidor escolhido do portal do Azure.

A base de dados do Azure para o servidor de PostgreSQL é criado com as funções de 3 predefinidas definidas. Pode ver estas funções executando o comando: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- o utilizador de administrador do servidor

O utilizador de administrador do servidor é um membro da função azure_pg_admin. No entanto, a conta de administrador do servidor não faz parte da função azure_superuser. Uma vez que este serviço é um serviço gerido de PaaS, Microsoft apenas faz parte da função de utilizador super. 

PostgreSQL utiliza o motor privilégios para controlar o acesso a objetos de base de dados, tal como explicado o [documentação do produto PostgreSQL](https://www.postgresql.org/docs/current/static/sql-createrole.html). Na base de dados do Azure para PostgreSQL, o utilizador de administrador do servidor é concedido esses privilégios: início de sessão, NOSUPERUSER, HERDAR, CREATEDB, CREATEROLE, NOREPLICATION

A conta de utilizador de administrador do servidor pode ser utilizada para criar utilizadores adicionais e conceder aos utilizadores para a função de azure_pg_admin. Além disso, a conta de administrador do servidor pode ser utilizada para criar com menos privilégios de utilizadores e funções que tenham acesso a bases de dados individuais e esquemas.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Como criar utilizadores administradores adicionais na base de dados do Azure para PostgreSQL
1. Obter o nome de utilizador de admin e informações da ligação.
   Para ligar ao seu servidor de bases de dados, precisa do nome completo do servidor e das credenciais de início de sessão de administrador. Pode localizar facilmente o nome do servidor e informações de início de sessão do servidor **descrição geral** página ou o **propriedades** página no portal do Azure. 

2. Utilize a conta de administrador e a palavra-passe para ligar ao seu servidor de base de dados. Utilize a ferramenta de cliente preferencial, tais como pgAdmin ou psql.
   Se não souber como ligar, consulte [ligar à base de dados PostgreSQL utilizando psql na Shell de nuvem](./quickstart-create-server-database-portal.md#connect-to-the-postgresql-database-by-using-psql-in-cloud-shell)

3. Edite e execute o seguinte código do SQL Server. Substitua o novo nome de utilizador para o valor do marcador de posição < new_user > e substitua a palavra-passe do marcador de posição pela sua própria palavra-passe segura. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Como criar utilizadores de base de dados na base de dados do Azure para PostgreSQL

1. Obter o nome de utilizador de admin e informações da ligação.
   Para ligar ao seu servidor de bases de dados, precisa do nome completo do servidor e das credenciais de início de sessão de administrador. Pode localizar facilmente o nome do servidor e informações de início de sessão do servidor **descrição geral** página ou o **propriedades** página no portal do Azure. 

2. Utilize a conta de administrador e a palavra-passe para ligar ao seu servidor de base de dados. Utilize a ferramenta de cliente preferencial, tais como pgAdmin ou psql.

3. Edite e execute o seguinte código do SQL Server. Substitua o valor do marcador de posição `<db_user>` com os seus pretendido novo nome de utilizador e o valor do marcador de posição `<newdb>` com o seu próprio nome de base de dados. Substitua a palavra-passe do marcador de posição pela sua própria palavra-passe segura. 

   Esta sintaxe de código do SQL Server cria uma nova base de dados com o nome testdb, para fins de exemplo. Em seguida, cria um novo utilizador no serviço PostgreSQL e atribui ligar privilégios para a nova base de dados para esse utilizador. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE testdb TO <db_user>;
   ```

4. Utilizar uma conta de administrador, poderá ter de conceder privilégios adicionais para proteger os objetos na base de dados. Consulte o [PostgreSQL documentação](https://www.postgresql.org/docs/current/static/ddl-priv.html) para obter mais detalhes nas funções de base de dados e privilégios. Por exemplo: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Inicie sessão no seu servidor, especificar a base de dados designado, utilizando o novo nome de utilizador e palavra-passe. Este exemplo mostra a linha de comandos psql. Com este comando, é-lhe pedida a palavra-passe para o nome de utilizador. Substitua o seu próprio nome do servidor, o nome de base de dados e o nome de utilizador.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Passos Seguintes
Abra a firewall para os endereços IP de máquinas de novos utilizadores para permitir que os mesmos se liguem: [criar e gerir o Azure base de dados PostgreSQL das regras de firewall com o portal do Azure](howto-manage-firewall-using-portal.md) ou [CLI do Azure](howto-manage-firewall-using-cli.md).

Para obter mais informações sobre a gestão de contas de utilizador, consulte a documentação do produto PostgreSQL para [funções de base de dados e privilégios](https://www.postgresql.org/docs/current/static/user-manag.html), [conceder sintaxe](https://www.postgresql.org/docs/current/static/sql-grant.html), e [privilégios](https://www.postgresql.org/docs/current/static/ddl-priv.html).
