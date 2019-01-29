---
title: Ver e atribuir permissões de função no Azure Active Directory de administrador | Documentos da Microsoft
description: Agora, pode ver e gerir os membros de uma função de administrador no portal do Azure AD. Para aqueles que freqüentemente gerenciam as atribuições de funções.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/06/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 267b9f7c6dcdc994345371a0884db3c1271f56bd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198168"
---
# <a name="view-and-assign-administrator-roles-in-azure-active-directory"></a>Ver e atribuir funções de administrador no Azure Active Directory

Agora, pode ver e gerir todos os membros das funções de administrador no portal do Azure Active Directory. Se gerir atribuições de funções com freqüência, provavelmente preferirá esta experiência. E se já pensou em "O que burrice a minha estas funções realmente fazer?", pode ver uma lista detalhada das permissões para cada uma das funções de administrador do Azure AD.

## <a name="view-all-roles"></a>Ver todas as funções

No Azure Active Directory, selecione **funções e os administradores** para ver a lista de todas as funções disponíveis. 

Clique nas reticências à direita de cada linha para abrir a descrição detalhada da função.

![lista de funções no portal do Azure AD](./media/directory-manage-roles-portal/role-list.png)

## <a name="view-my-roles"></a>Ver as minhas funções

É fácil ver também suas próprias permissões. Selecione **função** sobre o **funções e os administradores** página para ver as funções que estão atualmente atribuídas ao utilizador.

## <a name="view-assignments-for-a-role"></a>Atribuições de exibição para uma função

Clique numa função para ver os utilizadores atribuídos à função. Pode selecionar **gerir no PIM** para capacidades de gestão adicionais. Os administradores de função com privilégios, pode alterar "Permanente" (sempre ativa na função) atribuições para "Elegíveis" (na função apenas quando elevados). Se não tiver o PIM, pode ainda selecionar **gerir no PIM** para se inscrever para uma versão de avaliação. Privileged Identity Management requer uma [plano de licenciamento do Azure AD Premium P2](../privileged-identity-management/subscription-requirements.md).

![lista de membros de uma função de administrador](./media/directory-manage-roles-portal/member-list.png)

Se for um Administrador Global ou um administrador com função privilegiada, pode facilmente adicionar ou remover membros, filtrar a lista ou selecione um membro para ver os seus ativos atribuídos a funções.

## <a name="view-a-users-role-permissions"></a>Ver permissões de função de um utilizador

Quando estiver a ver os membros de uma função, selecione **Descrição** para ver uma lista completa das permissões concedidas pela atribuição de função. A página inclui ligações para documentação relevante para o ajudar a orientá-lo a gerir funções de diretório.

![lista de permissões para uma função de administrador](./media/directory-manage-roles-portal/role-description.png)


## <a name="next-steps"></a>Passos Seguintes

* Sinta-se à vontade para compartilhar na [fórum de funções administrativas do Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obter mais informações sobre as funções e atribuição de função de administrador, consulte [atribuir funções de administrador](directory-assign-admin-roles.md).
* Para permissões de utilizador predefinidas, consulte um [comparação de permissões de utilizador do convidado e o membro predefinido](../fundamentals/users-default-permissions.md).
