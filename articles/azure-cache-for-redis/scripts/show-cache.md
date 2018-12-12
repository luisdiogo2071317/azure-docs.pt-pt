---
title: Exemplo do Script da CLI do Azure - Obtenha detalhes de um Cache do Azure para Redis | Documentos da Microsoft
description: Exemplo do Script da CLI do Azure - Obtenha detalhes de um Cache do Azure para Redis
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 0dd32ae2b8f0fb91ca8d6391d5c3f3979d13b90f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085268"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>Obter os detalhes de uma Cache do Azure para Redis

Neste cenário, irá aprender a obter os detalhes de um Cache do Azure para a instância de Redis, incluindo o estado de aprovisionamento.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Cache for Redis")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para obter os detalhes de um Cache do Azure para a instância de Redis. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [show do AZ redis](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Obter os detalhes de uma Cache do Azure para a instância de Redis. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

A Cache do Azure adicionais para exemplos do script da CLI de Redis pode ser encontrada na [Cache do Azure para a documentação do Redis](../cli-samples.md).