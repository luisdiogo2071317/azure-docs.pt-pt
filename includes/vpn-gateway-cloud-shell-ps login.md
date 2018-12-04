---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: fee97b29f24d8bb4f50a2929c3ceb33af85a5e21
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852378"
---
Abra a consola do PowerShell com privilégios elevados.



Se estiver a executar localmente do Azure PowerShell, ligue à sua conta do Azure. O *Connect-AzureRmAccount* cmdlet pede-lhe as credenciais. Após a autenticação, são transferidas as definições de conta para que fiquem disponíveis para o Azure PowerShell. Se não estiver a executar PowerShell localmente e estiver em vez disso, a utilizar o Azure Cloud Shell "Experimente" no navegador, ignore este passo primeiro. Irá ligar à sua conta do Azure automaticamente.

```azurepowershell
Connect-AzureRmAccount
```

Se tiver mais de uma subscrição, obtenha uma lista das suas subscrições do Azure.

```azurepowershell-interactive
Get-AzureRmSubscription
```

Especifique a subscrição que pretende utilizar.

```azurepowershell-interactive
Select-AzureRmSubscription -SubscriptionName "Name of subscription"
```