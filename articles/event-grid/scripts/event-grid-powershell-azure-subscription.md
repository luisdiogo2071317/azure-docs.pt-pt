---
title: Exemplo de script do Azure PowerShell - Subscrever uma subscrição do Azure | Microsoft Docs
description: Exemplo de script do Azure PowerShell - Subscrever uma subscrição do Azure
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: 7d4b7f49edca6c72681734f70dba3be8722edd91
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269316"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-powershell"></a>Subscrever eventos para uma subscrição do Azure com o PowerShell

Este script cria uma subscrição do Event Grid para os eventos para uma subscrição do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

O script de exemplo de pré-visualização requer o módulo do Event Grid. Para instalar, execute `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

## <a name="sample-script---stable"></a>Script de exemplo - estável

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.ps1 "Subscribe to Azure subscription")]

## <a name="sample-script---preview-module"></a>Script de exemplo - módulo de pré-visualização

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription-preview/subscribe-to-azure-subscription-preview.ps1 "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza o seguinte comando para criar a subscrição de eventos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Crie uma subscrição do Event Grid. |

## <a name="next-steps"></a>Passos seguintes

* Para obter uma introdução às aplicações geridas, veja [Descrição geral da Aplicação Gerida do Azure](../overview.md).
* Para obter mais informações sobre o PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
