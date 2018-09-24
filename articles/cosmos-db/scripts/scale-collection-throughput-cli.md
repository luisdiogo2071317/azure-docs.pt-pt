---
title: Script da CLI do Azure - Dimensionar Débito do contentor do Azure Cosmos DB | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Dimensionar débito do contentor do Azure Cosmos DB
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
ms.date: 05/23/2018
ms.author: sngun
ms.openlocfilehash: 8cd8ba674040454138ba236d9e6f7d96b33cf1d4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973991"
---
# <a name="scale-azure-cosmos-db-container-throughput-using-the-azure-cli"></a>Dimensionar débito do contentor do Azure Cosmos DB com a CLI do Azure

Este exemplo dimensiona o débito do contentor para qualquer tipo de contentor do Azure Cosmos DB.  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/scale-cosmosdb-throughput/scale-cosmosdb-throughput.sh?highlight=40-46 "Scale Azure Cosmos DB throughput")]

O script de exemplo acima permite-lhe criar e dimensionar uma coleção fixa. Se pretender criar e dimensionar uma coleção com capacidade de armazenamento ilimitada, tem de: 
 
* Criar a coleção com, pelo menos, 1000 RU/s e 
* Especificar uma chave de partição ao criar a coleção. 

O comando seguinte mostra um exemplo para criar uma coleção com capacidade de armazenamento ilimitada:

```cli
az cosmosdb collection create \
    --collection-name $collectionName \
    --name $name \
    --db-name $databaseName \
    --resource-group $resourceGroupName \
    --throughput 1000
    --partition-key-path /deviceId

```

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az cosmosdb update](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-update) | Atualiza uma conta do Azure Cosmos DB. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI do Azure Cosmos DB adicionais na [documentação da CLI do Azure Cosmos DB](../cli-samples.md).
