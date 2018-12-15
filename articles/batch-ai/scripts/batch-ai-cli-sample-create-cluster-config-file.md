---
title: Exemplo do Script da CLI do Azure - Criar um cluster do Batch AI com um ficheiro de configuração | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Criar um cluster do Batch AI ao especificar definições de configuração num ficheiro JSON.
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 08/16/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 41a3a801214ff00c01397034e26fde6946ab97f0
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407818"
---
# <a name="cli-example-create-a-batch-ai-cluster-using-a-cluster-configuration-file"></a>Exemplo da CLI: Criar um cluster do Batch AI através de um ficheiro de configuração de cluster

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Este script demonstra como utilizar um ficheiro de configuração JSON para especificar definições para um cluster do Batch AI. Utilize estas definições em vez dos parâmetros de linha de comandos correspondentes para `az batchai cluster create`. Um ficheiro de configuração é útil quando precisar de montar vários sistemas de ficheiros nos nós de cluster ou quiser utilizar uma configuração idêntica em vários clusters.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia requer a execução da versão 2.0.38 ou posterior da CLI do Azure. Execute `az --version` para determinar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-config-file.sh "Create Batch AI cluster - configuration file")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute os comandos seguintes para remover os grupos de recursos e todos os recursos associados aos mesmos.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Cria uma conta de armazenamento. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Cria uma partilha de ficheiros numa conta de armazenamento. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Cria uma área de trabalho do Batch AI. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Cria um cluster do Batch AI. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Mostra informações sobre um cluster do Batch AI. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).
