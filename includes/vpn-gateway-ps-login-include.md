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
ms.openlocfilehash: 0b535cac35012b2663dee4e276371c9eacfab03a
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31613658"
---
Antes de iniciar esta configuração, deve iniciar sessão na conta do Azure. O cmdlet pede-lhe as credenciais de início de sessão da conta Azure. Após iniciar sessão, são transferidas as definições da conta para que fiquem disponíveis para o Azure PowerShell. Para obter mais informações, veja [Utilizar o Windows PowerShell com o Resource Manager](../articles/powershell-azure-resource-manager.md).

Para iniciar sessão, abra a consola do PowerShell com privilégios elevados e ligue-se à sua conta. Utilize o exemplo seguinte para o ajudar na ligação:

```powershell
Connect-AzureRmAccount
```

Se tiver múltiplas subscrições do Azure, verifique as subscrições da conta.

```powershell
Get-AzureRmSubscription
```

Especifique a subscrição que pretende utilizar.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
