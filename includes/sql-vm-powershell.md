---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: 3dc799ecc75589279c8d1c73062a8f2157761330
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213156"
---
## <a name="start-your-powershell-session"></a>Iniciar a sessão do PowerShell
 

Executar o [ **Connect-Az Account** ](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet e verá um ecrã de início de sessão para introduzir as suas credenciais. Utilize as mesmas credenciais que utiliza para iniciar sessão no Portal do Azure.

```powershell
Connect-AzAccount
```

Se tiver várias subscrições, utilize o [ **conjunto AzContext** ](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet para selecionar a subscrição a sessão do PowerShell deve utilizar. Para ver que subscrição a atual sessão do PowerShell está a utilizar, execute [ **Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Para ver todas as suas subscrições, execute [ **Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

