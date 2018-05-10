---
title: 'Script do PowerShell: adicionar um utilizador externo a um laboratório personalizado nos serviços de laboratório do Azure | Microsoft Docs'
description: Este script do PowerShell adiciona um utilizador externo a um laboratório personalizado nos serviços de laboratório do Azure.
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
ms.openlocfilehash: b089067a889f0ffd3b317fcc3f0784d176473b91
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-an-external-user-to-a-custom-lab"></a>Utilize o PowerShell para adicionar um utilizador externo a um laboratório personalizado

Este script do PowerShell de exemplo adiciona um utilizador externo a um laboratório personalizado nos serviços de laboratório do Azure. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório personalizado**. O script requer que tenha um laboratório personalizado existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Repete o objeto de utilizador do Active Directory do Azure. |
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | Atribui a função especificada para o principal especificado, no âmbito especificado. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos de script do PowerShell do Azure laboratório serviços adicionais podem ser encontrados no [exemplos do PowerShell de serviços do Azure laboratório](../samples-powershell.md).