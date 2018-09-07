---
title: 'Script do PowerShell: conjunto permitida de tamanhos de VM no Azure Lab Services | Documentos da Microsoft'
description: Este script do PowerShell define permitidos de tamanhos de VM no Azure Lab Services.
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
ms.openlocfilehash: 559e74675a5d113584dca21979c20462c9cdf19c
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054711"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Utilize o PowerShell para definir permitida de tamanhos de VM no Azure Lab Services

Este script do PowerShell de exemplo define os tamanhos de máquinas de virtuais permitidas (VM) no Azure Lab Services.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório**. O script requer que tenha um laboratório existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) | Procura de recursos com base em parâmetros especificados. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Obtém os recursos. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Modifica um recurso. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Crie um recurso. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos do script do PowerShell do Azure Lab Services adicionais podem ser encontrados no [exemplos do PowerShell do Azure Lab Services](../samples-powershell.md).
