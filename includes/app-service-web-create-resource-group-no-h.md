---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 8d0367b4e087da5954e9b16828655625978634fb
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
[!INCLUDE [resource group intro text](resource-group.md)]

No Cloud Shell, crie um grupo de recursos com o comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup*, na localização *Europa Ocidental*. Para ver todas as localizações suportadas para o Serviço de Aplicações, execute o comando [`az appservice list-locations`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Geralmente, o grupo de recursos e os recursos são criados numa região perto de si. 

Quando o comando for concluído, uma saída JSON mostra as propriedades do grupo de recursos.