---
title: Utilizar um grupo para gerir o acesso a aplicações SaaS | Documentos da Microsoft
description: Como utilizar os grupos no Azure Active Directory Premium ou Basic atribuir acesso a aplicações SaaS que são integradas no Azure Active Directory.
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
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 5d8e6c0db2ce6c35935c2b00aa40de2a5d5a050f
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297627"
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Utilizar um grupo para gerir o acesso a aplicações SaaS
Utilizar o Azure Active Directory (Azure AD) com uma licença do Azure AD Premium ou do Azure AD básico, pode utilizar grupos para atribuir acesso a uma aplicação SaaS que está integrada com o Azure AD. Por exemplo, se pretender atribuir acesso para o departamento de marketing utilizar cinco aplicações SaaS diferentes, pode criar um grupo que contenha os utilizadores do departamento de marketing e, em seguida, atribua esse grupo para estas cinco aplicativos de SaaS que são necessários para o departamento de marketing. Desta forma, que pode poupar tempo gerenciando a associação do departamento de marketing num único local. Os utilizadores, em seguida, são atribuídos à aplicação quando são adicionados como membros do grupo de marketing, e tem suas atribuições removidas da aplicação quando os mesmos serão removidos do grupo de marketing. Esse recurso pode ser usado com centenas de aplicativos que pode adicionar a partir de Galeria de aplicações do Azure AD.

> [!IMPORTANT]
> Pode utilizar esta funcionalidade apenas depois de iniciar uma versão de avaliação do Azure AD Premium ou comprar licenças do Azure AD Premium ou do Azure AD básico. Atribuição baseada em grupo é suportada apenas para grupos de segurança. Filiações aninhadas em grupos não são suportadas atualmente para uma atribuição a aplicações baseada em grupos.

**Atribuir acesso para um utilizador ou grupo a uma aplicação SaaS**

1. Na [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **aplicações empresariais**.
2. Selecione uma aplicação que adicionados a partir da Galeria de aplicações para abri-lo.
3. Selecione **utilizadores e grupos**e, em seguida, selecione **adicionar utilizador**.
4. No **adicionar atribuição**, selecione **utilizadores e grupos** para abrir o **utilizadores e grupos** lista de seleção.
6. Selecione os grupos ou utilizadores que desejar, em seguida, clique ou toque em **selecionar** para adicioná-los para o **adicionar atribuição** lista. Também pode atribuir uma função a um utilizador nesta fase.
7. Selecione **atribuir** para atribuir utilizadores ou grupos para o aplicativo empresarial selecionado.

### <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Gerir o acesso aos recursos com grupos do Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Gestão de aplicações no Azure Active Directory](../manage-apps/what-is-application-management.md)
* [Cmdlets do Azure Active Directory para configurar definições de grupo](groups-settings-cmdlets.md)
* [O que é o Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Integrar as identidades no local ao Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
