---
title: Script da CLI do Azure - Restaurar um servidor da Base de Dados do Azure para PostgreSQL
description: Este script de exemplo da CLI do Azure mostra como restaurar um servidor da Base de Dados do Azure para PostgreSQL e as respetivas bases de dados para um ponto anterior no tempo.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 391e3126fe3f4e02083e609b7e5f9bbe5f33605d
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "41918247"
---
# <a name="restore-an-azure-database-for-postgresql-server-using-azure-cli"></a>Restaurar um servidor da Base de Dados do Azure para PostgreSQL com a CLI do Azure
Este script da CLI de exemplo restaura um servidor único da Base de Dados do Azure para PostgreSQL para um ponto anterior no tempo.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão ao executar `az --version`. Veja [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure.

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua o ID de subscrição utilizado nos comandos `az monitor` pelo seu ID da subscrição.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/backup-restore.sh?highlight=18-19 "Restore Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az postgresql server create](/cli/azure/postgres/server#az-postgres-server-create) | Cria um servidor PostgreSQL que aloja as bases de dados. |
| [az postgresql server restore](/cli/azure/postgres/server#az-postgres-server-restore) | Restaure um servidor da cópia de segurança. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [Amostras da CLI do Azure para a Base de Dados do Azure para PostgreSQL](../sample-scripts-azure-cli.md)
- [Como criar uma cópia de segurança e restaurar um servidor na Base de Dados do Azure para PostgreSQL no portal do Azure](../howto-restore-server-portal.md)
