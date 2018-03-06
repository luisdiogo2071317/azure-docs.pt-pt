---
title: Script da CLI do Azure - Transferir registos do servidor na Base de Dados do Azure para MySQL
description: Este script de exemplo da CLI mostra como ativar e transferir os registos de um servidor da Base de Dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 31dd89f3bf5f43f45979b9a3cc5dda5eea1e352d
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Ativar e transferir registos de consulta lenta de um servidor da Base de Dados do Azure para MySQL com a CLI do Azure
Este script de exemplo da CLI ativa e transfere os registos de consulta lenta de um único servidor da Base de Dados do Azure para MySQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão, executando `az --version`. Veja [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua <log_file_name> nos comandos `az monitor` pelo seu nome de ficheiro de registo do servidor.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=18-19 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#az_msql_server_create) | Cria um servidor MySQL que aloja as bases de dados. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Liste os valores de configuração para um servidor. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Atualize a configuração de um servidor. |
| [az mysql server-logs list](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Liste os ficheiros de registo para um servidor. |
| [az mysql server-logs download](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Descarregue ficheiros de registo. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure/overview).
- Experimente scripts adicionais: [Exemplos da CLI do Azure para a Base de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
