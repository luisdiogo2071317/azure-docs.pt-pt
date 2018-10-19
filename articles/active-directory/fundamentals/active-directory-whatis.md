---
title: O que é o Azure Active Directory (Azure AD)? | Microsoft Docs
description: Saiba como utilizar o Microsoft Azure Active Directory para levar as suas identidades no local existentes para a cloud ou para desenvolver aplicações integradas do Microsoft Azure AD.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 09/13/2018
ms.custom: it-pro
ms.openlocfilehash: 755c301651e7c49faa8b05b2b443c5cce1a03c90
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364061"
---
# <a name="what-is-azure-active-directory"></a>O que é o Azure Active Directory?
O Microsoft Azure Active Directory (Microsoft Azure AD) é o serviço de gestão de identidades e diretórios baseado na cloud multi-inquilino da Microsoft. O Microsoft Azure AD combina serviços de diretório centrais, gestão de acesso da aplicação e proteção de identidade numa única solução, ao oferecer uma plataforma baseada em normas que ajuda os programadores a disponibilizarem o controlo de acesso às suas aplicações, com base em regras e políticas centralizadas.

![Pilha do Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="benefits-of-azure-ad"></a>Benefícios do Microsoft Azure AD
O Microsoft Azure AD ajuda-o a:

-   Criar e gerir uma única identidade para cada utilizador em toda a sua empresa, ao manter os utilizadores, os grupos e os dispositivos sincronizados com o [Azure AD Connect](../connect/active-directory-aadconnect.md).

-   Disponibilizar o acesso de início de sessão único às aplicações, incluindo a milhares de aplicações SaaS pré-integradas, ou a conceder acesso remoto mais seguro a aplicações SaaS no local com o [Proxy de Aplicações do Azure AD](../manage-apps/application-proxy.md).

-   Permitir a segurança do acesso a aplicações mediante a aplicação das políticas da [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) baseada em regras, tanto nas aplicações no local, como na cloud.

-   Melhorar a produtividade dos utilizadores com a [reposição personalizada de palavra-passe](../user-help/user-help-reset-password.md) e com os pedidos de acesso a grupos e aplicações através do [portal MyApps](../user-help/active-directory-saas-access-panel-introduction.md).

-   Tirar partido da [elevada disponibilidade e fiabilidade](https://docs.microsoft.com/azure/architecture/checklist/availability) de uma solução de gestão de acesso e identidades baseada na cloud de nível empresarial e global.

## <a name="who-uses-azure-ad"></a>Quem utiliza o Microsoft Azure AD
O Microsoft Azure AD destina-se aos administradores de TI, programadores de aplicações e aos utilizadores do Office 365, do Azure ou do Dynamics CRM Online.

- **Administradores de TI.** O Azure AD oferece uma solução mais segura à sua organização mediante a utilização de uma gestão de identidades mais forte e de acesso de início de sessão único (SSO) a milhares de [aplicações SaaS baseadas na cloud](../saas-apps/tutorial-list.md) e aplicações no local. Com essas aplicações, também obterá segurança para as aplicações baseada na cloud, acesso ininterrupto, colaboração melhorada e automatização do ciclo de vida das identidades dos seus utilizadores, o que ajuda a aumentar a segurança e a conformidade.

    Além disso, com o [Azure AD Connect](../connect/active-directory-aadconnect-get-started-express.md), pode integrar o Microsoft Azure AD numa instância já existente do Windows Server Active Directory, o que permite à sua organização utilizar os investimentos atuais em identidade no local para gerir o acesso a aplicações SaaS baseadas na cloud.

- **Para programadores de aplicações.** O Microsoft Azure AD ajuda-o a concentrar-se na criação das suas aplicações mediante a integração numa solução de gestão de identidades que é utilizada por milhões de organizações de todo o mundo.

- **Para clientes do Office 365, do Azure ou do Dynamics CRM Online.** Já está a utilizar o Azure AD. Cada inquilino do Office 365, do Azure e do Dynamics CRM Online é, na verdade, um inquilino do Microsoft Azure AD, com o qual pode começar imediatamente a gerir o acesso dos utilizadores às suas aplicações na cloud integradas.

## <a name="how-reliable-is-azure-ad"></a>Até que ponto é o Azure AD fiável?
O design de multi-inquilino, distribuição geográfica e elevada disponibilidade do Microsoft Azure AD significa que pode confiar no mesmo para as necessidades mais críticas do seu negócio. O Microsoft Azure AD é executado em 28 datacenters em todo o mundo com ativação pós-falha automática. Tal significa que, mesmo que um datacenter fique inativo, existem cópias dos dados do seu diretório em, pelo menos, outros dois datacenters dispersos por outras regiões e disponíveis para acesso instantâneo.

Para obter mais informações sobre os contratos de nível de serviço, veja [Contratos de Nível de Serviço](https://azure.microsoft.com/support/legal/sla/).

## <a name="choose-an-edition"></a>Escolher uma edição
Todos os serviços de negócios do Microsoft Online dependem do Microsoft Azure AD para início de sessão e outras necessidades em termos de identidades. Se subscrever algum dos serviços de negócios do Microsoft Online (por exemplo, o Office 365 ou o Microsoft Azure), obterá automaticamente o Microsoft Azure AD com acesso a todas as funcionalidades Gratuitas. Com a edição Azure Active Directory Gratuito, pode gerir utilizadores e grupos, sincronizar com diretórios no local, obter início de sessão único no Azure, no Office 365 e em milhares de aplicações SaaS populares como Salesforce, Workday, Concur, DocuSign, Google Apps, Box, ServiceNow, Dropbox, entre outros. 

Para melhorar a sua implementação do Microsoft Azure AD, também pode adicionar capacidades pagas ao atualizar para as edições do Azure Active Directory Básico, Premium P1 ou Premium P2. As edições pagas do Microsoft Azure AD são criadas por cima do diretório gratuito já existente, ao proporcionar capacidades de nível empresarial que vão desde monitorização personalizada melhorada, relatórios de segurança, Multi-Factor Authentication (MFA) e acesso seguro à sua força de trabalho móvel.

> [!NOTE]
> Para obter as opções de preços destas edições, veja [Preços de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). O Azure Active Directory Premium P1 e Premium P2 e o Azure Active Directory Básico não são suportados na China, atualmente. Para obter mais informações sobre os preços do Azure AD, pode contactar o Fórum do Azure Active Directory.

- **Azure Active Directory Básico.** Concebida a pensar nos trabalhadores de tarefas que precisem de trabalhar na cloud, esta edição oferece acesso a aplicações centradas na cloud e soluções de gestão de identidade personalizada. Com a edição Básica, beneficia de funcionalidades de otimização de produtividade e de redução de custos, como gestão de acesso baseado em grupos, reposição de palavras-passe personalizada para aplicações na cloud, e do Proxy de Aplicações do Azure Active Directory (para publicar aplicações Web no local com o Microsoft Azure AD), com o apoio de um SLA empresarial de 99,9% de tempo de atividade.

- **Azure Active Directory Premium P1.** Concebida para capacitar as organizações com necessidades de gestão de identidades e acesso mais exigentes, a edição Azure Active Directory Premium acrescenta capacidades de gestão de identidades com funcionalidades avançadas e de nível empresarial e permite que os utilizadores de soluções híbridas acedam sem interrupções a recursos no local e na cloud. Esta edição inclui tudo aquilo de que precisa para os técnicos de informações e os administradores de identidades em ambientes híbridos, desde acesso de aplicações, gestão de identidades e acesso (IAM) self-service, proteção de identidades a segurança na cloud. Suporta recursos avançados de administração e delegação, como grupos dinâmicos e gestão de grupos personalizada. Inclui o Microsoft Identity Manager (um conjunto de aplicações de gestão de identidades e acesso no local) e oferece capacidades de repetição de escrita de palavras-passe que permitem soluções como a reposição de palavras-passe para os utilizadores no local.

- **Azure Active Directory Premium P2.** Concebida em torno da proteção avançada para os utilizadores e administradores, esta oferta nova inclui todas as capacidades do Azure AD Premium P1, bem como o Identity Protection e o Privileged Identity Management. O Azure Active Directory Identity Protection tira partido de milhares de milhões de sinais para proporcionar acesso condicional com base em riscos às suas aplicações e a dados críticos da empresa. Também o ajudamos a gerir e a proteger as contas com privilégios com o Azure Active Directory Privileged Identity Management, para que possa detetar, restringir e monitorizar os administradores e o acesso daqueles a recursos, assim como disponibilizar acesso just-in-time quando for necessário.  

> [!NOTE]
> As edições “pay as you go” também incluem diversas capacidades do Azure Active Directory:<ul><li>**Azure Active Directory B2C.** Solução de gestão de identidades e acesso para as suas aplicações direcionadas para consumidores. Para obter mais informações, veja [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).</li><li>**Multi-Factor Authentication do Azure.** Foi utilizado o fornecedor por utilizador ou por autenticação. Para obter mais informações, veja [O que é a Multi-Factor Authentication do Azure?](../authentication/multi-factor-authentication.md).

## <a name="as-an-admin-how-do-i-get-started"></a>Como administrador, como posso começar?
Inscreva-se numa avaliação gratuita de 30 dias e implemente a primeira solução cloud, veja [avaliação gratuita do Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/).

## <a name="as-a-developer-how-do-i-get-started"></a>Como programador, como posso começar?
Inscreva-se numa avaliação gratuita de 30 dias e comece a integrar as suas aplicações com o Azure AD, veja [versão de avaliação do Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/). Para obter mais informações, também pode ver o [Guia de Programadores](../develop/azure-ad-developers-guide.md) para o Azure Active Directory.

## <a name="next-steps"></a>Passos seguintes
- [Learn more about the fundamentals of Azure identity and access management](identity-fundamentals.md) (Saiba mais sobre as noções básicas da gestão de identidades e acesso do Azure).

- [Integrar o Azure AD com o Windows Server Active directory](../hybrid/how-to-connect-install-express.md).