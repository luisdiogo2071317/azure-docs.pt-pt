---
title: Exemplo de script do Azure PowerShell - Subscrever um grupo de recursos | Microsoft Docs
description: Exemplo de script do Azure PowerShell - Subscrever um grupo de recursos
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
ms.openlocfilehash: fe36336bb1bcc3b0d1cc718724ca05f6d23110c7
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270540"
---
# <a name="subscribe-to-events-for-a-resource-group-with-powershell"></a>Subscrever eventos para um grupo de recursos com o PowerShell

Este script cria uma subscrição do Event Grid para os eventos para um grupo de recursos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

O script de exemplo de pré-visualização requer o módulo do Event Grid. Para instalar, execute `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

## <a name="sample-script---stable"></a>Script de exemplo - estável

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.ps1 "Subscribe to resource group")]

## <a name="sample-script---preview-module"></a>Script de exemplo - módulo de pré-visualização

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.ps1 "Subscribe to resource group")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza o seguinte comando para criar a subscrição de eventos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Crie uma subscrição do Event Grid. |

## <a name="next-steps"></a>Passos seguintes

* Para obter uma introdução às aplicações geridas, veja [Descrição geral da Aplicação Gerida do Azure](../overview.md).
* Para obter mais informações sobre o PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
