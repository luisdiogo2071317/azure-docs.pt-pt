---
title: 'Script do PowerShell: criar uma função personalizada num laboratório personalizado nos serviços de laboratório do Azure | Microsoft Docs'
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
ms.openlocfilehash: df91c9f842d338e1725fec2734129f2f1f3d3721
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-create-a-custom-role-in-a-custom-lab-in-azure-lab-services"></a>Utilize o PowerShell para criar uma função personalizada num laboratório personalizado nos serviços de laboratório do Azure

Este script do PowerShell de exemplo cria uma função personalizada a utilizar num laboratório personalizado nos serviços de laboratório do Azure. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Um laboratório personalizado**. O script requer que tenha um laboratório personalizado existente. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: 

| Comando | Notas |
|---|---|
| [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) | Obtém as operações para um fornecedor de recursos do Azure que têm capacidade de segurança através do RBAC do Azure. |
| [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) | Apresenta uma lista de todas as funções de RBAC do Azure que estão disponíveis para atribuição. |
| [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) | Cria uma função personalizada. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Exemplos de script do PowerShell do Azure laboratório serviços adicionais podem ser encontrados no [exemplos do PowerShell de serviços do Azure laboratório](../samples-powershell.md).