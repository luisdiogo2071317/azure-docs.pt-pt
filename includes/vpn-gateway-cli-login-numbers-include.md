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
ms.openlocfilehash: a72f5f5a1d8c64b347185827906f15d3cb9f55ad
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30197861"
---
1. Inicie sessão na sua subscrição do Azure com o comando [az login](/cli/azure/#login) e siga as instruções no ecrã. Para obter mais informações sobre o início de sessão, veja [Introdução à CLI 2.0 do Azure](/cli/azure/get-started-with-azure-cli).

  ```azurecli
  az login
  ```
2. Se tiver mais de uma subscrição do Azure, liste as subscrições da conta.

  ```azurecli
  az account list --all
  ```
3. Especifique a subscrição que pretende utilizar.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```