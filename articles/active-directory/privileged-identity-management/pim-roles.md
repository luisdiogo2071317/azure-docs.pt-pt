---
title: Funções que não é possível gerir PIM - Azure | Documentos da Microsoft
description: Descreve as funções que não pode ser gerido no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 01/18/2019
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: e4c60c6ecdc586ba97a01fa42cd80b8ed541fa61
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434943"
---
# <a name="roles-you-cannot-manage-in-pim"></a>Funções não é possível gerir no PIM

O Azure AD Privileged Identity Management (PIM) permite-lhe gerir tudo [funções de diretório do Azure AD](../users-groups-roles/directory-assign-admin-roles.md) e todos os [funções de recursos do Azure](../../role-based-access-control/built-in-roles.md). Estas funções também incluem as suas funções personalizadas anexadas a seus grupos de gestão, subscrições, grupos de recursos e recursos. No entanto, existem algumas funções que não pode gerir. Este artigo descreve as funções que não é possível gerir PIM.

## <a name="classic-subscription-administrator-roles"></a>Funções de administrador de subscrição clássica

Não é possível gerir as seguintes funções de administrador de subscrição clássica no PIM:

- Administrador de Conta
- Administrador de Serviços
- Coadministrador

Para obter mais informações sobre as funções de administrador de subscrição clássica, consulte [funções de administrador de subscrição clássico, funções RBAC do Azure e funções de administrador do Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>E funções de administração do Office 365?

Funções no Exchange Online ou SharePoint Online, exceto para o administrador do Exchange e o administrador do SharePoint, não são representadas no Azure AD e, portanto, não podem ser gerenciadas no PIM. Para obter mais informações sobre estes serviços do Office 365, consulte [funções de administração do Office 365](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> Administrador do SharePoint tem acesso administrativo ao SharePoint Online através do Centro de administração do SharePoint Online e podem executar quase qualquer tarefa no SharePoint Online. Os utilizadores elegíveis poderão ocorrem atrasos com esta função dentro do SharePoint após a ativação no PIM.

## <a name="next-steps"></a>Passos Seguintes

- [Comece a utilizar o PIM](pim-getting-started.md)
- [Atribuir funções de diretório do Azure AD no PIM](pim-how-to-add-role-to-user.md)
- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
