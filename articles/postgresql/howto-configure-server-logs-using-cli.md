---
title: Configurar e aceder aos registos de servidor para o PostgreSQL com a CLI do Azure
description: Este artigo descreve como configurar e aceder os registos do servidor na base de dados do Azure para PostgreSQL com a linha de comando da CLI do Azure.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: af5a0843acced4611b219088b7662f9ea4b32beb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969587"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configurar e aceder aos registos de servidor com a CLI do Azure
Pode transferir os registos de erros de servidor PostgreSQL, utilizando a interface de linha de comandos (CLI do Azure). No entanto, o acesso aos logs de transação não é suportado. 

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, terá de:
- [Base de dados do Azure para o servidor PostgreSQL](quickstart-create-server-database-azure-cli.md)
- O [CLI do Azure](/cli/azure/install-azure-cli) utilitário da linha de comandos ou o Azure Cloud Shell no browser

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Configurar o registo da base de dados do Azure para PostgreSQL
Pode configurar o servidor para aceder a registos de consulta e registos de erros. Registos de erros podem ter informações aspirador automática, ligação e ponto de verificação.
1. Ative o registo.
2. Para ativar o registo de consulta, atualize **log\_instrução** e **registo\_min\_duração\_instrução**.
3. Período de retenção de atualização.

Para obter mais informações, consulte [personalizar os parâmetros de configuração de servidor](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Lista de registos da base de dados do Azure para o servidor PostgreSQL
Para listar os ficheiros de registo disponíveis para o seu servidor, execute o [lista do az postgres server-logs](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) comando.

Pode listar os ficheiros de registo para o servidor **mydemoserver.postgres.database.azure.com** sob o grupo de recursos **myresourcegroup**. Em seguida, direcionar a lista de ficheiros de registo para um arquivo de texto chamado **log\_arquivos\_txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Transferir localmente os registos do servidor
Com o [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) de comando, pode transferir ficheiros de registo individuais para o seu servidor. 

Utilize o exemplo seguinte para transferir o ficheiro de registo específico para o servidor **mydemoserver.postgres.database.azure.com** sob o grupo de recursos **myresourcegroup** ao seu ambiente local.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Passos Seguintes
- Para saber mais sobre os registos do servidor, consulte [registos do servidor na base de dados do Azure para PostgreSQL](concepts-server-logs.md).
- Para obter mais informações sobre parâmetros do servidor, consulte [personalizar os parâmetros de configuração de servidor com a CLI do Azure](howto-configure-server-parameters-using-cli.md).
