---
title: Utilização de um grupo para gerir o acesso a aplicações SaaS | Microsoft Docs
description: Como utilizar grupos no Azure Active Directory Premium ou Basic atribuir acesso a aplicações SaaS que estão integradas com o Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/14/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: bdcda54d4e43c81bde587b949cdc12a0237469a9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Utilizar um grupo para gerir o acesso a aplicações SaaS
Utilizar o Azure Active Directory (Azure AD) com uma licença do Azure AD Premium ou Basic do Azure AD, pode utilizar grupos para atribuir acesso a uma aplicação SaaS que está integrada com o Azure AD. Por exemplo, se pretender atribuir acesso para o departamento de marketing utilizar cinco diferentes aplicações SaaS, pode criar um grupo que contenha os utilizadores do departamento de marketing e, em seguida, atribuir esse grupo a estas cinco aplicações SaaS que são necessários pelo departamento de marketing. Desta forma, que pode poupar tempo ao gerir a associação do departamento de marketing num único local. Os utilizadores, em seguida, são atribuídos à aplicação quando estes são adicionados como membros do grupo de marketing, e as respetivas atribuições removidas da aplicação quando os mesmos serão removidos do grupo de marketing. Esta capacidade pode ser utilizada com centenas de aplicações que pode adicionar a partir de dentro da Galeria de aplicações do Azure AD.

> [!IMPORTANT]
> Pode utilizar esta funcionalidade depois de iniciar uma versão de avaliação do Azure AD Premium ou adquirir licenças do Azure AD Premium ou Basic do Azure AD. Atribuição baseada em grupo só é suportada para grupos de segurança. Filiações aninhadas em grupos não são suportadas atualmente para uma atribuição a aplicações baseada em grupos.

**Atribuir acesso para um utilizador ou grupo a uma aplicação SaaS**

1. No [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **aplicações empresariais**.
2. Selecione uma aplicação que adicionou da Galeria de aplicações para abri-lo.
3. Selecione **utilizadores e grupos**e, em seguida, selecione **adicionar utilizador**.
4. No **adicionar atribuição**, selecione **utilizadores e grupos** para abrir o **utilizadores e grupos** lista de seleção.
6. Selecione o número de grupos ou utilizadores como quiser, em seguida, clique ou toque em **selecione** adicioná-los para o **adicionar atribuição** lista. Também pode atribuir uma função a um utilizador nesta fase.
7. Selecione **atribuir** para atribuir os utilizadores ou grupos à aplicação empresarial selecionado.

### <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](active-directory-manage-groups.md)
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)
* [Cmdlets do Azure Active Directory para configurar definições de grupo](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [O que é o Azure Active Directory?](active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md)
