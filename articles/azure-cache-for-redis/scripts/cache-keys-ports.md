---
title: Script da CLI do Azure de exemplo – obter o nome de anfitrião, portas e as chaves para a Cache do Azure para Redis | Documentos da Microsoft
description: Script da CLI do Azure de exemplo – obter o nome de anfitrião, portas e as chaves para uma Cache do Azure para a instância de Redis
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 820161b5ea5c1d071292897cab30f921f1256cf3
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53019933"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Obter o nome de anfitrião, portas e as chaves para a Cache do Azure para Redis

Neste cenário, irá aprender a obter o nome de anfitrião, portas e as chaves utilizadas para ligar a uma Cache do Azure para a instância de Redis.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Azure Cache for Redis")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para obter o nome de anfitrião, as chaves e as portas de uma Cache do Azure para a instância de Redis. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [show do AZ redis](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Obter os detalhes de uma Cache do Azure para a instância de Redis. |
| [lista de redis de AZ-chaves](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Obter chaves de acesso para uma Cache do Azure para a instância de Redis. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

A Cache do Azure adicionais para exemplos do script da CLI de Redis pode ser encontrada na [Cache do Azure para a documentação do Redis](../cli-samples.md).