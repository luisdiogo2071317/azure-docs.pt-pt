---
title: "Exemplo de Script CLI do Azure - obter o nome do anfitrião, portas e as chaves de Cache de Redis do Azure | Microsoft Docs"
description: "Exemplo de Script CLI do Azure - obter o nome do anfitrião, portas e as chaves de instância da Cache de Redis do Azure"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 6cfff74ed037835abb6d3e3975fe33d71b9730a3
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Obter o nome de anfitrião, portas e as chaves para a Cache de Redis do Azure

Neste cenário, irá aprender a obter o nome de anfitrião, portas e as chaves utilizadas para ligar a uma instância da Cache de Redis do Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos para obter o nome de anfitrião, as chaves e as portas de uma instância da Cache de Redis do Azure. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Obter os detalhes de uma instância da Cache de Redis do Azure. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Obter chaves de acesso para uma instância da Cache de Redis do Azure. |


## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, consulte [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Exemplos de script CLI de Cache de Redis do Azure adicionais podem ser encontrados no [documentação de Cache de Redis do Azure](../cli-samples.md).