---
title: CLI do Azure do script - criar uma base de dados do Azure para MariaDB
description: Este script da CLI de exemplo cria uma base de dados do Azure para MariaDB server e configura uma regra de firewall ao nível do servidor.
services: mariadb
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: 80609035933e45ea9b7cc911a908f6ed24b9f6a7
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585359"
---
# <a name="create-a-mariadb-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Criar um servidor da MariaDB e configurar uma regra de firewall com a CLI do Azure
Este script da CLI de exemplo cria uma base de dados do Azure para MariaDB server e configura uma regra de firewall ao nível do servidor. Assim que o script é executado com êxito, o servidor de MariaDB está acessível por todos os serviços do Azure e o endereço IP configurado.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão ao executar `az --version`. Veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure.

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/create-mariadb-server-and-firewall-rule/create-mariadb-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for mariadb, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/create-mariadb-server-and-firewall-rule/delete-mariadb.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [Criar servidor do AZ mariadb](/cli/azure/mariadb/server#az-mariadb-server-create) | Cria um servidor de MariaDB que aloja as bases de dados. |
| [criar a firewall do servidor AZ mariadb](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create) | Cria uma regra de firewall para permitir o acesso ao servidor e às bases de dados incluídas, a partir do intervalo de endereços IP introduzido. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [exemplos da CLI do Azure para a base de dados do Azure para MariaDB](../sample-scripts-azure-cli.md)
