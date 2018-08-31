---
title: 'Script do PowerShell: adicionar um utilizador externo a um laboratório no Azure DevTest Labs | Documentos da Microsoft'
description: Este script do PowerShell adiciona um utilizador externo a um laboratório no Azure DevTest Labs.
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
ms.openlocfilehash: 0acff1eb4cee441187205b11a7e07cc718072cbf
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248194"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>Utilize o PowerShell para adicionar um utilizador externo a um laboratório no Azure DevTest Labs

Este script do PowerShell de exemplo adiciona um utilizador externo a um laboratório no Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório**. O script requer que tenha um laboratório existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Repete o objeto de utilizador do Active Directory do Azure. |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Atribui a função especificada para o principal especificado, no âmbito especificado. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos do script do PowerShell do Azure Lab Services adicionais podem ser encontrados no [exemplos do PowerShell do Azure Lab Services](../samples-powershell.md).
