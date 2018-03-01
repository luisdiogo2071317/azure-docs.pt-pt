---
title: Exemplo do Script da CLI do Azure - Conjunto do Linux no Batch | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Criar e gerir um conjunto do Linux no Batch
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: f0e6ca88217cf7acbfa2478bf3eba1f52d83001c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="cli-example-create-and-manage-a-linux-pool-in-azure-batch"></a>Exemplo da CLI: Criar e gerir um conjunto do Linux no Azure Batch

Este script demonstra alguns dos comandos disponíveis na CLI do Azure para criar e gerir um conjunto de nós de computação do Linux no Azure Batch.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este início rápido requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Cria a conta do Batch. |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Autentica contra a conta do Batch especificada para obter mais interação da CLI.  |
| [az batch pool node-agent-skus list](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#az_batch_pool_node_agent_skus_list) | Lista os SKUs do agente de nó e as informações da imagem disponíveis.  |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Cria um conjunto de nós de computação.  |
| [az batch pool resize](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_resize) | Redimensiona o número de VMs em execução no conjunto especificado.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Apresenta as propriedades de um conjunto.  |
| [az batch node list](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_list) | Lista todos os nós de computação no conjunto especificado.  |
| [az batch node reboot](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_reboot) | Reinicia o nó de computação especificado.  |
| [az batch node delete](https://docs.microsoft.com/cli/azure/batch/node#az_batch_node_delete) | Elimina os nós listados do conjunto especificado.  |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).
