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
ms.openlocfilehash: 82de8eab089e5f666e1a2ce4eab09bfd2895185b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020086"
---
1. Inicie sessão na sua subscrição do Azure com o comando [az login](/cli/azure/#login) e siga as instruções no ecrã. Para obter mais informações sobre o início de sessão, veja [Introdução à CLI do Azure](/cli/azure/get-started-with-azure-cli).

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
