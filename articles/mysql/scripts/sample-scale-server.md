---
title: Script da CLI do Azure - Dimensionar um servidor da Base de Dados do Azure para MySQL
description: Este script de exemplo da CLI dimensiona o servidor da Base de Dados do Azure para MySQL para um nível de desempenho diferente depois de consultar as métricas.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 04/05/2018
ms.openlocfilehash: 8c5a196afbcc607300dd69ada0a06a292a9c2c0c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38635113"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Monitorizar e dimensionar um servidor da Base de Dados do Azure para MySQL com a CLI do Azure
Este script de exemplo da CLI dimensiona um único servidor da Base de Dados do Azure para MySQL para um nível de desempenho diferente depois de consultar as métricas.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão ao executar `az --version`. Veja [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus. Substitua o ID de subscrição utilizado nos comandos `az monitor` pelo seu id da subscrição. [!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=18-19 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#az_mysql_server_create) | Cria um servidor MySQL que aloja as bases de dados. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Liste o valor métrico dos recursos. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [Exemplos da CLI do Azure para a Base de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
- Para obter mais informações sobre como dimensionar, veja os [Escalões de Serviço](../concepts-service-tiers.md) e as [Unidades de Computação e Unidades de Armazenamento](../concepts-compute-unit-and-storage.md).
