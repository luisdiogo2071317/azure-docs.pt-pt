---
title: incluir ficheiro
description: incluir ficheiro
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 0ae24deca1cce14a475c59046be71b3b17ca5505
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957685"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Obtenha o nome do anfitrião, as portas e as chaves de acesso através da CLI do Azure

Para obter o nome de anfitrião e as portas com a CLI do Azure, pode chamar [show do az redis](https://docs.microsoft.com/cli/azure/redis#az_redis_show)e para obter as chaves pode chamar [az redis lista as chaves](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys). O script seguinte chama estes dois comandos e ecoa o nome de anfitrião, as portas e as chaves para a consola.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Para mais informações sobre este script, consulte [Get the hostname, ports, and keys for Azure Redis Cache (Obter o nome de anfitrião, as portas e as chaves para a Cache de Redis do Azure)](../articles/redis-cache/scripts/cache-keys-ports.md). Para obter mais informações sobre a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e [introdução à CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
