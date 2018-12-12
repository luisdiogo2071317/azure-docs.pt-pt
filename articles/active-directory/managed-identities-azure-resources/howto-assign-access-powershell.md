---
title: Como atribuir acesso uma identidade gerida a um recurso do Azure com o PowerShell
description: Instruções passo a passo instruções para atribuir uma identidade gerida num recurso, o acesso a outro recurso, com o PowerShell.
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
ms.date: 12/06/2018
ms.author: daveba
ms.openlocfilehash: 7ed61ead475acb81da4434c880954e801492351b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081562"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Atribuir um acesso de identidade gerida a um recurso com o PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Assim que tiver configurado um recurso do Azure com uma identidade gerida, pode dar o acesso de identidade gerida para outro recurso, tal como qualquer entidade de segurança. Este exemplo mostra como conceder acesso de identidade gerida de uma máquina virtual do Azure para uma conta de armazenamento do Azure com o PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre uma identidade gerida atribuído de sistema e atribuído ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Utilizar o RBAC para atribuir um acesso de identidade gerida para outro recurso

Depois de ativar a identidade gerida num recurso do Azure, [como uma VM do Azure](qs-configure-powershell-windows-vm.md):

1. Inicie sessão no Azure com o `Connect-AzureRmAccount` cmdlet. Utilize uma conta que seja associada à subscrição do Azure na qual configurou a identidade gerida:

   ```powershell
   Connect-AzureRmAccount
   ```
2. Neste exemplo, estamos oferecendo um acesso de VM do Azure para uma conta de armazenamento. Primeiro, usamos [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) para obter o principal de serviço para a VM com o nome `myVM`, que foi criada quando, nós habilitamos a identidade gerida. Em seguida, utilize [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) para dar a VM **leitor** acesso a uma conta de armazenamento denominada `myStorageAcct`:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Passos Seguintes

- [Identidade gerida para descrição geral de recursos do Azure](overview.md)
- Para ativar a identidade gerida numa VM do Azure, consulte [configurar geridos identidades para recursos do Azure na VM do Azure com o PowerShell](qs-configure-powershell-windows-vm.md).
