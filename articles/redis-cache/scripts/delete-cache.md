---
title: Exemplo de Script do CLI do Azure - eliminar uma Cache de Redis do Azure | Microsoft Docs
description: Exemplo de Script do CLI do Azure - eliminar uma Cache de Redis do Azure
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: f24dbb27a9ac38b5987fd1e21ce9e32b80d8082f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="delete-an-azure-redis-cache"></a>Eliminar uma Cache de Redis do Azure

Neste cenário, irá aprender a eliminar uma Cache de Redis do Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para eliminar uma instância da Cache de Redis do Azure. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az redis delete](https://docs.microsoft.com/cli/azure/redis#az_redis_delete) | Elimine a instância da Cache de Redis. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Exemplos de script CLI de Cache de Redis do Azure adicionais podem ser encontrados no [documentação de Cache de Redis do Azure](../cli-samples.md).