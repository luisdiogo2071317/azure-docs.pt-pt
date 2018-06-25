---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc7367ca2162dde9a266341f0d7b3dd1e8b0da81
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313698"
---
Inicie sessão na sua subscrição do Azure com o comando [az login](/cli/azure/#login) e siga as instruções no ecrã. Para obter mais informações sobre o início de sessão, veja [Introdução à CLI 2.0 do Azure](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Se tiver mais de uma subscrição do Azure, liste as subscrições da conta.

```azurecli
az account list --all
```

Especifique a subscrição que pretende utilizar.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```