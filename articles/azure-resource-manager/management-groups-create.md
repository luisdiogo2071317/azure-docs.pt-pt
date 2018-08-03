---
title: Criar grupos de gestão para organizar recursos do Azure | Documentos da Microsoft
description: Saiba como criar grupos de gestão do Azure para gerir vários recursos.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: rithorn
ms.openlocfilehash: 13eea4f2b5125d7ab25b05a9e20021ebc6a2e0c6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435378"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Criar grupos de gestão para a organização de recursos e gestão

Os grupos de gestão são contentores que o ajudam a gerir o acesso, política e conformidade em várias subscrições. Criar estes contentores para criar uma hierarquia, eficaz e eficiente, que pode ser utilizada com [do Azure Policy](../azure-policy/azure-policy-introduction.md) e [controlos de acesso com base do Azure função](../role-based-access-control/overview.md). Para obter mais informações sobre grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md).

O primeiro grupo de gestão criado no diretório pode demorar até 15 minutos a concluir. Existem processos que executam na primeira vez para configurar o serviço de grupos de gestão do Azure para o seu diretório. Receber uma notificação quando o processo estiver concluído.  

## <a name="create-a-management-group"></a>Criar um grupo de gestão

Pode criar o grupo de gestão, utilizando o portal, o PowerShell ou a CLI do Azure.

### <a name="create-in-portal"></a>Criar no portal

1. Inicie sessão no [portal do Azure](http://portal.azure.com).
2. Selecione **todos os serviços** > **grupos de gestão**.
3. Na página principal, selecione **grupo de gestão de novo.**

    ![Grupo principal](media/management-groups/main.png)
4.  Preencha o campo de ID do grupo de gestão.
    - O **ID do grupo de gestão** é o identificador exclusivo do diretório que é utilizado para submeter os comandos neste grupo de gestão. Este identificador não é editável após a criação, como é usado em todo o sistema do Azure para identificar este grupo.
    - O campo de nome de exibição é o nome que é apresentado no portal do Azure. Um nome de apresentação separada é um campo opcional ao criar a gestão de grupo e pode ser alterado em qualquer altura.  

    ![Criar](media/management-groups/create_context_menu.png)  
5.  Selecione **guardar**

### <a name="create-in-powershell"></a>Criar no PowerShell

No PowerShell, utilize os cmdlets Add-AzureRmManagementGroups:

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso
```

O **GroupName** é um identificador exclusivo que está sendo criado. Esse identificador é utilizado por outros comandos, para fazer referência a este grupo e não é possível alterar mais tarde.

Se quisesse o grupo de gestão para mostrar um nome diferente no portal do Azure, adicionaria as **DisplayName** parâmetro com a cadeia de caracteres. Por exemplo, se quisesse criar um grupo de gestão com o GroupName da Contoso e o nome a apresentar do "Grupo de Contoso", usaria o seguinte cmdlet:

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
```

Utilize o **ParentId** parâmetro para que este grupo de gestão criadas numa gestão diferentes.  

### <a name="create-in-azure-cli"></a>Criar na CLI do Azure

Na CLI do Azure, utiliza o az comando de criação do grupo de gestão de conta.

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
```

---

## <a name="next-steps"></a>Passos Seguintes 
Para saber mais sobre os grupos de gestão, veja: 
- [Organizar os recursos com grupos de gestão do Azure ](management-groups-overview.md)
- [Como alterar, eliminar ou gerir os seus grupos de gestão](management-groups-manage.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalar a extensão CLI do Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
