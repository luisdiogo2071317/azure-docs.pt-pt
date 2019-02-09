---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: f9a45da2703518000aa464da067c5cf71a198fd4
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984975"
---
## <a name="start-your-powershell-session"></a>Iniciar a sessão do PowerShell
 

Executar o [ **Connect-Az Account** ](https://docs.microsoft.com/powershell/module/az.accounts/connect-azmaccount) cmdlet e verá um ecrã de início de sessão para introduzir as suas credenciais. Utilize as mesmas credenciais que utiliza para iniciar sessão no Portal do Azure.

```powershell
Connect-AzAccount
```

Se tiver várias subscrições, utilize o [ **conjunto AzContext** ](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet para selecionar a subscrição a sessão do PowerShell deve utilizar. Para ver que subscrição a atual sessão do PowerShell está a utilizar, execute [ **Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext). Para ver todas as suas subscrições, execute [ **Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription).

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

