---
title: Ver os membros de uma função de administrador no Azure Active Directory | Documentos da Microsoft
description: Agora, pode ver e gerir os membros de uma função de administrador no portal do Azure AD. Para aqueles que freqüentemente gerenciam as atribuições de funções.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/02/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 588e475a451ea00e52dd995edc3f1bf0e284044f
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872329"
---
# <a name="view-all-members-of-an-administrator-role-in-azure-active-directory"></a>Ver todos os membros de uma função de administrador no Azure Active Directory

Agora, pode ver e gerir todos os membros das funções de administrador no portal do Azure Active Directory. Se gerir atribuições de funções com freqüência, provavelmente preferirá esta experiência. E se já pensou em "O que burrice a minha estas funções realmente fazer?", pode ver uma lista detalhada das permissões para cada uma das funções de administrador do Azure AD.

É fácil ver também suas próprias permissões. Clique em **sua função** obtenha acesso rápido à sua página de utilizador para uma lista de todas as funções atribuídas Active Directory. Clique nas reticências à direita de cada linha para abrir a descrição detalhada da função.

![lista de funções no portal do Azure AD](./media/directory-manage-roles-portal/role-list.png)

Selecione a linha inteira para ver a lista de membros atribuídas. Pode selecionar **gerir no PIM** para capacidades de gestão adicionais. Os administradores de função com privilégios, pode alterar "Permanente" (sempre ativa na função) atribuições para "Elegíveis" (na função apenas quando elevados). Se não tiver o PIM, pode ainda selecionar **gerir no PIM** para se inscrever para uma versão de avaliação. Privileged Identity Management requer uma [plano de licenciamento do Azure AD Premium P2](../privileged-identity-management/subscription-requirements.md).

![lista de membros de uma função de administrador](./media/directory-manage-roles-portal/member-list.png)

Se for um Administrador Global ou um administrador com função privilegiada, pode facilmente adicionar ou remover membros, filtrar a lista ou selecione um membro para ir para a página de utilizador para ver os seus ativos atribuídos a funções. 

## <a name="role-details-in-the-portal"></a>Detalhes da função no portal

Quando estiver a ver os membros de uma função, selecione **Descrição** para ver uma lista completa das permissões concedidas pela atribuição de função. A página inclui ligações para documentação relevante para o ajudar a orientá-lo a gerir funções de diretório.

![lista de permissões para uma função de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Passos Seguintes

* Sinta-se à vontade para compartilhar na [fórum de funções administrativas do Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obter mais informações sobre as funções e atribuição de função de administrador, consulte [atribuir funções de administrador](directory-assign-admin-roles.md).
* Para permissões de utilizador predefinidas, consulte um [comparação de permissões de utilizador do convidado e o membro predefinido](../fundamentals/users-default-permissions.md).
