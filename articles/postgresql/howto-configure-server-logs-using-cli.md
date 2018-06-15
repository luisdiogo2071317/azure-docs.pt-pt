---
title: Configurar e registos do servidor de acesso para PostgreSQL utilizando a CLI do Azure
description: Este artigo descreve como configurar e aceder os registos do servidor na base de dados do Azure para PostgreSQL utilizando a linha de comandos da CLI do Azure.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 951dcca562c08698b4ce4528d005fc91152ea337
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
ms.locfileid: "30235537"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configurar e iniciar sessão no servidor de acesso utilizando a CLI do Azure
Pode transferir os registos de erros de servidor PostgreSQL utilizando a interface de linha de comandos (CLI do Azure). No entanto, o acesso aos registos de transações não é suportado. 

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, tem de:
- [Base de dados do Azure para o servidor de PostgreSQL](quickstart-create-server-database-azure-cli.md)
- O [Azure CLI 2.0](/cli/azure/install-azure-cli) utilitário da linha de comandos ou a Shell de nuvem do Azure no browser

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Configurar o registo da base de dados do Azure para PostgreSQL
Pode configurar o servidor para aceder a registos de consulta e registos de erros. Os registos de erros podem ter informações vacuum automática, a ligação e o ponto de verificação.
1. Ative o registo.
2. Para ativar o registo de consultas, atualizar **registo\_instrução** e **registo\_min\_duração\_instrução**.
3. Período de retenção de atualização.

Para obter mais informações, consulte [personalizar parâmetros de configuração do servidor](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Registos de lista da base de dados do Azure para o servidor de PostgreSQL
Para listar os ficheiros de registo disponível para o servidor, execute o [lista de registos do servidor de postgres az](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) comando.

Pode listar os ficheiros de registo para o servidor **mydemoserver.postgres.database.azure.com** sob o grupo de recursos **myresourcegroup**. Em seguida, direcionar a lista de ficheiros de registo para um ficheiro de texto chamado **registo\_ficheiros\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Transferir os registos do servidor, localmente
Com o [az postgres-registos do servidor transferir](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) comando, pode transferir os ficheiros de registo individuais para o servidor. 

Utilize o seguinte exemplo para transferir o ficheiro de registo específicos para o servidor **mydemoserver.postgres.database.azure.com** sob o grupo de recursos **myresourcegroup** ao seu ambiente local.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Passos Seguintes
- Para saber mais sobre os registos do servidor, consulte [registos do servidor na base de dados do Azure para PostgreSQL](concepts-server-logs.md).
- Para obter mais informações sobre os parâmetros de servidor, consulte [personalizar parâmetros de configuração de servidor utilizando a CLI do Azure](howto-configure-server-parameters-using-cli.md).
