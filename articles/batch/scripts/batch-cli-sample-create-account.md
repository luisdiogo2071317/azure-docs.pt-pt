---
title: "Exemplo do Script da CLI do Azure - Criar conta do Batch - serviço do Batch | Microsoft Docs"
description: "Exemplo de Script da CLI do Azure - Criar uma conta do Batch no modo de serviço do Batch"
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
ms.openlocfilehash: ced93032203c33dc4cda362d30192ee8eb37d944
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="cli-example-create-a-batch-account-in-batch-service-mode"></a>Exemplo da CLI: Criar uma conta do Batch no modo de serviço do Batch

Este script cria uma conta do Azure Batch no modo de serviço do Batch e mostra como consultar ou atualizar várias propriedades da conta. Quando cria uma conta do Batch no modo de serviço do Batch predefinido, os respetivos nós de computação são atribuídos internamente pelo serviço do Batch. Os nós de computação alocados estão sujeitos a uma quota de vCPU (núcleo) separada e a conta pode ser autenticada através de credenciais de chave partilhada ou um token do Azure Active Directory.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

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
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Cria uma conta de armazenamento. |
| [az batch account set](/cli/azure/batch/account#az_batch_account_set) | Atualiza propriedades da conta do Batch.  |
| [az batch account show](/cli/azure/batch/account#az_batch_account_show) | Obtém os detalhes da conta do Batch especificada.  |
| [az batch account keys list](/cli/azure/batch/account/keys#az_batch_account_keys_list) | Obtém as chaves de acesso da conta do Batch especificada.  |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Autentica contra a conta do Batch especificada para obter mais interação da CLI.  |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).
