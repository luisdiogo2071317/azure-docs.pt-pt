---
title: Índice de artigos para gestão de aplicações no Azure Active Directory | Microsoft Azure
description: Saiba como personalizar a data de expiração para os certificados de Federação e como renovar certificados que irão expirar em breve.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: cde70518164c386697127b079979b5b3b76ae088
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42055630"
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Índice de Artigos da Gestão da Aplicação no Azure Active Directory
Esta página fornece uma lista abrangente de cada documento escrito sobre os vários recursos relacionados com a aplicação no Azure Active Directory (Azure AD).

Há uma breve introdução para cada área de recursos principais, bem como orientações sobre os artigos para ler consoante as informações que procura.

## <a name="overview-articles"></a>Artigos de descrição geral
Os artigos abaixo são bons pontos de partida para aqueles que desejam simplesmente uma breve explicação de funcionalidades de gestão de aplicações do Azure AD.

| Guia do artigo |  |
|:---:| --- |
| Uma introdução para os problemas de gestão de aplicações do Azure AD resolve |[Gestão de aplicações com o Azure Active Directory (AD)](manage-apps/what-is-application-management.md) |
| Uma descrição geral das várias funcionalidades no Azure AD relacionado com a ativação de início de sessão único, definição de quem tem acesso a aplicações e como os utilizadores iniciar as aplicações |[Acesso a aplicações e início de sessão único no Azure Active Directory](manage-apps/what-is-single-sign-on.md) |
| Vejamos as etapas diferentes envolvidas ao integrar aplicações no seu Azure AD |[Integrar o Azure Active Directory com aplicativos](manage-apps/plan-an-application-integration.md)<br /><br />[Ativar o início de sessão único para aplicações SaaS](manage-apps/configure-single-sign-on-portal.md)<br /><br />[Gerir o acesso a aplicações](manage-apps/what-is-access-management.md) |
| Uma explicação sobre técnica como as aplicações são representadas no Azure AD |[Como e por que os aplicativos são adicionados ao Azure AD](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Artigos de resolução de problemas
Esta seção fornece acesso rápido para guias de resolução de problemas relevantes. Podem encontrar mais informações sobre cada área de recursos no restante desta página.

| Área de recursos |  |
|:---:| --- |
| Início de sessão único federado |[Resolução de problemas com base em SAML início de sessão único](develop/howto-v1-debug-saml-sso-issues.md) |
| Com base em palavra-passe de início de sessão único |[A extensão do painel de acesso de resolução de problemas do Internet Explorer](active-directory-saas-ie-troubleshooting.md) |
| Proxy da aplicação |[Guia de resolução de problemas do Proxy de aplicações](manage-apps/application-proxy-troubleshoot.md) |
| Início de sessão único no local AD e o Azure AD |[Resolução de problemas de sincronização de Hash de palavra-passe](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md#troubleshoot-password-hash-synchronization)<br /><br />[Resolução de problemas de repetição de escrita de palavra-passe](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Associações de grupo dinâmico |[Resolução de problemas de associações de grupo dinâmico](users-groups-roles/groups-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Início de Sessão Único (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Início de sessão único federado: Início de sessão em muitas aplicações com uma identidade
Início de sessão único permite aos utilizadores aceder uma variedade de aplicações e serviços com apenas um conjunto de credenciais. Federação é um método por meio do qual pode ativar o início de sessão único. Quando os utilizadores tentam iniciar sessão em aplicações federadas, eles irão obter redirecionados para oficial início de sessão página da respetiva organização processada pelo Azure Active Directory e, em seguida, são redirecionados para a aplicação quando a autenticação com êxito.

| Guia do artigo |  |
|:---:| --- |
| Uma introdução à Federação e outros tipos de início de sessão |[Início de sessão único com o Azure AD](manage-apps/what-is-single-sign-on.md) |
| Simplificado de milhares de aplicações SaaS previamente integradas com o Azure AD com passos de configuração de início de sessão único |[Guia de introdução da Galeria de aplicações do Azure AD](manage-apps/what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Lista completa das aplicações previamente integradas que oferecem suporte à Federação](saas-apps/tutorial-list.md)<br /><br />[Como adicionar a sua aplicação na Galeria de aplicações do Azure AD](develop/howto-app-gallery-listing.md) |
| Mais de 150 aplicações tutoriais sobre como configurar o início de sessão único para aplicações, tal como [Salesforce](saas-apps/salesforce-tutorial.md), [ServiceNow](saas-apps/servicenow-tutorial.md), [Google Apps](saas-apps/google-apps-tutorial.md), [Workday](saas-apps/workday-tutorial.md)e muitos mais |[Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](saas-apps/tutorial-list.md) |
| Como configurar e personalizar a configuração de início de sessão única manualmente |[Como para configurar federado início de sessão único para aplicações que não estão na Galeria de aplicações de diretório do Azure Active Directory](application-config-sso-how-to-configure-federated-sso-non-gallery.md)<br /><br />[Como personalizar afirmações emitidas no Token SAML para aplicações previamente integradas](active-directory-saml-claims-customization.md) |
| Guia de resolução de problemas para aplicações federadas que utilizam o protocolo SAML |[Resolução de problemas com base em SAML início de sessão único](develop/howto-v1-debug-saml-sso-issues.md) |
| Como configurar a data de expiração do certificado da sua aplicação e como renovar os certificados |[Gestão de certificados para início de sessão único federado no Azure Active Directory](manage-apps/manage-certificates-for-federated-single-sign-on.md) |

Início de sessão único federado está disponível para todas as edições do Azure AD para até dez aplicações por utilizador. [O Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) oferece suporte a aplicativos ilimitados. Se a organização tiver [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) ou [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), em seguida, pode [utilizar grupos para atribuir acesso a aplicações federadas](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Com base em palavra-passe de início de sessão único: conta de partilha e o SSO para aplicações não federadas
Para ativar o início de sessão único para aplicações que não suportam o Federação, o Azure AD oferece funcionalidades de gestão de palavra-passe que podem armazenar em segurança de palavras-passe para aplicações SaaS e inscrevem automaticamente os utilizadores estas aplicações. Pode facilmente distribuir credenciais para contas recentemente criadas e partilhar contas de equipe com várias pessoas. Os usuários não precisam necessariamente de saber as credenciais para as contas que eles estão dado acesso.

| Guia do artigo |  |
|:---:| --- |
| Uma introdução ao funciona SSO como palavra-passe e uma breve visão geral técnica |[Com base em palavra-passe de início de sessão único com o Azure AD](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on) |
| Um resumo dos cenários relacionados com a conta de partilha e como esses problemas são resolvidos pelo Azure AD |[Partilhar contas com o Azure AD](active-directory-sharing-accounts.md) |
| Alterar automaticamente a palavra-passe para determinadas aplicações em intervalos regulares |[Rollover de palavra-passe automatizada (pré-visualização)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Implementação e resolução de problemas de guias para a versão do Internet Explorer de extensão de gestão de palavra-passe do Azure AD |[Como implementar a extensão do painel de acesso para o Internet Explorer usando a diretiva de grupo](active-directory-saas-ie-group-policy.md)<br /><br />[A extensão do painel de acesso de resolução de problemas do Internet Explorer](active-directory-saas-ie-troubleshooting.md) |

Com base em palavra-passe de início de sessão único está disponível para todas as edições do Azure AD para até dez aplicações por utilizador. [O Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) oferece suporte a aplicativos ilimitados. Se a organização tiver [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) ou [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), em seguida, pode [utilizar grupos para atribuir acesso a aplicações](#managing-access-to-applications). Rollover de palavra-passe automatizada é um [do Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funcionalidade.

### <a name="app-proxy-single-sign-on-and-remote-access-to-on-premises-applications"></a>Proxy de aplicação: Único início de sessão e acesso remoto para aplicações no local
Em seguida, se tiver aplicações na sua rede privada que têm de ser acedido por utilizadores e dispositivos fora da rede, pode utilizar o Proxy de aplicações do Azure AD para ativar o acesso remoto seguro às aplicações.

| Guia do artigo |  |
|:---:| --- |
| Descrição geral do Proxy de aplicações do Azure AD e como funciona |[Fornecer acesso remoto seguro a aplicações no local](manage-apps/application-proxy.md) |
| Tutoriais sobre como configurar o Proxy de aplicações e como publicar a sua primeira aplicação |[Como configurar o Proxy de aplicações do Azure AD](manage-apps/application-proxy-enable.md)<br /><br />[Como instalar automaticamente o conector do Proxy de aplicação](manage-apps/application-proxy-register-connector-powershell.md)<br /><br />[Como publicar aplicações através do Proxy de aplicações](manage-apps/application-proxy-publish-azure-portal.md)<br /><br />[Como utilizar o seu próprio nome de domínio](manage-apps/application-proxy-configure-custom-domain.md) |
| Como ativar o acesso de início de sessão e condicional único para aplicações publicadas com o Proxy da aplicação |[Início de sessão único com o Proxy de aplicações](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)<br /><br />[Acesso condicional e de Proxy de aplicações](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Documentação de orientação sobre como utilizar o Proxy de aplicações para os seguintes cenários |[Como suportar aplicações de cliente nativo](manage-apps/application-proxy-configure-native-client-application.md)<br /><br />[Como suportar aplicações com suporte para afirmações](manage-apps/application-proxy-configure-for-claims-aware-applications.md)<br /><br />[Como suportar aplicações publicadas em redes separadas e localizações](manage-apps/application-proxy-connector-groups.md) |
| Guia de resolução de problemas do Proxy de aplicações |[Guia de resolução de problemas do Proxy de aplicações](manage-apps/application-proxy-troubleshoot.md) |

Proxy de aplicações está disponível para todas as edições do Azure AD para até dez aplicações por utilizador. [O Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) oferece suporte a aplicativos ilimitados. Se a organização tiver [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) ou [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), em seguida, pode [utilizar grupos para atribuir acesso a aplicações](#managing-access-to-applications).

Também poderá estar interessado [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), que permite-lhe migrar as suas aplicações no local para o Azure ao mesmo tempo, ainda que satisfaça as necessidades de identidade desses aplicativos.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Ativar o início de sessão único entre o Azure AD e AD no local
Se sua organização mantém um Windows Server Active Directory no local, juntamente com o Azure Active Directory na cloud, em seguida, provavelmente desejará ativar o início de sessão único entre esses dois sistemas. O Azure AD Connect (a ferramenta integra-se esses dois sistemas juntos) fornece várias opções para configurar o início de sessão único: estabelecer federação com o ADFS ou outro fornecedor de Federação ou ativar a sincronização de palavra-passe.

| Guia do artigo |  |
|:---:| --- |
| Uma descrição geral sobre as opções de início de sessão únicas oferecidos no Azure AD Connect, bem como informações sobre como gerir os ambientes híbridos |[Início de sessão do utilizador sobre as opções no Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Orientação geral para o gerenciamento de ambientes com ambos no local do Active Directory e Azure Active Directory |[Considerações de Design de identidade híbrida do Azure AD](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[Integrar as identidades no local com o Azure Active Directory](active-directory-aadconnect.md) |
| Documentação de orientação sobre como utilizar a sincronização de palavra-passe para ativar o SSO |[Implementar a sincronização de palavra-passe com o Azure AD Connect](connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)<br /><br />[Resolver problemas de sincronização de palavra-passe](https://support.microsoft.com/kb/2855271) |
| Documentação de orientação sobre como utilizar a repetição de escrita de palavra-passe para ativar o SSO |[Introdução à gestão de palavra-passe no Azure AD](authentication/quickstart-sspr.md)<br /><br />[Resolver problemas na Repetição de Escrita de Palavras-passe](authentication/active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Documentação de orientação sobre como utilizar fornecedores de identidade de terceiros para ativar o SSO |[Lista de fornecedores de identidade de terceiros compatíveis que podem ser utilizadas para ativar o início de sessão único](https://aka.ms/ssoproviders) |
| Como os utilizadores do Windows 10 podem desfrutar dos benefícios de início de sessão único através de associação do Azure AD |[Junte-se aumentar as capacidades de Cloud para o Windows 10 dispositivos através do Azure Active Directory](active-directory-azureadjoin-overview.md) |

O Azure AD Connect está disponível para [todas as edições do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). O Azure AD reposição personalizada de palavra-passe está disponível para [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) e [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Repetição de escrita de palavra-passe no local AD é um [do Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funcionalidade.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Acesso condicional: Impor requisitos de segurança adicional para aplicações de alto risco
Depois de configurar o início de sessão único às suas aplicações e recursos, pode, em seguida, proteger ainda mais aplicações confidenciais ao impor requisitos de segurança específicos em cada início de sessão para essa aplicação. Por exemplo, pode utilizar o Azure AD para a pedido que todo o acesso a uma aplicação específica necessita sempre de autenticação multifator, independentemente de estar ou não essa aplicação intrinsecamente oferece suporte a essa funcionalidade. Outro exemplo comum de acesso condicional é exigir que os utilizadores ligada a rede fidedigna da organização para poder aceder a uma aplicação particularmente sensível.

| Guia do artigo |  |
|:---:| --- |
| Uma introdução às funcionalidades de acesso condicional oferecidos no Azure AD, Office 365 e Intune |[Gestão de riscos com acesso condicional](active-directory-conditional-access-azure-portal.md) |
| Como ativar o acesso condicional para os seguintes tipos de recursos |[Acesso condicional para aplicações SaaS](conditional-access/app-based-conditional-access.md)<br /><br />[Acesso condicional para serviços do Office 365](active-directory-conditional-access-device-policies.md)<br /><br />[Acesso condicional para aplicações no local](active-directory-conditional-access-azure-portal.md)<br /><br />[Acesso condicional para aplicações no local publicadas através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-integrate-with-sharepoint-server.md) |
| Como registar dispositivos com o Azure Active Directory para ativar políticas de acesso condicional com base no dispositivo |[Descrição geral do registo de dispositivos do Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Como ativar o registo automático de dispositivos para o domínio associado a um dispositivos do Windows](active-directory-conditional-access-automatic-device-registration.md)<br />— [Dispositivos de passos para o Windows 8.1](active-directory-conditional-access-automatic-device-registration-setup.md)<br />— [Dispositivos de passos para o Windows 7](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Como utilizar a aplicação Microsoft Authenticator para verificação de dois passos | [Microsoft Authenticator](user-help/microsoft-authenticator-app-how-to.md) |

Acesso condicional é um [do Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funcionalidade.

## <a name="apps--azure-ad"></a>Aplicações e do Azure AD
### <a name="cloud-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>A cloud Discovery: Encontrar as aplicações de SaaS estão a ser utilizadas na sua organização
Cloud Discovery analisa os seus registos de tráfego no catálogo de aplicações da Microsoft Cloud App Security cloud da cloud mais de 16 000 aplicações que são classificadas e pontuadas com base em mais de 70 fatores de risco, para fornecer visibilidade contínua para cloud utilizar, Shadow IT e o risco Desafio IT cópias sombra em sua organização.

| Guia do artigo |  |
|:---:| --- |
| Uma visão geral de como funciona |[Configurar a Cloud Discovery](/cloud-app-security/set-up-cloud-discovery) |


### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automaticamente provisione e desprovisione contas de utilizador em aplicações SaaS
Automatize a criação, a manutenção e a remoção de identidades de utilizador em aplicações de SaaS, como o Dropbox, Salesforce, ServiceNow e muito mais. Fazer corresponder e sincronização de identidades existentes entre o Azure AD e a aplicações SaaS e controlar o acesso ao desativar automaticamente contas quando os usuários deixam a organização.

| Guia do artigo |  |
|:---:| --- |
| Saiba mais sobre como funciona e encontre respostas a perguntas comuns |[Automatizar o aprovisionamento e desaprovisionamento às aplicações SaaS de utilizador](active-directory-saas-app-provisioning.md) |
| Configurar a forma como informações são mapeadas entre o Azure AD e a sua aplicação SaaS |[Personalizar mapeamentos de atributos](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Escrever expressões para mapeamentos de atributos](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Como ativar o aprovisionamento automatizado para qualquer aplicação que suporte o protocolo SCIM |[Configurar o aprovisionamento automatizado do utilizador para qualquer aplicação SCIM-Enabled](manage-apps/use-scim-to-provision-users-and-groups.md) |
| Como um relatório sobre e resolver problemas de aprovisionamento de utilizadores |[Relatórios sobre o aprovisionamento automático de utilizadores](active-directory-saas-provisioning-reporting.md)<br><br>[Resolução de problemas de aprovisionamento de utilizadores](active-directory-application-provisioning-content-map.md) |
| Limitam que obtém o provisionamento a uma aplicação com base nos seus valores de atributo |[Filtros de âmbito](active-directory-saas-scoping-filters.md) |

Aprovisionamento automatizado do utilizador está disponível para todas as edições do Azure AD para até dez aplicações por utilizador. [O Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) oferece suporte a aplicativos ilimitados. Se a organização tiver [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) ou [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), em seguida, pode [utilizar grupos para gerir os utilizadores que sejam provisionados](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Criando aplicativos que se integram com o Azure AD
Se sua organização está a desenvolver ou a manutenção de linha de negócio (LoB) aplicativos, ou se for um programador de aplicações com os clientes que utilizam o Azure Active Directory, os tutoriais seguintes irão ajudar a integrar seus aplicativos com o Azure AD.

| Guia do artigo |  |
|:---:| --- |
| Orientações para profissionais de TI e desenvolvedores de aplicativos à integração de aplicações com o Azure AD |[O IT Guia do Pro para desenvolver aplicativos para o Azure AD](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[Guia do programador para o Azure Active Directory](develop/azure-ad-developers-guide.md) |
| Como a aplicação fornecedores podem adicionar as suas aplicações para a Galeria de aplicações do Azure AD |[Listar a sua aplicação na Galeria de aplicações de diretório do Azure Active Directory](develop/howto-app-gallery-listing.md) |
| Como gerir o acesso a aplicativos desenvolvidos com o Azure Active Directory |[Como ativar a atribuição de utilizadores para aplicativos desenvolvidos](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Atribuir utilizadores à sua aplicação](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Atribuição de grupo à sua aplicação](active-directory-applications-guiding-developers-assigning-groups.md) |

Se estiver a desenvolver aplicações destinadas ao consumidor, poderá estar interessado em usar [do Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) para que não precisa de desenvolver seu próprio sistema de identidade para gerir os seus utilizadores. [Saiba mais](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-to-applications"></a>Gerir o acesso a aplicações
### <a name="using-groups-and-self-service-to-manage-who-has-access-to-which-apps"></a>Utilização de grupos e Self-Service para gerir quem tem acesso para qual as aplicações
Para ajudar a gerir quem precisa ter acesso a quais recursos, o Azure Active Directory permite-lhe definir permissões de atribuições e à escala através de grupos. IT pode optar por ativar funcionalidades self-service, para que os usuários podem simplesmente solicitar permissão quando precisarem.

| Guia do artigo |  |
|:---:| --- |
| Uma descrição geral das funcionalidades de gestão de acesso do Azure AD |[Introdução à gestão de acesso a aplicações](manage-apps/what-is-access-management.md)<br /><br />[Como funciona a gestão de acesso no Azure AD](fundamentals/active-directory-manage-groups.md)<br /><br />[Como utilizar grupos para gerir o acesso a aplicações SaaS](users-groups-roles/groups-saasapps.md) |
| Ativar a gestão de self-service de aplicações e grupos |[Gestão de aplicações Self-Service](active-directory-self-service-application-access.md)<br /><br />[Gestão de grupos Self-Service](users-groups-roles/groups-self-service-management.md) |
| Instruções para configurar os grupos no Azure AD |[Como criar grupos de segurança](fundamentals/active-directory-groups-create-azure-portal.md)<br /><br />[Como designar os proprietários de um grupo](fundamentals/active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Como utilizar o grupo "Todos os utilizadores"](active-directory-accessmanagement-dedicated-groups.md) |
| Utilizar grupos dinâmicos para preencher automaticamente a associação de grupo usando regras de associação baseadas em atributos |[Associação de grupo dinâmico: Regras avançadas](active-directory-groups-dynamic-membership-azure-portal.md)<br /><br />[Resolução de problemas de associações de grupo dinâmico](users-groups-roles/groups-troubleshooting.md) |

Gestão de acesso de aplicação baseada em grupo está disponível para [do Azure AD básico](https://azure.microsoft.com/pricing/details/active-directory/) e [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Gestão de grupos self-service, gestão de aplicações self-service e grupos dinâmicos são [do Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) funcionalidades.

### <a name="b2b-collaboration-enable-partner-access-to-applications"></a>Colaboração B2B: Ativar o acesso de parceiro a aplicações
Se a sua empresa fez uma parceria com outras empresas, é provável que precisa gerir o acesso de parceiros às suas aplicações empresariais. A colaboração B2B do Active Directory do Azure fornece uma forma fácil e segura para partilhar as suas aplicações com parceiros.

| Guia do artigo |  |
|:---:| --- |
| Uma visão geral do Azure AD diferentes funcionalidades que pode obter ajuda a gerir utilizadores externos, como parceiros, clientes, etc. |[Comparar capacidades para o gerenciamento de identidades externas no Azure AD](active-directory-b2b-compare-external-identities.md) |
| Uma introdução à colaboração B2B e como começar |[Simples, seguro, integração de parceiros da Cloud com o Azure AD](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Colaboração B2B do Azure Active Directory](active-directory-b2b-collaboration-overview.md) |
| Se aprofundar nas colaboração B2B do Azure AD e como usá-lo |[Colaboração B2B: Como funciona](active-directory-b2b-how-it-works.md)<br /><br />[Limitações atuais da colaboração B2B do Azure AD](active-directory-b2b-current-limitations.md)<br /><br />[Instruções detalhadas da utilização de colaboração B2B do Azure AD](active-directory-b2b-detailed-walkthrough.md) |
| Artigos de referência com detalhes técnicos sobre como funciona a colaboração B2B do Azure AD |[Formato de ficheiro CSV para adicionar utilizadores parceiros](active-directory-b2b-references-csv-file-format.md)<br /><br />[Atributos de utilizador afetados pela colaboração B2B do Azure AD](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[Formato de Token de utilizador para os utilizadores do parceiro](active-directory-b2b-references-external-user-token-format.md) |

Colaboração B2B está atualmente disponível para [todas as edições do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Painel de acesso: Um portal para aceder a aplicações e funcionalidades self-service
O painel de acesso do Azure AD é onde os utilizadores finais podem iniciar as aplicações e aceder as funcionalidades self-service que permitem gerir as suas aplicações e associações de grupo. Para além do painel de acesso, as outras opções para aceder a aplicações com o SSO estão incluídas na lista abaixo.

| Guia do artigo |  |
|:---:| --- |
| Uma comparação das diferentes opções disponíveis para a implementação de aplicações de início de sessão únicas para utilizadores |[Implementar o AD do Azure integrado a aplicações para utilizadores](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) |
| Uma visão geral do painel de acesso e o respetivo MyApps equivalentes móveis |[Introdução ao painel de acesso e o My Apps](user-help/active-directory-saas-access-panel-introduction.md)<br />— [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Como aplicações de acesso do Azure AD a partir do site do Office 365 |[Utilizar o iniciador de aplicações do Office 365](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Como aceder a aplicações do Azure AD da aplicação móvel do Intune Managed Browser |[Browser gerido do Intune](https://technet.microsoft.com/library/dn878029.aspx)<br />— [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />— [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Como aceder a aplicações do Azure AD através de ligações avançadas para iniciar o início de sessão único |[Obter ligações diretas início de sessão às suas aplicações](manage-apps/what-is-single-sign-on.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Painel de acesso está disponível para [todas as edições do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-to-apps"></a>Relatórios: Facilmente auditar alterações de acesso da aplicação e monitorizar inícios de sessão para aplicações
O Azure Active Directory fornece vários relatórios e alertas para o ajudar a monitorizar o acesso da sua organização para aplicações. Pode receber alertas para inícios de sessão anómalos às suas aplicações e pode controlar quando e por que o acesso dos utilizadores a uma aplicação foi alterado.

| Guia do artigo |  |
|:---:| --- |
| Uma visão geral dos recursos de relatório no Azure Active Directory |[Introdução aos relatórios do Azure AD](active-directory-reporting-getting-started.md) |
| Como monitorizar a utilização de aplicações dos seus utilizadores e inícios de sessão |[Ver os relatórios de utilização e acesso](active-directory-view-access-usage-reports.md) |
| Controlar as alterações efetuadas a quem pode aceder uma determinada aplicação |[Eventos de relatório de auditoria do Azure Active Directory](active-directory-reporting-audit-events.md) |
| Exportar os dados destes relatórios para suas ferramentas preferidas usando a API de relatórios |[Introdução à API de relatórios do Azure AD](active-directory-reporting-api-getting-started.md) |

Para ver os relatórios estão incluídos em diferentes edições do Azure Active Directory, [clique aqui](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Consulte também
[O que é o Azure Active Directory?](fundamentals/active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Serviços de domínio do Active Directory do Azure](https://azure.microsoft.com/services/active-directory-ds/)

[Autenticação Multifator do Azure](https://azure.microsoft.com/services/multi-factor-authentication/)
