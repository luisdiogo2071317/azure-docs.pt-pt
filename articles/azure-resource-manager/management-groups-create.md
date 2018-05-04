---
title: Criar grupos de gestão para organizar os recursos do Azure | Microsoft Docs
description: Saiba como criar grupos de gestão do Azure para gerir vários recursos.
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
ms.openlocfilehash: bce7a8532782e6d911ab1acdd74de8bb0de84307
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Criar grupos de gestão para a organização de recursos e de gestão
Grupos de gestão são contentores que ajudam a gerir o acesso, políticas e conformidade entre várias subscrições. Criar estes contentores para criar uma hierarquia eficaz e eficiente que pode ser utilizada com [política Azure](../azure-policy/azure-policy-introduction.md) e [controlos de acesso com base do Azure função](../role-based-access-control/overview.md). Para obter mais informações sobre grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md). 

A funcionalidade de grupo de gestão está disponível uma versão de pré-visualização pública. Para começar a utilizar a gestão de grupos, início de sessão para o [portal do Azure](https://portal.azure.com) ou pode utilizar [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [CLI do Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), ou o [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) para Crie grupos de gestão.   

O primeiro grupo de gestão criado no diretório pode demorar até 15 minutos a concluir. Existem processos que executam na primeira vez para configurar o serviço de grupos de gestão no Azure para o seu diretório. Receber uma notificação quando o processo estiver concluído.  

## <a name="how-to-create-a-management-group"></a>Como criar um grupo de gestão
Pode criar o grupo de gestão utilizando o portal, o PowerShell ou a CLI do Azure.

### <a name="create-in-portal"></a>Criar no portal

1. Inicie sessão no [portal do Azure](http://portal.azure.com).
2. Selecione **todos os serviços** > **grupos de gestão**.
3. Na página principal, selecione **grupo de gestão novo.** 

    ![Criar Grupo](media/management-groups/create_main.png) 
4.  Preencha o campo de ID do grupo de gestão. 
    - O **ID do grupo de gestão** é o identificador exclusivo do diretório que é utilizado para submeter os comandos neste grupo de gestão. Este identificador não é editável após a criação, que está a ser utilizada em todo o sistema do Azure para identificar este grupo. 
    - O campo de nome de apresentação é o nome que é apresentado no portal do Azure. Um nome a apresentar separado é um campo opcional ao criar a gestão de grupo e podem ser alteradas em qualquer altura.  

    ![Criar](media/management-groups/create_context_menu.png)  
5.  Selecione **guardar**


### <a name="create-in-powershell"></a>Criar no PowerShell
No PowerShell, utilize os cmdlets Add-AzureRmManagementGroups.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
O **GroupName** é um identificador exclusivo a ser criado. Este ID é utilizado por outros comandos, para fazer referência a este grupo e não é possível alterar mais tarde.

Se quisesse o grupo de gestão para mostrar um nome diferente dentro do portal do Azure, teria de adicionar o **DisplayName** parâmetro com a cadeia. Por exemplo, se quisesse criar um grupo de gestão com GroupName da Contoso e o nome a apresentar do "Grupo de Contoso", teria de utilizar o seguinte cmdlet: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Utilize o **ParentId** parâmetro para este grupo de gestão criadas num de gestão diferente.  

### <a name="create-in-azure-cli"></a>Criar na CLI do Azure
Na CLI do Azure, utilize o az comando de criar grupo de gestão de conta. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Passos Seguintes 
Para obter mais informações sobre grupos de gestão, consulte: 
- [Organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md)
- [Como alterar, eliminar ou gerir os grupos de gestão](management-groups-manage.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Instale a extensão da CLI do Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
