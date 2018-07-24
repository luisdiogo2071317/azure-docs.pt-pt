---
title: Como capturar e restaurar na base de dados do Azure para PostgreSQL
description: Descreve como extrair uma base de dados do PostgreSQL para um arquivo de despejo e restaurar a partir de um arquivo criado por pg_dump na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213655"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrar a sua base de dados do PostgreSQL com a captura e restauro
Pode usar [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) para extrair um banco de dados do PostgreSQL para um arquivo de despejo e [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) para restaurar a base de dados do PostgreSQL a partir de um ficheiro de arquivo criado pelo pg_dump.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, terá de:
- Uma [base de dados do Azure para o servidor PostgreSQL](quickstart-create-server-database-portal.md) com regras de firewall para permitir o acesso e a base de dados sob a mesma.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) e [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) utilitários da linha de comandos instalados

Siga estes passos para cópia de segurança e restaurar a base de dados do PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Criar um arquivo de despejo com pg_dump que contém os dados a ser carregado
Para fazer uma cópia de segurança uma existente PostgreSQL da base de dados no local ou numa VM, execute o seguinte comando:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Por exemplo, se tiver um servidor local e uma base de dados chamado **testdb** nela
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Copie os ficheiros de cópia de segurança para um blob do Azure/armazenamento e efetuar o restauro a partir daí, o que deve ser muito mais rápido do que efetuar o restauro na Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Restaurar os dados para o base de dados do Azure de destino para PostrgeSQL usando pg_restore
Depois de criar a base de dados de destino, pode utilizar o comando de pg_restore e -d, parâmetro – dbname para restaurar os dados no banco de dados de destino do ficheiro de informação.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Incluindo o-- parâmetro de proprietário não faz com que, todos os objetos criados durante o restauro que é detido pelo utilizador especificado com – o nome de utilizador. Para obter mais informações, consulte a documentação oficial do PostgreSQL no [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Se o servidor PostgreSQL exigir ligações SSL (por predefinição na base de dados do Azure para servidores PostgreSQL), defina uma variável de ambiente `PGSSLMODE=require` para que a ferramenta de pg_restore liga-se com SSL. Sem SSL, pode ler o erro  `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Na linha de comando do Windows, execute o comando `SET PGSSLMODE=require` antes de executar o comando pg_restore. No Linux ou Bash, execute o comando `export PGSSLMODE=require` antes de executar o comando pg_restore.
>

Neste exemplo, restaurar os dados a partir do arquivo de despejo **testdb.dump** na base de dados **mypgsqldb** no servidor de destino **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Passos Seguintes
- Para migrar uma base de dados do PostgreSQL com exportar e importar, consulte [migrar a sua base de dados do PostgreSQL com exportar e importar](howto-migrate-using-export-and-import.md).
- Para obter mais informações sobre como migrar bases de dados para a base de dados do Azure para PostgreSQL, veja a [guia de migração de bases de dados](http://aka.ms/datamigration).
