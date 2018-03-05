---
title: "Como informação do Estado e restaurar na base de dados do Azure para PostgreSQL"
description: Descreve como extrair uma base de dados PostgreSQL para um ficheiro de captura e restauro de um ficheiro criado pelo pg_dump na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6ea839c10bffc9a024af38132081f2c9bd7dfc0a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrar a base de dados PostgreSQL através de captura e restauro
Pode utilizar [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) para extrair uma base de dados PostgreSQL para um ficheiro de informação e [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) para restaurar a base de dados PostgreSQL a partir de um ficheiro de arquivo criado pelo pg_dump.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, tem de:
- Um [base de dados do Azure para o servidor de PostgreSQL](quickstart-create-server-database-portal.md) com regras de firewall para permitir o acesso e a base de dados sob a mesma.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) e [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) utilitários da linha de comandos instalados

Siga estes passos para informação do Estado e restaurar a base de dados PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Crie um ficheiro de informação utilizando pg_dump que contém os dados para ser carregado
Para fazer uma cópia de segurança existente PostgreSQL da base de dados no local ou numa VM, execute o seguinte comando:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Por exemplo, se tiver um servidor local e uma base de dados chamado **testdb** no mesmo
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Restaurar os dados para o base de dados do Azure de destino para PostrgeSQL utilizando pg_restore
Assim que tiver criado a base de dados de destino, pode utilizar o comando pg_restore e -d, parâmetro – dbname para restaurar os dados na base de dados de destino do ficheiro de informação.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Neste exemplo, restaure os dados a partir do ficheiro de informação **testdb.dump** na base de dados **mypgsqldb** no servidor de destino **mydemoserver.postgres.database.azure.com**.
```bash
pg_restore -v --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Passos Seguintes
- Para migrar uma base de dados PostgreSQL exportação e importação a utilizar, consulte [migrar a base de dados PostgreSQL utilizando a exportação e importação](howto-migrate-using-export-and-import.md)