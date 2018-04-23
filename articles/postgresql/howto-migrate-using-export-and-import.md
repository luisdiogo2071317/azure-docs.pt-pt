---
title: Migrar uma base de dados utilizando a importação e exportação na base de dados do Azure para PostgreSQL
description: Descreve como extrair uma base de dados PostgreSQL para um ficheiro de script e importar os dados para a base de dados de destino do que o ficheiro.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 8726badde2214a0904336f5bc73310114bcf9e91
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migrar a base de dados PostgreSQL utilizando a exportação e importação
Pode utilizar [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) para extrair uma base de dados PostgreSQL para um ficheiro de script e [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) para importar os dados para a base de dados de destino do que o ficheiro.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, tem de:
- Um [base de dados do Azure para o servidor de PostgreSQL](quickstart-create-server-database-portal.md) com regras de firewall para permitir o acesso e a base de dados sob a mesma.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) instalado o utilitário da linha de comandos
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) instalado o utilitário da linha de comandos

Siga estes passos para exportar e importar a base de dados PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Crie um ficheiro de script utilizando pg_dump que contém os dados para ser carregado
Para exportar existente PostgreSQL da base de dados no local ou numa VM a um ficheiro de script de sql, execute o seguinte comando no seu ambiente existente:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Por exemplo, se tiver um servidor local e uma base de dados chamado **testdb** no mesmo:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Importar os dados na base de dados do Azure de destino para PostgreSQL
Pode utilizar a linha de comandos psql e o parâmetro – dbname (-d) para importar os dados para a base de dados do Azure para dados de servidor e a carga de PostgreSQL no ficheiro de sql.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Este exemplo utiliza o utilitário de psql e um ficheiro de script com o nome **testdb.sql** do passo anterior para importar dados para a base de dados **mypgsqldb** no servidor de destino  **mydemoserver.postgres.Database.Azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Passos Seguintes
- Para migrar uma base de dados PostgreSQL através de captura e restauro, consulte [migrar a base de dados PostgreSQL através de captura e restauro](howto-migrate-using-dump-and-restore.md)
