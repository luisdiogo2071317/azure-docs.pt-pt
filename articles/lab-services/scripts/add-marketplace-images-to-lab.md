---
title: 'Script do PowerShell: adicionar uma imagem do marketplace para um laboratório personalizado nos serviços de laboratório do Azure | Microsoft Docs'
description: Este script do PowerShell adiciona uma imagem do marketplace para um laboratório personalizado nos serviços de laboratório do Azure.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 64d168c132edce4ecd128b795fbfa5ab2607cb19
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-custom-lab"></a>Utilize o PowerShell para adicionar uma imagem do marketplace para um laboratório personalizado

Este script do PowerShell de exemplo adiciona uma imagem do marketplace para um laboratório personalizado nos serviços de laboratório do Azure.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório personalizado**. O script requer que tenha um laboratório personalizado existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a custom lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Localizar AzureRmResource](/module/azurerm.resources/find-azurermresource) | Procura recursos com base nos parâmetros especificados. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Obtém os recursos. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Modifica um recurso. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Crie um recurso. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos de script do PowerShell do Azure laboratório serviços adicionais podem ser encontrados no [exemplos do PowerShell de serviços do Azure laboratório](../samples-powershell.md).