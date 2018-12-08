---
title: Script do Azure PowerShell de exemplo - criar uma área de trabalho do Log Analytics | Documentos da Microsoft
description: Script do Azure PowerShell de exemplo - criar uma área de trabalho do Log Analytics para
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 5ad04c52da4709a7097ff7915d7af7404d6725eb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109494"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Criar uma área de trabalho do Log Analytics com o PowerShell

Este script ajuda-o em funcionamento rapidamente com uma área de trabalho do Log Analytics do Azure, que é necessária se quiser voltar a recolher, analisar e efetuar ações nos dados.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar uma nova área de trabalho do Log Analytics na sua subscrição. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Obtém informações sobre uma área de trabalho existente. |
| [Novo-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Cria uma área de trabalho no grupo de recursos especificado e localização. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

