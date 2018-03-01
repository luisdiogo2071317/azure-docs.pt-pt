---
title: "Exemplo do Script da CLI do Azure - Adicionar uma Aplicação no Batch | Microsoft Docs"
description: "Exemplo do Script da CLI do Azure - Adicionar uma Aplicação no Batch"
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
ms.openlocfilehash: 348e94e745350173196aeb64df3a814a05dd9144
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>Exemplo da CLI: Adicionar uma aplicação a uma conta do Azure Batch

Este script demonstra como adicionar uma aplicação para utilizar com um conjunto ou tarefas do Azure Batch. Para configurar uma aplicação para adicionar à sua conta do Batch, reúna o executável juntamente com quaisquer dependências num ficheiro zip. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos.
Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Cria uma conta de armazenamento. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Cria a conta do Batch. |
| [az batch account login](/cli/azure/batch/account#az_batch_account_login) | Autentica contra a conta do Batch especificada para obter mais interação da CLI.  |
| [az batch application create](/cli/azure/batch/application#az_batch_application_create) | Cria uma aplicação.  |
| [az batch application package create](/cli/azure/batch/application/package#az_batch_application_package_create) | Adiciona um pacote de aplicação à aplicação especificada.  |
| [az batch application set](/cli/azure/batch/application#az_batch_application_set) | Atualiza as propriedades de uma aplicação.  |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).
