---
title: Script da CLI do Azure - restaurar uma base de dados do Azure para o servidor de MariaDB
description: Este script da CLI do Azure de exemplo mostra como restaurar uma base de dados do Azure para MariaDB server e respetivas bases de dados para um ponto anterior no tempo.
services: mariadb
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: 5c7e3f96488ef5142c19920b7f14282fe3c89b9a
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585321"
---
# <a name="restore-an-azure-database-for-mariadb-server-using-azure-cli"></a>Restaurar uma base de dados do Azure para o servidor de MariaDB com a CLI do Azure
Este script da CLI de exemplo restaura uma única base de dados do Azure para MariaDB server para um ponto anterior no tempo.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão ao executar `az --version`. Veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua o ID de subscrição utilizado nos comandos `az monitor` pelo seu ID da subscrição.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/backup-restore-pitr/backup-restore.sh?highlight=15-16 "Restore Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/backup-restore-pitr/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [Criar servidor do AZ mariadb](/cli/azure/mariadb/server#az-mariadb-server-create) | Cria um servidor de MariaDB que aloja as bases de dados. |
| [Restauro do servidor AZ mariadb](/cli/azure/mariadb/server#az-mariadb-server-restore) | Restaure um servidor da cópia de segurança. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [exemplos da CLI do Azure para a base de dados do Azure para MariaDB](../sample-scripts-azure-cli.md)
