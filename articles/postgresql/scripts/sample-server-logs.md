---
title: "Script CLI do Azure: os registos do servidor de transferência na base de dados do Azure para PostgreSQL"
description: Este script da CLI do Azure de exemplo mostra como ativar e transferir os registos do servidor de uma base de dados do Azure para o servidor de PostgreSQL.
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Ativar e transferir os registos de consulta lento do servidor de uma base de dados do Azure para o servidor de PostgreSQL utilizando a CLI do Azure
Este script de exemplo do CLI ativa e transfere os registos de consulta lenta de uma única base de dados do Azure para o servidor de PostgreSQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este exemplo requer que está a executar a CLI do Azure versão 2.0 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo
Este script de exemplo, altere as linhas realçadas para personalizar o nome de utilizador de administrador e a palavra-passe. Substitua < log_file_name > nos comandos monitor az seu próprio nome de ficheiro de registo do servidor.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados à mesma.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação de script
Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [az postgres server create](/cli/azure/postgres/server#az_msql_server_create) | Cria um servidor de PostgreSQL que aloja as bases de dados. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Liste os valores de configuração para um servidor. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Atualize a configuração de um servidor. |
| [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Lista os ficheiros de registo para um servidor. |
| [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Transferir ficheiros de registo. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
- Leia mais informações sobre a CLI do Azure: [documentação da CLI do Azure](/cli/azure/overview).
- Tente scripts adicionais: [exemplos da CLI do Azure para a base de dados do Azure para PostgreSQL](../sample-scripts-azure-cli.md)
- [Configurar e iniciar sessão no servidor de acesso no portal do Azure](../howto-configure-server-logs-in-portal.md)
