---
title: Como alterar, eliminar ou gerir os seus grupos de gestão no Azure
description: Aprenda a manter e atualizar a hierarquia de grupo de gestão.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rithorn
ms.openlocfilehash: 627ef0123f05e768dd8a83c197b25da7f161a37c
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853001"
---
# <a name="manage-your-resources-with-management-groups"></a>Gerir os recursos com grupos de gestão

Os grupos de gestão são contentores que o ajudam a gerir o acesso, política e conformidade em várias subscrições. Pode alterar, eliminar e gerir estes contentores ter hierarquias que podem ser utilizadas com [do Azure Policy](../policy/overview.md) e [controlos de acesso com base do Azure funções (RBAC)](../../role-based-access-control/overview.md). Para saber mais sobre os grupos de gestão, veja [organizar os recursos com grupos de gestão do Azure](overview.md).

Para fazer alterações a um grupo de gestão, tem de ter uma função de proprietário ou Contribuidor no grupo de gestão. Para ver quais as permissões que tem, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções do RBAC, veja [gerir o acesso e permissões com RBAC](../../role-based-access-control/overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Alterar o nome de um grupo de gestão

Pode alterar o nome do grupo de gestão, utilizando o portal, o PowerShell ou a CLI do Azure.

### <a name="change-the-name-in-the-portal"></a>Altere o nome no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Selecione o grupo de gestão que pretende mudar o nome.

1. Selecione o **mudança de nome de grupo** opção na parte superior da página.

   ![Mudar o Nome do Grupo](./media/detail_action_small.png)

1. Quando abre o menu, introduza o novo nome que gostaria de ter apresentado.

   ![Mudar o Nome do Grupo](./media/rename_context.png)

1. Selecione **Guardar**.

### <a name="change-the-name-in-powershell"></a>Alterar o nome no PowerShell

Para atualizar o uso de nome de exibição **AzureRmManagementGroup atualização**. Por exemplo, para alterar um nome de grupos de gestão de "Contoso TI" para "Grupo de Contoso", execute o seguinte comando:

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Altere o nome na CLI do Azure

Para a CLI do Azure, utilize o comando de atualização.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Eliminar um grupo de gestão

Para eliminar um grupo de gestão, devem ser cumpridos os seguintes requisitos:

1. Não há grupos de gestão subordinado ou subscrições no grupo de gestão.

   - Para mover uma subscrição para fora de um grupo de gestão, consulte [mover a subscrição para outro grupo de gestão](#Move-subscriptions-in-the-hierarchy).

   - Para mover um grupo de gestão para outro grupo de gestão, consulte [mover grupos de gestão na hierarquia de](#Move-management-groups-in-the-hierarchy).

1. Tem permissões de escrita sobre a função de proprietário ou contribuinte do grupo de gestão no grupo de gestão. Para ver quais as permissões que tem, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções do RBAC, veja [gerir o acesso e permissões com RBAC](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Eliminar no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Selecione o grupo de gestão que pretende eliminar.

1. Selecione **eliminar**

   - Se o ícone estiver desabilitado, passar o rato seu Seletor de mouse sobre o ícone mostra-lhe o motivo.

   ![Eliminar grupo](./media/delete.png)

1. Há uma janela que abre-se de confirmar que pretende eliminar o grupo de gestão.

   ![Eliminar grupo](./media/delete_confirm.png)

1. Selecione **Sim**.

### <a name="delete-in-powershell"></a>Eliminar no PowerShell

Utilize o **Remove-AzureRmManagementGroup** comando do PowerShell para eliminar grupos de gestão.

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Eliminação na CLI do Azure

Com a CLI do Azure, utilize a eliminação de grupo de gestão de conta do comando az.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Ver grupos de gestão

Pode ver qualquer grupo de gestão que tem uma função RBAC direta ou herdada no.  

### <a name="view-in-the-portal"></a>Ver no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Onde poderá explorar todos os grupos de gestão e as subscrições que tem acesso de carregamentos de página de hierarquia de grupo de gestão. Selecionar o nome do grupo, leva-o para um nível na hierarquia. A navegação funciona da mesma forma um Explorador de ficheiros.

   ![Principal](./media/main.png)

1. Para ver os detalhes do grupo de gestão, selecione o **(detalhes)** ligação ao lado do título do grupo de gestão. Se esta ligação não estiver disponível, não tem permissões para ver esse grupo de gestão.  

### <a name="view-in-powershell"></a>Modo de exibição no PowerShell

Utilize o comando Get-AzureRmManagementGroup para recuperar todos os grupos.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

Para informações de um grupo de gestão único, utilize o parâmetro - GroupName

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="view-in-azure-cli"></a>Modo de exibição na CLI do Azure

Utilize o comando de lista para obter todos os grupos.  

```azurecli-interactive
az account management-group list
```

Para informações de um grupo de gestão único, use o comando show

```azurecli-interactive
az account management-group show --name 'Contoso'
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Mover subscrições na hierarquia

Uma das razões para criar um grupo de gestão é agrupar subscrições. Apenas grupos de gestão e as subscrições podem ser feitas filhos do outro grupo de gestão. Uma subscrição que se move para um grupo de gestão herda todas as políticas de acesso de utilizador e do grupo de gestão principal.

Para mover a subscrição, existem algumas permissões que tem de ter:

- Função de "Proprietário" na subscrição filho.
- Função de "Proprietário" ou "Contribuinte" no novo grupo de gestão principal.
- Função "Proprietário" ou "Contribuinte" no grupo de gestão principal antigo.

Para ver quais as permissões que tem, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções do RBAC, veja [gerir o acesso e permissões com RBAC](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Mover subscrições no portal

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Adicionar uma subscrição existente para um grupo de gestão

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Selecione o grupo de gestão que pretende ser o elemento principal.

1. Na parte superior da página, selecione **adicionar subscrição**.

1. Selecione a subscrição na lista com o ID correto.

   ![Crianças](./media/add_context_sub.png)

1. Selecione "Guardar".

#### <a name="remove-a-subscription-from-a-management-group"></a>Remover uma assinatura de um grupo de gestão

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Selecione o grupo de gestão estiver a planear isto é o principal atual.  

1. Selecione as reticências no final da linha para a subscrição na lista que pretende mover.

   ![Mover](./media/move_small.png)

1. Selecione **mover**.

1. No menu que se abre, selecione o **grupo de gestão principal**.

   ![Mover](./media/move_small_context.png)

1. Selecione **Guardar**.

### <a name="move-subscriptions-in-powershell"></a>Mover subscrições no PowerShell

Para mover uma subscrição no PowerShell, utilize o comando New-AzureRmManagementGroupSubscription.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Para remover a associação entre e subscrição e o grupo de gestão, utilize o comando de Remove-AzureRmManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Mover subscrições na CLI do Azure

Para mover uma subscrição na CLI, utilize o comando add.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Para remover a subscrição do grupo de gestão, utilize o comando de remoção de subscrição.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Mover grupos de gestão na hierarquia  

Quando move um grupo de gestão principal, todos os recursos filho que incluem grupos de gestão, subscrições, grupos de recursos e movimentação de recursos com o elemento principal.

### <a name="move-management-groups-in-the-portal"></a>Mover grupos de gestão no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Selecione o grupo de gestão que pretende ser o elemento principal.

1. Na parte superior da página, selecione **adicionar grupo de gestão**.

1. No menu que se abre, selecione se quiser que um novo ou utilizar um grupo de gestão existente.

   - Selecionar novo irá criar um novo grupo de gestão.
   - Selecionar um existente irá apresentar-lhe com um menu suspenso de todos os grupos de gestão que pode mover a este grupo de gestão.  

   ![mover](./media/add_context_MG.png)

1. Selecione **Guardar**.

### <a name="move-management-groups-in-powershell"></a>Mover grupos de gestão no PowerShell

Utilize o comando de atualização AzureRmManagementGroup no PowerShell para mover um grupo de gestão num grupo diferente.

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Mover grupos de gestão na CLI do Azure

Utilize o comando de atualização para mover um grupo de gestão com a CLI do Azure.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os grupos de gestão, veja:

- [Organizar os recursos com grupos de gestão do Azure](overview.md)
- [Criar grupos de gestão para organizar recursos do Azure](create.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Rever as Especificações da API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalar a extensão CLI do Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
