---
title: Próximos passos para a gestão de acesso através de grupos - Azure AD | Documentos da Microsoft
description: Advanced como-cosmética para gestão de grupos de segurança e como utilizar estes grupos para gerir o acesso a um recurso.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: d84c3b556f27366c249382a37e0a8227726110cf
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714689"
---
# <a name="managing-owners-for-a-group"></a>Gerir proprietários de um grupo
Assim que um proprietário do recurso tem atribuído o acesso a um recurso a um grupo do Azure AD, a associação do grupo é gerida pelo proprietário do grupo. O proprietário do recurso delega efetivamente a permissão para atribuir aos utilizadores para o recurso para o proprietário do grupo.

## <a name="add-an-owner-to-a-group"></a>Adicionar proprietário a um grupo

1. Na [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **utilizadores e grupos**.
2. Selecione **todos os grupos**e, em seguida, abra o grupo que pretende adicionar os proprietários.
3. Selecione **adicionar proprietários**.
4. Sobre o **adicionar proprietários** página, selecione o utilizador que pretende adicionar como proprietário deste grupo e certificar-se de que este nome é adicionado à **selecionados** painel.

## <a name="remove-an-owner-from-a-group"></a>Remover um proprietário de um grupo

1. Na [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **utilizadores e grupos**.
2. Selecione **todos os grupos**e, em seguida, abra o grupo a partir do qual pretende remover os proprietários.
3. Selecione o **proprietários** separador.
4. Selecione o proprietário que pretende remover deste grupo e, em seguida, selecione **remover**.

## <a name="additional-information"></a>Informações adicionais
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](active-directory-manage-groups.md)
* [Cmdlets do Azure Active Directory para configurar definições de grupo](../users-groups-roles/groups-settings-cmdlets.md)
* [Gestão de aplicações no Azure Active Directory](../manage-apps/what-is-application-management.md)
* [O que é o Azure Active Directory?](active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](../connect/active-directory-aadconnect.md)
