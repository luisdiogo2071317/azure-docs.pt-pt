---
title: "Script CLI do Azure: Altere as configurações do servidor"
description: "Este script de exemplo do CLI apresenta uma lista de todas as opções de configuração de servidor disponível e o valor de uma das opções de atualizações."
services: postgres
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 334322557c83d5576c9a11def6bb0dc0e3ad1fec
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Configurações de lista e a atualização de uma base de dados do Azure para o servidor de PostgreSQL utilizando a CLI do Azure
Este script de exemplo do CLI apresenta uma lista de todos os parâmetros de configuração disponíveis, bem como os respetivos valores permitidos para a base de dados do Azure para o servidor de PostgreSQL e define o *log_retention_days* para um valor que é diferente da predefinição um.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo
Este script de exemplo, altere as linhas realçadas para personalizar o nome de utilizador de administrador e a palavra-passe.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados à mesma.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação de script
Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) | Cria um servidor de PostgreSQL que aloja as bases de dados. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Lista as configurações de uma base de dados do Azure para o servidor de PostgreSQL. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Atualize a configuração de uma base de dados do Azure para o servidor de PostgreSQL. |
| [az postgres server configuration show](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) | Mostra a configuração de uma base de dados do Azure para o servidor de PostgreSQL. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
- Leia mais informações sobre a CLI do Azure: [documentação da CLI do Azure](/cli/azure/overview).
- Tente scripts adicionais: [exemplos da CLI do Azure para a base de dados do Azure para PostgreSQL](../sample-scripts-azure-cli.md)
- Para obter mais informações sobre os parâmetros de servidor, consulte [como para configurar parâmetros de servidor no portal do Azure](../howto-configure-server-parameters-using-portal.md).
