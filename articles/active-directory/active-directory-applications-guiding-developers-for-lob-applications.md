---
title: Registar a sua aplicação para utilizar o Azure Active Directory | Documentos da Microsoft
description: Escrito para profissionais de TI, este artigo fornece diretrizes para a integração de aplicações do Azure com o Active Directory.
services: active-directory
documentationcenter: ''
author: kgremban
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2018
ms.author: kgremban
ms.custom: seohack1
ms.openlocfilehash: 41829add17f69c5f051618195247ed2a5512ca64
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505641"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Desenvolver aplicações de linha de negócio para o Azure Active Directory
Este guia fornece uma descrição geral do desenvolvimento de linha de negócio (LoB) aplicativos para o Azure Active Directory (AD). O público-alvo é que os administradores globais do Active Directory/Office 365.

## <a name="overview"></a>Descrição geral
Criação de aplicações integradas no Azure AD proporciona aos utilizadores na sua organização início de sessão único com o Office 365. Ver o aplicativo no permite o Azure AD que controle sobre a política de autenticação para a aplicação. Para saber mais sobre como proteger aplicações com autenticação multifator (MFA) Consulte e de acesso condicional [configurando as regras de acesso](active-directory-conditional-access-azure-portal-get-started.md).

Registre-se a sua aplicação para utilizar o Azure Active Directory. Registar a aplicação significa que os programadores podem utilizar o Azure AD para autenticar os utilizadores e pedidos de acesso a recursos de utilizador, tais como e-mail, calendário e os documentos.

Qualquer membro do seu diretório (não convidados) pode registar uma aplicação, também conhecida como *criação de um objeto de aplicativo*.

Registo de aplicações permite que qualquer utilizador fazer o seguinte:

* Obtenha uma identidade para seu aplicativo que reconhece do Azure AD
* Obter um ou mais segredos/chaves que a aplicação pode utilizar para autenticar-se ao AD
* Marca a aplicação no portal do Azure com um nome personalizado, logotipo, etc.
* Aplicar funcionalidades de autorização do Azure AD à sua aplicação, incluindo:

  * Controlo de Acesso Baseado em Funções (RBAC)
  * O Azure Active Directory como servidor de autorização de oAuth (proteger uma API exposta pela aplicação)
* Declare as permissões necessárias necessárias para a aplicação funcione conforme esperado, incluindo:

      - Permissões de aplicações (apenas administradores globais). Por exemplo: associação de função na outra do Azure AD aplicação ou função de associação em relação um recurso de Azure, grupo de recursos ou subscrição
      - Permissões delegadas (qualquer usuário). Por exemplo: Azure AD, início de sessão e ler perfil

> [!NOTE]
> Por padrão, qualquer membro pode registar uma aplicação. Para saber como restringir as permissões para registar aplicações para membros específicos, consulte [como os aplicativos são adicionados ao Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Eis o que, o administrador global, precisa de fazer para ajudar os desenvolvedores a tornar seu aplicativo pronto para produção:

* Configurar regras de acesso (política de acesso/MFA)
* Configurar a aplicação para exigir a atribuição de utilizadores e atribuir utilizadores
* Suprimir a experiência de consentimento de utilizador padrão

## <a name="configure-access-rules"></a>Configurar regras de acesso
Configure regras de acesso por aplicação para as suas aplicações de SaaS. Por exemplo, pode exigir a MFA ou permitir apenas o acesso a utilizadores em redes fidedignas. Os detalhes para isso estão disponíveis no documento [configurando as regras de acesso](active-directory-conditional-access-azure-portal-get-started.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Configurar a aplicação para exigir a atribuição de utilizadores e atribuir utilizadores
Por predefinição, os usuários podem acessar aplicativos sem que está sendo atribuído. No entanto, se a aplicação expõe funções, ou se pretender que o aplicativo para aparecer no painel de acesso de um utilizador, deve solicitar a atribuição de utilizadores.

[Necessidade de atribuição de utilizadores](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Se for um assinante do Azure AD Premium ou Enterprise Mobility Suite (EMS), recomendamos vivamente utilizar grupos. Atribuir grupos à aplicação permite-lhe delegar a gestão de acesso em curso para o proprietário do grupo. Pode criar o grupo ou peça a parte responsável na sua organização para criar o grupo com suas instalações de gestão de grupo.

[Atribuir utilizadores a uma aplicação](active-directory-applications-guiding-developers-assigning-users.md)  
[Atribuir grupos a uma aplicação](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Suprimir o consentimento do utilizador
Por predefinição, cada utilizador vai através de uma experiência de consentimento para iniciar sessão. A experiência de consentimento, pedir aos utilizadores para conceder permissões a uma aplicação, pode ser desconcertante para os utilizadores que não estão familiarizados com essas decisões.

Para aplicativos que confia, pode simplificar a experiência do usuário ao que consinta a aplicação em nome da sua organização.

Para obter mais informações sobre o consentimento do utilizador e o consentimento de experiência no Azure, veja [integrar aplicações com o Azure Active Directory](develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="related-articles"></a>Artigos relacionados
* [Ativar o acesso remoto seguro a aplicações no local com o Proxy de aplicações do Azure AD](manage-apps/application-proxy.md)
* [Pré-visualização de acesso condicional do Azure para aplicações SaaS](active-directory-conditional-access-azure-portal-get-started.md)
* [Gerir o acesso a aplicações com o Azure AD](manage-apps/what-is-access-management.md)
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)
