---
title: Script da CLI do Azure de exemplo - criar uma Cache do Azure Premium para Redis com o clustering | Documentos da Microsoft
description: Script da CLI do Azure de exemplo – criar uma camada Premium do Azure Cache de Redis com clustering
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 7ecae6711950ac180d8126060e9bc5a8e71e71d6
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696103"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Criar uma Cache do Azure Premium para Redis com clustering

Neste cenário, saiba como criar um Cache do Azure o escalão Premium 6 GB para Redis com o clustering ativado e partições horizontais de duas.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos e ativar um escalão Premium a Cache de Redis do Azure com o clustering. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [Criar AZ redis](https://docs.microsoft.com/cli/azure/redis) | Crie a Cache do Azure para a instância de Redis. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

A Cache do Azure adicionais para exemplos do script da CLI de Redis pode ser encontrada na [Cache do Azure para a documentação do Redis](../cli-samples.md).
