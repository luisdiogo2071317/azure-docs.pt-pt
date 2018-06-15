---
title: Script da CLI do Azure - Regenerar chave da conta do Azure Cosmos DB | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Regenerar uma chave da conta do Azure Cosmos DB
services: cosmos-db
documentationcenter: cosmosdb
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: sngun
ms.openlocfilehash: a50517d711fdae75184c85a5cb3adff83ec83ab3
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794892"
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>Regenerar uma chave da conta do Azure Cosmos DB com a CLI do Azure

Este exemplo regenera qualquer tipo de chave da conta do Azure Cosmos DB com a CLI do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Atualiza uma conta do Azure Cosmos DB. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb/regenerate-key) | Regenera chaves da conta do Azure Cosmos DB. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI do Azure Cosmos DB adicionais na [documentação da CLI do Azure Cosmos DB](../cli-samples.md).
