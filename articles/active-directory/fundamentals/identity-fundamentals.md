---
title: Quais são os conceitos básicos da gestão de identidades e acessos do Azure? -O azure Active Directory | Documentos da Microsoft
description: Saiba mais sobre as capacidades de proteção avançada e ferramentas adicionais que estão disponíveis com as edições do Azure Active Directory Premium.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734680"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>Quais são os conceitos básicos da gestão de identidades e acessos do Azure?
O Azure AD Premium é um com base na cloud identidades e acessos solução de gestão, com capacidades de proteção avançada. Estas capacidades avançadas de ajudar a fornecer uma identidade de segura para todas as suas aplicações, proteção de identidade (aprimorada com o [gráfico de segurança do Microsoft inteligência](https://www.microsoft.com/security/intelligence)), e [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md). O Azure AD ajuda a proteger as identidades dos seus utilizadores em tempo real, ajudando-o a criar com base no risco e políticas de acesso adaptáveis em todos os dados da sua organização.

Assista este breve vídeo e veja uma descrição geral da gestão e da proteção de identidades do Azure AD.
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

O Azure AD também fornece um conjunto de ferramentas que podem ajudar a proteger, automatizar e gerir o seu ambiente, incluindo a reposição de palavras-passe, gestão de utilizadores e de grupo e pedidos de aplicação. O Azure AD pode também ajudar a gerir dispositivos pertencentes ao utilizador e o acesso e o controlo de Software como aplicações de serviço (SaaS).

Para obter mais informações sobre os custos de edições do Azure Active Directory Premium e as ferramentas associadas, consulte [preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Ligar o Active Directory no local ao Azure AD e ao Office 365
Expandir a sua implementação do Active Directory no local para a cloud com a integração dos diretórios no local com o Azure AD através de [gestão de identidade híbrida](https://aka.ms/aadframework). [O Azure AD Connect](../connect/active-directory-aadconnect.md) fornece esta integração, fornecer aos utilizadores uma única identidade e início de sessão único (SSO) acesso a seus recursos no local e seus serviços cloud, como o Office 365.

O Azure AD Connect substitui as versões mais antigas ferramentas de integração de identidade, como o DirSync e Azure AD Sync, ajudando a suportar as suas necessidades de sincronização de identidade no local e o Azure AD. Sincronização do Azure AD Connect é ativada através de:

- **Sincronização.** Responsável por criar utilizadores, grupos e outros objetos. Também é responsável por certificar-se de que as informações de identidade para os seus utilizadores no local corresponde ao que está no Azure AD. Ativar a repetição de escrita de palavras-passe também ajuda a manter os seus diretórios no local sincronizado quando os usuários atualizam as palavras-passe no Azure AD.

- Autenticação. Escolher o método de autenticação direita é importante quando configurar a sua solução de identidade híbrida do Azure AD. Pode escolher a autenticação na nuvem (sincronização de Hash de palavra-passe / autenticação pass-through) ou autenticação federada (AD FS) para a sua organização. Para obter mais informações sobre as opções disponíveis, consulte [escolha o método de autenticação correta para sua solução de identidade híbrida do Azure Active Directory](https://aka.ms/auth-options).

- **Monitorização de estado de funcionamento.** O Azure AD Connect Health fornece monitorização e um local central no portal do Azure para visualizar esta atividade. Para obter mais informações, consulte [Monitor your on-premises identity infrastructure and synchronization services in the cloud (Monitorizar os serviços de infraestrutura de identidade no local e sincronização na cloud)](../connect-health/active-directory-aadconnect-health.md).

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Aumentar a produtividade e reduzir os custos de suporte técnico com as experiências de self-service e início de sessão único
Os utilizadores poupar tempo em que é necessário um nome de utilizador único e a palavra-passe, juntamente com uma experiência consistente em todos os dispositivos. Os utilizadores também poupar tempo ao realizar tarefas de Self-serviços, como[repor uma palavra-passe esquecida](../user-help/active-directory-passwords-update-your-own-password.md) ou pedir acesso a uma aplicação sem ter de esperar de assistência do suporte técnico.

Promovendo o SSO e uma experiência consistente, do Azure AD [expande o seu Active Directory no local](../connect/active-directory-aadconnect.md) para a cloud, permitindo que os utilizadores utilizam suas contas institucionais primária para os respetivos dispositivos associados a um domínio, recursos da empresa, e todas as aplicações de SaaS e web têm de utilizar para fazer um trabalho. 

Além disso, o acesso de aplicativo pode ser automaticamente aprovisionado (ou desaprovisionado) com base em associações de grupo e o estado de funcionário de um utilizador, ajudando-o controlar o acesso a aplicações de galeria ou as suas aplicações no local que já desenvolveu e publicado por meio de o [Proxy de aplicações do Azure AD](../manage-apps/application-proxy.md).

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Gerir e controlar o acesso aos recursos da sua organização
Soluções de gestão de identidades e acessos do Microsoft ajudá-lo a proteger o acesso a aplicações e recursos entre o datacenter da sua organização e para a cloud. Esta gestão de acesso ajuda a fornecer níveis adicionais de validação como [multi-factor Authentication](../authentication/concept-mfa-howitworks.md) e [políticas de acesso condicional](../conditional-access/overview.md). Atividade suspeita através de monitorização avançada de relatórios de segurança, auditoria, e alertas também podem ajudar a mitigar potenciais problemas de segurança.

Utilizar políticas de acesso condicional no Azure AD Premium permite-lhe criar regras de acesso baseado em políticas para qualquer aplicação do Azure AD-ligado, tais como aplicações de SaaS, aplicações personalizadas em execução na cloud ou no local ou aplicações web). O Azure AD avalia as regras em tempo real, impondo-las sempre que um utilizador tenta aceder a uma aplicação. As políticas de proteção de identidade do Azure permitem-lhe automaticamente tome medidas (por bloquear o acesso, a imposição de multi-factor Authentication ou repor palavras-passe de utilizador) se for detetada atividade suspeita.

## <a name="azure-active-directory-privileged-identity-management"></a>Gestão de Identidades Privilegiadas do Azure Active Directory
[Privileged Identity Management (PIM)](../privileged-identity-management/pim-getting-started.md), incluído com a edição Azure Active Directory Premium 2, ajuda-o a detetar, restringir e monitorizar as contas administrativas e respetivo acesso a recursos no seu Azure Active Directory e outros Serviços online da Microsoft. PIM também o ajuda a administrar o acesso administrativo a pedido para o período de exato do tempo que achar necessário, o que significa que pode permitir que os administradores solicitem multifator autenticado, elevação temporária de seus privilégios para um período de tempo pré-configurado antes de suas contas de retornam a um Estado de usuário normal.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a arquitetura do Azure AD, consulte [qual é a arquitetura do Azure AD?](active-directory-architecture.md).
