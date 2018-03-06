---
title: Script da CLI do Azure - Restaurar um servidor da Base de Dados do Azure para MySQL para um ponto anterior no tempo
description: Este script de exemplo da CLI restaura um servidor da Base de Dados do Azure para MySQL, para um ponto anterior no tempo.
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
ms.openlocfilehash: 871e4f643313634a6d3d8234540724fb71b71625
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Restaurar um servidor da Base de Dados do Azure para MySQL com a CLI do Azure
Este script de exemplo da CLI restaura um único servidor da Base de Dados do Azure para MySQL para um ponto anterior no tempo.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este exemplo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, altere as linhas realçadas para personalizar o nome de utilizador administrador e a palavra-passe. Substitua o ID de subscrição utilizado nos comandos az monitor pelo seu ID da subscrição.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/backup-restore.sh?highlight=18-19 "Restore Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#create) | Cria um servidor MySQL que aloja as bases de dados. |
| [az mysql server restore](/cli/azure/mysql/server#restore) | Restaure um servidor da cópia de segurança. |
| [az group delete](/cli/azure/group#delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure/overview).
- Experimente scripts adicionais: [Exemplos da CLI do Azure para a Base de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
