---
title: Exemplo de Script do Azure PowerShell - Alterar o intervalo de portas RDP | Microsoft Docs
description: Exemplo de Script do Azure PowerShell - Altera o intervalo de portas RDP de um cluster implementado.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 83fb6cc03f605a60b06f31fa6ddd82cd4e3e899e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30180192"
---
# <a name="update-the-rdp-port-range-values"></a>Atualizar os valores de intervalos de portas RDP

Este script de exemplo altera os valores de intervalos de portas RDP nas VMs do nó de cluster após o cluster ser implementado.  O Azure PowerShell é utilizado para que não haja ciclos das VMs correspondentes.  O script obtém o recurso `Microsoft.Network/loadBalancers` no grupo de recursos de cluster e atualiza os valores `inboundNatPools.frontendPortRangeStart` e `inboundNatPools.frontendPortRangeEnd`. Personalize os parâmetros conforme necessário.

Se necessário, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Obtenha o recurso `Microsoft.Network/loadBalancers`. |
|[Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Atualiza o recurso `Microsoft.Network/loadBalancers`.|

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para o Azure Service Fabric em [Exemplos do Azure PowerShell](../service-fabric-powershell-samples.md).
