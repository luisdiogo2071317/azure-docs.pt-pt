---
title: Exemplo do Script da CLI do Azure - Conjunto do Windows no Batch | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Criar e gerir um conjunto do Windows no Batch
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: 0d9c757925b26e9f46afbd075184f853efa25dc3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598361"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>Exemplo da CLI: Criar e gerir um conjunto do Windows no Azure Batch

Este script demonstra alguns dos comandos disponíveis na CLI do Azure para criar e gerir um conjunto de nós de computação do Windows no Azure Batch. Pode configurar um conjunto do Windows de duas formas, com uma configuração dos Serviços Cloud ou da Máquina Virtual. Este exemplo mostra como criar um conjunto do Windows com a configuração dos Serviços Cloud.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

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
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Autentica contra a conta do Batch especificada para obter mais interação da CLI. |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Cria um conjunto de nós de computação.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Atualiza as propriedades de um conjunto.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Permite o dimensionamento automático num conjunto e aplica uma fórmula.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Apresenta as propriedades de um conjunto.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Desativa o dimensionamento automático num conjunto. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |


## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).
