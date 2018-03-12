---
title: Exemplo de Script CLI do Azure - Obtenha detalhes de uma Cache de Redis do Azure | Microsoft Docs
description: Exemplo de Script CLI do Azure - Obtenha detalhes de uma Cache de Redis do Azure
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 00ae8e00d456d4660cadf645d4521173d254bbd7
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="get-details-of-an-azure-redis-cache"></a>Obter os detalhes de uma Cache de Redis do Azure

Neste cenário, irá aprender a obter os detalhes de uma instância da Cache de Redis do Azure, incluindo o respetivo estado de aprovisionamento.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Redis Cache")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para obter os detalhes de uma instância da Cache de Redis do Azure. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Obter os detalhes de uma instância da Cache de Redis do Azure. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Exemplos de script CLI de Cache de Redis do Azure adicionais podem ser encontrados no [documentação de Cache de Redis do Azure](../cli-samples.md).