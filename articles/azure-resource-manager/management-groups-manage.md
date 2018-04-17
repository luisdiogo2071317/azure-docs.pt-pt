---
title: Como alterar, eliminar ou gerir os grupos de gestão - Azure | Microsoft Docs
description: Saiba como manter e atualizar a hierarquia de grupo de gestão.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: cba3f9290aff1808133b9d7780e4169fa25a10b2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="manage-your-resources-with-management-groups"></a>Gerir os recursos com grupos de gestão 
Grupos de gestão são contentores que ajudam a gerir o acesso, políticas e conformidade entre várias subscrições. Pode alterar, eliminar e gerir estes contentores ter hierarquias que podem ser utilizadas com [política Azure](../azure-policy/azure-policy-introduction.md) e [controlos de acesso com base do Azure funções (RBAC)](../role-based-access-control/overview.md). Para obter mais informações sobre grupos de gestão, consulte o artigo [organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md).

A funcionalidade de grupo de gestão está disponível uma versão de pré-visualização pública. Para começar a utilizar a gestão de grupos, início de sessão para o [portal do Azure](https://portal.azure.com) ou pode utilizar [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), ou o [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) para gerir os grupos de gestão.

Para efetuar alterações a um grupo de gestão, tem de ter uma função de proprietário ou contribuinte no grupo de gestão. Para ver as permissões têm, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções do RBAC, veja [gerir o acesso e permissões com RBAC](../role-based-access-control/overview.md).

## <a name="change-the-name-of-a-management-group"></a>Alterar o nome de um grupo de gestão 
Pode alterar o nome do grupo de gestão utilizando o portal, o PowerShell ou a CLI do Azure.

### <a name="change-the-name-in-the-portal"></a>Altere o nome no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão**  
3. Selecione o grupo de gestão que pretende mudar o nome. 
4. Selecione o **mudança de nome de grupo** opção na parte superior da página. 

    ![Mudança de nome de grupo](media/management-groups/detail_action_small.png)
5. Quando abre o menu, introduza o novo nome que pretende ter apresentado.

    ![Mudança de nome de grupo](media/management-groups/rename_context.png) 
4. Selecione **Guardar**. 

### <a name="change-the-name-in-powershell"></a>Altere o nome do PowerShell

Para atualizar a utilização do nome de apresentação **atualização AzureRmManagementGroup**. Por exemplo, para alterar um nome de grupos de gestão da "Contoso TI" para "Grupo de Contoso", execute o seguinte comando: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Altere o nome da CLI do Azure

Para a CLI do Azure, utilize o comando de atualização. 

```azure-cli
C:\> az account management-group update --group-name Contoso --display-name "Contoso Group" 
```

---
 
## <a name="delete-a-management-group"></a>Eliminar um grupo de gestão
Para eliminar um grupo de gestão, devem ser cumpridos os seguintes requisitos:
1. Não são grupos de gestão do elemento subordinado ou subscrições sob o grupo de gestão. 
    - Para mover uma subscrição fora de um grupo de gestão, consulte [mover subscrição para outro grupo de managemnt](#Move-subscriptions-in-the-hierarchy). 
    - Para mover um grupo de gestão para outro grupo de gestão, consulte [mover grupos de gestão na hierarquia](#Move-management-groups-in-the-hierarchy). 
2. Tem permissões de escrita a função de proprietário ou contribuinte grupo de gestão no grupo de gestão. Para ver as permissões têm, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções de RBAC, veja [gerir o acesso e permissões com RBAC](../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Eliminar no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão**  
3. Selecione o grupo de gestão que pretende eliminar. 
    
    ![Eliminar grupo](media/management-groups/delete.png)
4. Selecione **Eliminar**. 
    - Se o ícone estiver desativado, posicionado o Seletor de rato sobre o ícone mostra-lhe o motivo. 
5. Há uma janela que abre a confirmar que pretende eliminar o grupo de gestão. 

    ![Eliminar grupo](media/management-groups/delete_confirm.png) 
6. Selecione **Sim** 


### <a name="delete-in-powershell"></a>Eliminar no PowerShell

Utilize o **remover AzureRmManagementGroup** comando PowerShell ao eliminar grupos de gestão. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Eliminação na CLI do Azure
Com a CLI do Azure, utilize a eliminação do grupo de gestão da conta de az comando. 

```azure-cli
C:\> az account management-group delete --group-name Contoso
```
---

## <a name="view-management-groups"></a>Grupos de gestão de vista
Pode ver a qualquer grupo de gestão que tiver uma função RBAC direta ou herdada no.  

### <a name="view-in-the-portal"></a>Ver no portal
1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão** 
3. Hierarquia de grupo de gestão de página onde são apresentados todos os grupos que têm acesso de cargas. 
    ![Principal](media/management-groups/main.png)
4. Selecione um grupo de gestão individual para obter os detalhes  

### <a name="view-in-powershell"></a>Ver no PowerShell
Utilize o comando Get-AzureRmManagementGroup obter todos os grupos.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Para informações de um grupo de gestão único, utilize o parâmetro - GroupName

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Vista da CLI do Azure
Utilize o comando da lista para obter todos os grupos.  

```azure-cli
az account management-group list
```
Para informações de um grupo de gestão único, utilize o comando de mostrar

```azurepowershell-interactive
az account management-group show --group-name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Mover as subscrições da hierarquia
Uma razão para criar um grupo de gestão é agrupar subscrições em conjunto. Apenas grupos de gestão e as subscrições podem ser efetuadas subordinado de outro grupo de gestão. Uma subscrição que passa a um grupo de gestão herda todas as políticas de acesso de utilizador e do grupo de gestão principal. 

Para mover a subscrição, existem alguns permissões que tem de ter: 
- Função de "Proprietário" na subscrição subordinado.
- Função de "Proprietário" ou "Contribuinte" no novo grupo de gestão principal. 
- Função de "Proprietário" ou "Contribuinte" no grupo de gestão antigo do principal.
Para ver as permissões têm, selecione o grupo de gestão e, em seguida, selecione **IAM**. Para saber mais sobre as funções de RBAC, veja [gerir o acesso e permissões com RBAC](../role-based-access-control/overview.md). 

### <a name="move-subscriptions-in-the-portal"></a>Mover subscrições no portal

**Adicionar uma subscrição existente para um grupo de gestão**
1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão** 
3. Selecione o grupo de gestão que está a planear para ser o elemento principal.      
5. Na parte superior da página, selecione **adicionar existente**. 
6. No menu aberto, selecione o **tipo de recurso** do item que está a tentar mover a que é **subscrição**.
7. Selecione a subscrição na lista com o ID correto. 

    ![Elementos subordinados](media/management-groups/add_context_2.png)
8. Selecione "Guardar"

**Remover uma subscrição de um grupo de gestão**
1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão** 
3. Selecione o grupo de gestão que está a planear que é o principal atual.  
4. Selecione elipse no final da linha da subscrição na lista que pretende mover.

    ![Mover](media/management-groups/move_small.png)
5. Selecione **mover**
6. No menu que se abre, selecione o **grupo de gestão principal**.

    ![Mover](media/management-groups/move_small_context.png)
7. Selecione **guardar**

### <a name="move-subscriptions-in-powershell"></a>Mover subscrições no PowerShell
Para mover uma subscrição no PowerShell, utilize o comando de Add-AzureRmManagementGroupSubscription.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

Para remover a associação entre e subscrição e o grupo de gestão, utilize o comando Remove-AzureRmManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Mover subscrições na CLI do Azure
Para mover uma subscrição na CLI, utilize o comando adicionar. 

```azure-cli
C:\> az account management-group add --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Para remover a subscrição do grupo de gestão, utilize o comando de remoção de subscrição.  

```azure-cli
C:\> az account management-group remove --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Mover grupos de gestão na hierarquia  
Ao mover um grupo de gestão principal, todos os recursos subordinados que incluem a grupos de gestão, subscrições, grupos de recursos e mover recursos com o elemento principal.   

### <a name="move-management-groups-in-the-portal"></a>Mover grupos de gestão no portal

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **todos os serviços** > **grupos de gestão** 
3. Selecione o grupo de gestão que está a planear para ser o elemento principal.      
5. Na parte superior da página, selecione **adicionar existente**.
6. No menu aberto, selecione o **tipo de recurso** do item que está a tentar mover a que é **grupo de gestão**.
7. Selecione o grupo de gestão com o ID e o nome correto.

    ![Mover](media/management-groups/add_context.png)
8. Selecione **guardar**

### <a name="move-management-groups-in-powershell"></a>Mover grupos de gestão no PowerShell
Utilize o comando de atualização AzureRmManagementGroup no PowerShell para mover um grupo de gestão no outro grupo.  

```powershell
C:\> Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  
### <a name="move-management-groups-in-azure-cli"></a>Mover grupos de gestão na CLI do Azure
Utilize o comando de atualização para mover um grupo de gestão com a CLI do Azure. 

```azure-cli
C:/> az account management-group udpate --group-name Contoso --parent-id "Contoso Tenant" 
``` 

---

## <a name="next-steps"></a>Passos Seguintes 
Para obter mais informações sobre grupos de gestão, consulte: 
- [Organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md)
- [Criar grupos de gestão para organizar os recursos do Azure](management-groups-create.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Instale a extensão da CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)