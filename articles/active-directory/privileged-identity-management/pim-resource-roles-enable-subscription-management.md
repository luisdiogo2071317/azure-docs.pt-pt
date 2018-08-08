---
title: Privileged Identity Management para recursos do Azure - ativar a gestão de subscrição | Documentos da Microsoft
description: Saiba os administradores globais como pode gerir as subscrições no inquilino.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d7a9c0090d3bfaaf1161b6255c4c0b659c77d692
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620285"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Ativar a gestão de subscrição no seu inquilino

Como administrador global do seu diretório, poderá não ter acesso de predefinido a todos os recursos de subscrição no seu inquilino. Este artigo descreve os passos para dar acesso a todas as subscrições no seu inquilino. Ele também fornece uma abordagem recomendada para restantes em conformidade com quaisquer controles de segurança a sua organização obriga depois de receber acesso.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Quem pode ativar a gestão de subscrições no meu diretório?

Cada utilizador atribuído à função de administrador global tem de seguir os passos abaixo para ativar a gestão de subscrição. Depois de ter ativado a gestão de subscrições por conta própria, pode adicionar outros administradores globais que poderão ter o recurso também o acesso. Não existe qualquer definição de diretório que permite o acesso para todos os membros da função de administrador global.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no portal do Azure com uma conta que seja um membro elegível ou Active Directory da função de administrador global. Se a conta for um administrador global elegível, primeiro tem de ativar a função antes de passar para o passo seguinte.

## <a name="access-the-azure-active-directory-admin-center"></a>Aceder ao centro de administração do Azure Active Directory

Agora que tem sessão iniciada no portal do Azure como um administrador global, pode editar as definições que fornecem acesso a subscrições do Azure. Navegue para o Centro de administração do Azure Active Directory (Azure AD) e selecione **propriedades**.

![Centro de administração de captura de ecrã do Azure AD, com propriedades realçado](media/azure-pim-resource-rbac/aad_properties.png)

Na lista de propriedades, sob **Administrador Global pode gerir as subscrições do Azure**, selecione **Sim**.

![Página de captura de ecrã de propriedades, com o botão definido como Yes](media/azure-pim-resource-rbac/aad_properties_save.png)

Agora a sua conta é automaticamente adicionada à função de administrador de acesso de utilizador para todos os recursos de subscrição no inquilino.

## <a name="browse-to-azure-ad-pim"></a>Navegue até o PIM do Azure AD

 A partir daqui, vá para o Azure AD Privileged Identity Management (PIM). Sob **Manage**, selecione **recursos do Azure**.

![Captura de ecrã do PIM, com recursos do Azure realçados](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Gerir e detetar recursos

Se sua organização já utiliza o Azure AD PIM para proteger os administradores no Azure AD, pode ver uma lista de subscrições, quando o painel for carregada.

![Captura de ecrã do PIM, com a lista de subscrições mostrado no painel](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Se não vir quaisquer recursos, confirme se a:
>- A função de administrador global não expirou. 
>- Sua organização tiver uma subscrição do Azure.

![Captura de ecrã do PIM, com a lista de recursos vazio](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Configurar atribuições

Selecione uma subscrição na lista e atribui-se (ou um grupo que for um membro da) como elegível proprietário do recurso. 
[Saiba mais sobre a atribuição de funções](pim-resource-roles-assign-roles.md).

Repita este processo para cada recurso antes de prosseguir para o passo seguinte.

## <a name="clean-up-standing-access"></a>Limpar acesso permanente

Agora que tem atribuições elegíveis para as subscrições importantes na sua organização, é possível limpar acesso permanente ao desativar a opção nas propriedades do diretório.

![Página de captura de ecrã de propriedades, com o botão definido como não](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Passos Seguintes

[Detetar recursos](pim-resource-roles-discover-resources.md)

[Configurar as definições de função](pim-resource-roles-configure-role-settings.md)








