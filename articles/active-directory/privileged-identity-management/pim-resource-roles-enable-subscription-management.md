---
title: Privileged Identity Management para recursos do Azure – ativar gestão de subscrições | Microsoft Docs
description: Saiba como os administradores globais podem gerir subscrições no inquilino.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4c6ae3da34fe5157314b8ea422591f7ecbd2a667
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="enable-subscription-management"></a>Ativar a gestão de subscrição

Como Administrador Global do seu diretório, poderá não ter acesso de predefinição para todos os recursos de subscrição no seu inquilino. Este artigo irá delinear os passos para conceder si próprio acesso a todas as subscrições no seu inquilino e uma abordagem recomendada para os restantes em conformidade com quaisquer controlos de segurança que requer a sua organização depois de receber o acesso.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Quem pode ativar a gestão de subscrições no meu diretório?

Cada utilizador atribuído à função de Administrador Global tem de seguir os passos abaixo para ativar a gestão de subscrição. Assim que tiver ativado a gestão de subscrição para si, pode adicionar outros administradores globais que possam ter acesso a recursos bem. Não há nenhuma definição de diretório que permite o acesso a todos os membros da função de Administrador Global.

## <a name="log-on-to-the-azure-portal"></a>Inicie sessão no portal do Azure

Comece por iniciar sessão no portal do Azure com uma conta que seja um membro do Active Directory ou elegível da função de Administrador Global. Se a conta for um Administrador Global elegível tem de ativar primeiro a função antes de passar para o passo seguinte.

## <a name="access-the-azure-ad-admin-center"></a>Aceder ao centro de administração do Azure AD

Agora que tem sessão iniciada no portal do Azure como um Administrador Global pode editar as definições que irão fornecer acesso a subscrições do Azure. Navegue para o Centro de administração do Azure AD, localize e selecione o separador de propriedades no painel de navegação esquerdo.

![](media/azure-pim-resource-rbac/aad_properties.png)

Na lista de propriedades, ative a opção intitulada "Administrador Global pode gerir as subscrições do Azure" como "Yes".

![](media/azure-pim-resource-rbac/aad_properties_save.png)

## <a name="navigate-to-azure-ad-pim"></a>Navegue para o Azure AD PIM

Com esta opção ativada, a sua conta é automaticamente adicionada à função do "Administrador de acesso de utilizador" para todos os recursos de subscrição no inquilino. Aqui, navegue para o Azure AD PIM e selecione os recursos do Azure na secção Gerir do painel de navegação esquerdo.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Gerir e detetar recursos

Se a sua organização já está a utilizar o Azure AD PIM para proteger os administradores no Azure Active Directory irá ver uma lista de subscrições quando carrega o painel.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Se não vir quaisquer recursos, consulte:
>- A função de Administrador Global não expirou 
>- A sua organização tem uma subscrição do Azure

![](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Configurar atribuições

Selecione uma subscrição da lista e atribui por si (ou um grupo que é membros) como um proprietário do recurso elegível. 
[Saiba mais sobre a atribuição de funções](pim-resource-roles-assign-roles.md).

Repita este processo para cada recurso antes de avançar para o passo seguinte.

## <a name="clean-up-standing-access"></a>Limpar acesso colocado

Agora que tem atribuições elegíveis para as subscrições importantes na sua organização, pode limpar acesso colocado ao desativar a opção nas propriedades de diretório:

![](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Passos Seguintes

[Detetar recursos](pim-resource-roles-discover-resources.md)

[Configurar definições de função](pim-resource-roles-configure-role-settings.md)








