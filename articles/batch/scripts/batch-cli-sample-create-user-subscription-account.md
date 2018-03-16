---
title: "Exemplo de Script da CLI do Azure - Criar conta do Batch - subscrição do utilizador | Microsoft Docs"
description: "Exemplo de Script da CLI do Azure - Criar uma conta do Batch no modo de subscrição do utilizador"
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
ms.openlocfilehash: 0923512d62a1efe31e08377b6c074df2e0618552
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="cli-example-create-a-batch-account-in-user-subscription-mode"></a>Exemplo da CLI: Criar uma conta do Batch no modo de subscrição do utilizador

Este script cria uma conta do Azure Batch no modo de subscrição do utilizador. Uma conta que aloca nós de computação na sua subscrição tem de ser autenticada através de um token do Azure Active Directory. Os nós de computação alocados contam para a quota de vCPU (núcleo) da sua subscrição. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh "Create Account using user subscription")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az role assignment create](/cli/azure/role#az_role_assignment_create) | Crie uma nova atribuição de função para um utilizador, grupo ou principal de serviço. |
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Cria um cofre de chaves. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Atualize a política de segurança do cofre de chaves especificado. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Cria a conta do Batch.  |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Autentica contra a conta do Batch especificada para obter mais interação da CLI.  |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).
