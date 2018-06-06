---
title: Privileged Identity Management para recursos do Azure – ativar a gestão de subscrição | Microsoft Docs
description: Saiba os administradores globais como pode gerir subscrições no inquilino.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 22e735fc10f314e47c00795bf993b5502f639c84
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699806"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Ativar a gestão de subscrição no seu inquilino

Como administrador global do seu diretório, poderá não ter acesso de predefinição para todos os recursos de subscrição no seu inquilino. Este artigo descreve os passos para conceder si próprio acesso a todas as subscrições no seu inquilino. Também fornece uma abordagem recomendada para os restantes em conformidade com quaisquer controlos de segurança que requer a sua organização depois de receber o acesso.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Quem pode ativar a gestão de subscrições no meu diretório?

Cada utilizador atribuído à função de administrador global tem de seguir os passos abaixo para ativar a gestão de subscrição. Depois de ter ativado a gestão de subscrição para si, pode adicionar outros administradores globais que poderão necessitar de recursos, bem como o acesso. Não há nenhuma definição de diretório que permite acesso para todos os membros da função de administrador global.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no portal do Azure com uma conta que seja um membro do Active Directory ou elegível da função de administrador global. Se a conta for um administrador global elegível, terá de ativar primeiro a função antes de passar para o passo seguinte.

## <a name="access-the-azure-active-directory-admin-center"></a>Aceder ao centro de administração do Azure Active Directory

Agora que tem sessão iniciada no portal do Azure como um administrador global, pode editar as definições que fornecem acesso a subscrições do Azure. Navegue para o Centro de administração do Azure Active Directory (Azure AD) e selecione **propriedades**.

![Centro de administração de captura de ecrã do Azure AD, com propriedades realçado](media/azure-pim-resource-rbac/aad_properties.png)

Na lista de propriedades, sob **Administrador Global pode gerir as subscrições do Azure**, selecione **Sim**.

![Página de captura de ecrã de propriedades, com o botão de alternar definido como Sim](media/azure-pim-resource-rbac/aad_properties_save.png)

Agora a sua conta é automaticamente adicionada à função de administrador de acesso de utilizador para todos os recursos de subscrição no inquilino.

## <a name="browse-to-azure-ad-pim"></a>Navegue para o Azure AD PIM

 Aqui, vá para Azure AD Privileged Identity Management (PIM). Em **gerir**, selecione **recursos do Azure**.

![Captura de ecrã do PIM, com recursos do Azure realçados](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Gerir e detetar recursos

Se a sua organização já estiver a utilizar Azure AD PIM para proteger os administradores no Azure AD, pode ver uma lista de subscrições quando carrega o painel.

![Captura de ecrã do PIM, com a lista de subscrições apresentado no painel](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Se não vir quaisquer recursos, confirme se a:
>- A função de administrador global não expirou. 
>- A organização tem uma subscrição do Azure.

![Captura de ecrã do PIM, com a lista de recursos em branco](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Configurar atribuições

Selecione uma subscrição da lista e atribui por si (ou um grupo que é membros) como um proprietário do recurso elegível. 
[Saiba mais sobre a atribuição de funções](pim-resource-roles-assign-roles.md).

Repita este processo para cada recurso antes de avançar para o passo seguinte.

## <a name="clean-up-standing-access"></a>Limpar acesso colocado

Agora que tem atribuições elegíveis para as subscrições importantes na sua organização, pode limpar acesso colocado ao desativar a opção nas propriedades do diretório.

![Página de captura de ecrã de propriedades, com o botão de alternar definido como nenhum](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Passos Seguintes

[Detetar recursos](pim-resource-roles-discover-resources.md)

[Configurar as definições de função](pim-resource-roles-configure-role-settings.md)








