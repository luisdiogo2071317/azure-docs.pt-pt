---
title: 'CLI do Azure: Transferir os registos do servidor na base de dados do Azure para MySQL'
description: Este script da CLI do Azure de exemplo mostra como ativar e transferir os registos do servidor de uma base de dados do Azure para o servidor de MySQL.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/11/2018
ms.openlocfilehash: b0d34009d189ab136dcb6f28fdccc49b6da9e108
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Ativar e transferir os registos de consulta lento do servidor de uma base de dados do Azure para o servidor de MySQL utilizando a CLI do Azure
Este script de exemplo do CLI ativa e transfere os registos de consulta lenta de uma única base de dados do Azure para o servidor de MySQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este exemplo requer que está a executar a CLI do Azure versão 2.0 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo
Este script de exemplo, altere as linhas realçadas para personalizar o nome de utilizador de administrador e a palavra-passe. Substitua < log_file_name > nos comandos monitor az seu próprio nome de ficheiro de registo do servidor.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados à mesma.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação de script
Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| **Comando** | **Notas** |
|---|---|
| [Criar grupo AZ](/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [Criar servidor do AZ mysql](/cli/azure/mysql/server#az_msql_server_create) | Cria um servidor de MySQL que aloja as bases de dados. |
| [lista de configuração de servidor do AZ mysql](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Liste os valores de configuração para um servidor. |
| [conjunto de configuração do servidor do AZ mysql](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Atualize a configuração de um servidor. |
| [lista de registos do servidor do AZ mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Lista os ficheiros de registo para um servidor. |
| [transferência de registos do servidor do AZ mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Transferir ficheiros de registo. |
| [eliminação do grupo de AZ](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
- Leia mais informações sobre a CLI do Azure: [documentação da CLI do Azure](/cli/azure/overview).
- Tente scripts adicionais: [amostras da CLI do Azure para a base de dados do Azure para MySQL](../sample-scripts-azure-cli.md)
