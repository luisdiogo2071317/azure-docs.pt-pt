---
title: Como informação do Estado e restaurar na base de dados do Azure para PostgreSQL
description: Descreve como extrair uma base de dados PostgreSQL para um ficheiro de captura e restauro de um ficheiro criado pelo pg_dump na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 586df8d72dc05104bbf589eabcf3bd2245c268c8
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737253"
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

> [!IMPORTANT]
> Copie os ficheiros de cópia de segurança para um blob do Azure/arquivo e efetuar o restauro a partir daí, o que deve ser muito mais rápida do que efetuar o restauro através da Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Restaurar os dados para o base de dados do Azure de destino para PostrgeSQL utilizando pg_restore
Assim que tiver criado a base de dados de destino, pode utilizar o comando pg_restore e -d, parâmetro – dbname para restaurar os dados na base de dados de destino do ficheiro de informação.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Incluindo as – causas de parâmetro de proprietário não todos os objetos criados durante o restauro para ser detidos pelo utilizador especificado com - username. Para obter mais informações, consulte a documentação oficial do PostgreSQL no [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

Neste exemplo, restaure os dados a partir do ficheiro de informação **testdb.dump** na base de dados **mypgsqldb** no servidor de destino **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Passos Seguintes
- Para migrar uma base de dados PostgreSQL exportação e importação a utilizar, consulte [migrar a base de dados PostgreSQL utilizando a exportação e importação](howto-migrate-using-export-and-import.md).
- Para obter mais informações sobre como migrar bases de dados para a base de dados do Azure para PostgreSQL, consulte o [guia de migração de base de dados](http://aka.ms/datamigration).
