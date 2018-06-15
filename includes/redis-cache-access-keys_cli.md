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
ms.openlocfilehash: 0289604cce7f956406d65743d5b058ec92111724
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182662"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Obtenha o nome do anfitrião, as portas e as chaves de acesso através da CLI do Azure
Para obter o nome do anfitrião e as portas através da CLI 2.0 do Azure, pode chamar para [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) e para obter as chaves pode chamar para [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys). O script seguinte chama estes dois comandos e ecoa o nome de anfitrião, as portas e as chaves para a consola.

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

Para mais informações sobre este script, consulte [Get the hostname, ports, and keys for Azure Redis Cache (Obter o nome de anfitrião, as portas e as chaves para a Cache de Redis do Azure)](../articles/redis-cache/scripts/cache-keys-ports.md). Para obter mais informações sobre a CLI 2.0 do Azure, consulte [Install Azure CLI 2.0 (Instalar a CLI 2.0 do Azure)](https://docs.microsoft.com/cli/azure/install-azure-cli) e [Get started with Azure CLI 2.0 (Introdução à CLI 2.0 do Azure)](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
