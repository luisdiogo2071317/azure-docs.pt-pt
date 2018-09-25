---
title: Criar grupos de gestão para organizar recursos do Azure
description: Saiba como criar grupos de gestão do Azure para gerir vários recursos.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2018
ms.author: rithorn
ms.openlocfilehash: c2d4317bcbf70a0cebf6ab1915968eeb9ef8b4c6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992631"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Criar grupos de gestão para a organização de recursos e gestão

Os grupos de gestão são contentores que o ajudam a gerir o acesso, política e conformidade em várias subscrições. Criar estes contentores para criar uma hierarquia, eficaz e eficiente, que pode ser utilizada com [do Azure Policy](../policy/overview.md) e [controlos de acesso com base do Azure função](../../role-based-access-control/overview.md). Para obter mais informações sobre grupos de gestão, consulte [organizar os recursos com grupos de gestão do Azure](overview.md).

O primeiro grupo de gestão criado no diretório pode demorar até 15 minutos a concluir. Existem processos que executam na primeira vez para configurar o serviço de grupos de gestão do Azure para o seu diretório. Receber uma notificação quando o processo estiver concluído.

## <a name="create-a-management-group"></a>Criar um grupo de gestão

Pode criar o grupo de gestão, utilizando o portal, o PowerShell ou a CLI do Azure. Atualmente, não é possível utilizar modelos do Resource Manager para criar grupos de gestão.

### <a name="create-in-portal"></a>Criar no portal

1. Inicie sessão no [portal do Azure](http://portal.azure.com).

1. Selecione **todos os serviços** > **grupos de gestão**.

1. Na página principal, selecione **grupo de gestão novos**.

   ![Grupo principal](./media/main.png)

1. Preencha o campo de ID do grupo de gestão.

   - O **ID do grupo de gestão** é o identificador exclusivo do diretório que é utilizado para submeter os comandos neste grupo de gestão. Este identificador não é editável após a criação, como é usado em todo o sistema do Azure para identificar este grupo.
   - O campo de nome de exibição é o nome que é apresentado no portal do Azure. Um nome de apresentação separada é um campo opcional ao criar a gestão de grupo e pode ser alterado em qualquer altura.  

   ![Criar](./media/create_context_menu.png)  

1. Selecione **Guardar**.

### <a name="create-in-powershell"></a>Criar no PowerShell

No PowerShell, utilize os cmdlets Add-AzureRmManagementGroups:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso'
```

O **GroupName** é um identificador exclusivo que está sendo criado. Esse identificador é utilizado por outros comandos, para fazer referência a este grupo e não é possível alterar mais tarde.

Se quisesse o grupo de gestão para mostrar um nome diferente no portal do Azure, adicionaria as **DisplayName** parâmetro com a cadeia de caracteres. Por exemplo, se quisesse criar um grupo de gestão com o GroupName da Contoso e o nome a apresentar do "Grupo de Contoso", usaria o seguinte cmdlet:

```azurepowershell-interactive
New-AzureRmManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId 'ContosoTenant'
```

Utilize o **ParentId** parâmetro para que este grupo de gestão criadas numa gestão diferentes.

### <a name="create-in-azure-cli"></a>Criar na CLI do Azure

Na CLI do Azure, utiliza o az comando de criação do grupo de gestão de conta.

```azurecli-interactive
az account management-group create --group-name 'Contoso'
```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os grupos de gestão, veja:

- [Organizar os recursos com grupos de gestão do Azure](overview.md)
- [Como alterar, eliminar ou gerir os seus grupos de gestão](manage.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalar a extensão CLI do Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)