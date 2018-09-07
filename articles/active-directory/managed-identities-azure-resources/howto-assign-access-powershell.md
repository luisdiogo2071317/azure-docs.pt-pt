---
title: Como atribuir acesso MSI a um recurso do Azure, com o PowerShell
description: Instruções passo a passo instruções para atribuir um MSI num recurso, o acesso a outro recurso, com o PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 6d503526ed97dd1f61269acd83810cd44598d72f
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028594"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Atribuir um acesso de identidade de serviço gerida (MSI) a um recurso com o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Assim que tiver configurado um recurso do Azure com um MSI, pode dar o acesso MSI para outro recurso, tal como qualquer entidade de segurança. Este exemplo mostra como conceder acesso MSI de uma máquina virtual do Azure para uma conta de armazenamento do Azure, com o PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Além disso, instale [do Azure PowerShell versão 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) se ainda não o fez.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI para outro recurso

Depois de ativar a MSI num recurso do Azure, [como uma VM do Azure](qs-configure-powershell-windows-vm.md):

1. Inicie sessão no Azure com o `Connect-AzureRmAccount` cmdlet. Utilize uma conta que seja associada à subscrição do Azure na qual configurou o MSI:

   ```powershell
   Connect-AzureRmAccount
   ```
2. Neste exemplo, estamos oferecendo um acesso de VM do Azure para uma conta de armazenamento. Primeiro, usamos [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) para obter o principal de serviço para a VM com o nome "myVM", que foi criada quando o MSI, nós habilitamos. Em seguida, usamos [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) para a VM de conceder acesso de "Leitor de" a uma conta de armazenamento chamado "myStorageAcct":

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Resolução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, certifique-se de que o MSI foi ativado corretamente. No nosso caso, podemos voltar à VM do Azure no [portal do Azure](https://portal.azure.com) e:

- Veja a página "Configuração" e certifique-se de MSI ativada = "Yes".
- Veja a página de "Extensões" e certifique-se a extensão MSI implementada com êxito.

Se uma for incorreta, terá de voltar a implementar o MSI no seu recurso, ou a falha de implementação de resolução de problemas.

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](overview.md).
- Para ativar o MSI numa VM do Azure, consulte [configurar uma Azure VM Managed Service Identity (MSI) com o PowerShell](qs-configure-powershell-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.

