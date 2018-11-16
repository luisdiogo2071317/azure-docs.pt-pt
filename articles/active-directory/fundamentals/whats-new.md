---
title: Novidades Notas de versão para o Azure AD | Documentos da Microsoft
description: Saiba quais são as novidades no Azure Active Directory (Azure AD), por exemplo, as notas de versão mais recente, problemas conhecidos, correções de erros, funcionalidades preteridas e alterações futuras.
services: active-directory
author: eross-msft
manager: mtillman
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: 1377280cf3b7272d710b47322b9b27ff8cb36327
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707168"
---
# <a name="whats-new-in-azure-active-directory"></a>O que há de novo no Azure Active Directory?

>Ser notificado sobre quando rever esta página para obter atualizações adicionando isso [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us/) para sua ![ícone RSS](./media/whats-new/feed-icon-16x16.png) feed leitor.

O Azure AD recebe melhorias de forma contínua. Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

- Versões mais recentes
- Problemas conhecidos
- Correções de erros
- Funcionalidades preteridas
- Planos para que as alterações

Esta página é atualizada mensalmente, por isso, revisitá-lo regularmente. Se estiver procurando por itens que têm mais de 6 meses, pode encontrá-los no [arquivada para o que há de novo no Azure Active Directory](whats-new-archive.md).

---
## <a name="october-2018"></a>Outubro de 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Registos do Azure AD agora trabalham com o Azure Log Analytics (pré-visualização pública)

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório

Temos o prazer de anunciar que agora pode reencaminhar os registos do Azure AD para o Azure Log Analytics! Esta funcionalidade mais pedidas ajuda dá a acesso ainda melhor para análise para o seu negócio, operações e segurança, bem como uma forma de ajudar a monitorizar a sua infraestrutura. Para obter mais informações, consulte a [registos de atividade do Active Directory do Azure no Azure Log Analytics agora disponível](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) blog.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD - Outubro de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Em Outubro de 2018, adicionámos que suportam a 14 novos aplicativos com a Federação para a Galeria de aplicações:

[Meus pontos de prémio](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), teclado, [ambiente Virtual de ON24](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler três](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot controle](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial). Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Notificações por correio eletrónico dos serviços de domínio do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** Azure AD Domain Services  
**Capacidade de produto:** Azure AD Domain Services

O Azure AD Domain Services fornece alertas no portal do Azure sobre configurações incorretas ou problemas com o seu domínio gerido. Estes alertas incluem guias passo a passo para poder experimentar corrigir os problemas sem ter de contactar o suporte.

A partir de Outubro, poderá personalizar as definições de notificação para o seu domínio gerido, de modo quando ocorrem novos alertas, é enviado um e-mail para um grupo designado de pessoas, eliminando a necessidade de verifique constantemente o portal para atualizações.

Para obter mais informações, consulte [definições de notificação no Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>O Azure AD portal suporta através do domínio de ForceDelete API para eliminar os domínios personalizados 

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** gerenciamento de diretório  
**Capacidade de produto:** diretório

É com prazer que Anunciamos que agora pode utilizar o domínio de ForceDelete API para eliminar os nomes de domínio personalizado ao modo assíncrono mudar o nome de referências, como os utilizadores, grupos e aplicações do seu nome de domínio personalizado (contoso.com) para o (de nome de domínio predefinido inicial contoso.onmicrosoft.com).

Esta alteração ajuda a eliminar mais rapidamente os seus nomes de domínio personalizado se sua organização já não utiliza o nome, ou se precisar de utilizar o nome de domínio com o outro Azure AD.

Para obter mais informações, consulte [eliminar um nome de domínio personalizado](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>Setembro de 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Permissões de função de administrador atualizadas para grupos dinâmicos

**Tipo:** fixo  
**Categoria de serviço:** grupo de gestão  
**Capacidade de produto:** colaboração

Corrigimos um problema para que as funções de administrador específico podem criar e atualizar regras de associação dinâmica, sem terem de ser o proprietário do grupo.

As funções são:

- Administrador global ou de gravador de empresa

- Administrador de Serviços do Intune

- Administrador de Conta de Utilizador

Para obter mais informações, consulte [criar um grupo dinâmico e verificar o Estado](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Definições de configuração do Início de Sessão Único (SSO) simplificado para algumas aplicações de terceiros

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO

Sabemos que a definição de cópia de segurança único início de sessão (SSO) para Software como serviço (SaaS) aplicações podem ser um desafio devido à natureza exclusiva de cada configuração de aplicações. Criámos uma experiência de configuração simplificada para as definições de configuração de SSO para as seguintes aplicações de SaaS de terceiros de preencher automaticamente:

- Zendesk

- ArcGis Online

- O Jamf Pro

Para começar a utilizar esta experiência de um clique, vá para o **portal do Azure** > **configuração do SSO** página da aplicação. Para obter mais informações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>O Azure Active Directory - onde a seus dados estão localizados? página

**Tipo:** novo recurso  
**Categoria de serviço:** outros  
**Capacidade de produto:** GoLocal

Selecione a região da sua empresa a partir da **do Azure Active Directory - onde a seus dados estão localizados** página para ver qual datacenter do Azure armazena os dados do Azure AD em rest para todos os serviços do Azure AD. Pode filtrar as informações por específicos de serviços do Azure AD para a região da sua empresa.

Para aceder a esta funcionalidade e para obter mais informações, consulte [do Azure Active Directory - onde a seus dados estão localizados](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Novo plano de implementação disponível para o painel Acesso do My Apps

**Tipo:** novo recurso  
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** SSO

Confira o novo plano de implementação que está disponível para o painel de acesso de aplicações My (http://aka.ms/deploymentplans).
O painel de acesso de aplicações My fornece aos usuários um único local para encontrar e aceder às suas aplicações. Esse portal também fornece aos usuários oportunidades de Self-serviços, tais como pedir acesso a aplicações e grupos ou gerir o acesso a estes recursos em nome de outros.

Para obter mais informações, consulte [o que é o portal as minhas aplicações?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Novo separador Resolução de Problemas e Suporte na página Registos de Inícios de Sessão do portal do Azure

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório

A nova **resolução de problemas e suporte** guia a **inícios de sessão** página do Azure portal, tem como objetivo ajudar os administradores e engenheiros de suporte resolver problemas relacionados com inícios de sessão do Azure AD. Este novo separador fornece o código de erro, mensagem de erro e em recomendações de atualização (se houver) para ajudar a resolver o problema. Se não é possível resolver o problema, também fornecemos uma nova forma de criar um pedido de suporte com o **copiar para área de transferência** experiência, que preenche a **ID do pedido** e **data (UTC)** campos para o ficheiro de registo no seu pedido de suporte.  

![Registos de início de sessão com a nova guia](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Suporte avançado para propriedades de extensão personalizadas utilizadas para criar regras de associação dinâmica

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** grupo de gestão  
**Capacidade de produto:** colaboração

Com esta atualização, pode agora clicar o **obter propriedades de extensão personalizado** uma ligação o construtor de regra de grupo dinâmico de utilizador, introduza o seu ID de aplicação exclusivo e receber a lista completa das propriedades de extensão personalizado para utilizar ao criar dinâmico regra de associação para os utilizadores. Esta lista também pode ser atualizada para obter quaisquer novas propriedades de extensão personalizado para essa aplicação.

Para obter mais informações sobre como utilizar propriedades de extensão personalizado para regras de associação dinâmica, consulte [propriedades de extensão e as propriedades de extensão personalizado](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovadas para acesso condicional baseado em aplicações do Azure AD

**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade

As seguintes aplicações estão na lista de [aplicações de cliente aprovadas](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement):

- Microsoft To-Do

- Microsoft Stream

Para obter mais informações, consulte:

- [O Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Novo suporte para Reposição Personalizada de Palavra-passe a partir do ecrã de Bloqueio do Windows 7/8/8.1

**Tipo:** novo recurso  
**Categoria de serviço:** SSPR  
**Capacidade de produto:** autenticação de utilizador

Depois de configurar esta nova funcionalidade, os utilizadores verão uma ligação para repor a palavra-passe do **bloqueio** ecrã de um dispositivo com Windows 7, Windows 8 ou Windows 8.1. Ao clicar nesse link, o utilizador é orientado através do mesmo fluxo de reposição de palavra-passe como, por meio de navegador da web.

Para obter mais informações, consulte [como ativar a reposição do Windows 7, 8 e 8.1 palavra-passe](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Aviso de alteração: Os códigos de autorização já não estarão disponíveis para reutilização 

**Tipo:** plano de alteração  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador

A partir de 15 de Novembro de 2018, do Azure AD irá parar abertos ao recebimento de códigos de autenticação utilizados anteriormente para aplicações. Esta alteração de segurança ajuda a trazer do Azure AD em conformidade com a especificação de OAuth e será imposta em pontos finais de ambos o v1 e v2.

Se a sua aplicação reutiliza os códigos de autorização para obter os tokens de vários recursos, recomendamos que utilize o código para obter um token de atualização e, em seguida, utilizar esse token de atualização para adquirir tokens adicionais para outros recursos. Códigos de autorização só podem ser utilizados uma vez, mas os tokens de atualização podem ser utilizadas várias vezes em vários recursos. Uma aplicação que tente reutilizar um código de autenticação durante o fluxo de código do OAuth obterá um erro de invalid_grant.

Para esta e outras alterações relacionadas com protocolos, consulte [a lista completa de quais são as novidades para autenticação](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Novas Aplicações Federadas disponíveis na galeria de aplicações do Azure AD - setembro de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Em Setembro de 2018, adicionámos que suportam a 16 novos aplicativos com a Federação para a Galeria de aplicações:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Software de recrutamento](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [ JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [floco de neve](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO para o Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial). Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Suporte para métodos de transformações de afirmações adicionais

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO

Introduzimos novos métodos de transformação de afirmação, ToLower() e ToUpper(), que pode ser aplicado em SAML tokens do baseado em SAML **configuração do início de sessão único** página.

Para obter mais informações, consulte [como personalizar afirmações emitidas no token SAML para aplicações empresariais no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>IU de configuração de aplicações baseado em SAML atualizado (pré-visualização)

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO

Como parte da nossa IU de configuração de aplicação atualizado baseado em SAML, que irá obter:

- Uma experiência atualizada passo a passo para configurar as suas aplicações baseadas em SAML.

- Mais visibilidade sobre as novidades em falta ou incorretas na sua configuração.

- A capacidade de adicionar vários endereços de e-mail de notificação de certificado de expiração.

- Novos métodos de transformação de afirmação, ToLower() e ToUpper() e muito mais.

- Uma forma de carregar o seu próprio certificado das suas aplicações empresariais de assinatura de tokens.

- Uma forma de definir o formato NameID para aplicações SAML e uma forma de definir o valor de NameID como extensões de diretório.

Para ativar nessa exibição atualizada, clique no **experimentar a nossa nova experiência** ligação da parte superior do **Single Sign-On** página. Para obter mais informações, consulte [Tutorial: baseado em SAML configurar início de sessão único para uma aplicação com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>Agosto de 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Alterações aos intervalos dos endereços de IP do Azure Active Directory

**Tipo:** plano de alteração  
**Categoria de serviço:** outros  
**Capacidade de produto:** plataforma

Estamos a introduzir maior intervalos IP para o Azure AD, que significa que, se tiver configurado a intervalos de endereços IP do Azure AD para seus firewalls, routers ou grupos de segurança de rede, terá de atualizá-los. Estamos a disponibilizar esta atualização para que não tenha de alterar a firewall, o roteador ou a configurações de intervalo de IP de grupos de segurança de rede novamente quando o Azure AD adiciona novos pontos de extremidade. 

Tráfego de rede está a mudar para estes intervalos de novo nos próximos dois meses. Para continuar com o serviço ininterrupto, tem de adicionar estes valores atualizados para os endereços IP antes de 10 de Setembro de 2018:

- 20.190.128.0/18 

- 40.126.0.0/18 

É altamente recomendável não remover os intervalos de endereço IP antigos até que todo seu tráfego de rede foi movido para os intervalos de novo. Para obter atualizações sobre a mudança e para saber quando é possível remover os intervalos de antigos, consulte [intervalos de endereços IP e URLs do Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Aviso de alteração: Os códigos de autorização já não estarão disponíveis para reutilização 

**Tipo:** plano de alteração  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador

A partir de 15 de Novembro de 2018, do Azure AD irá parar abertos ao recebimento de códigos de autenticação utilizados anteriormente para aplicações. Esta alteração de segurança ajuda a trazer do Azure AD em conformidade com a especificação de OAuth e será imposta em pontos finais de ambos o v1 e v2.

Se a sua aplicação reutiliza os códigos de autorização para obter os tokens de vários recursos, recomendamos que utilize o código para obter um token de atualização e, em seguida, utilizar esse token de atualização para adquirir tokens adicionais para outros recursos. Códigos de autorização só podem ser utilizados uma vez, mas os tokens de atualização podem ser utilizadas várias vezes em vários recursos. Uma aplicação que tente reutilizar um código de autenticação durante o fluxo de código do OAuth obterá um erro de invalid_grant.

Para esta e outras alterações relacionadas com protocolos, consulte [a lista completa de quais são as novidades para autenticação](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Gestão de informações de segurança convergidas para palavras-passe self-service (SSPR) e para a Autenticação Multifator (MFA)

**Tipo:** novo recurso  
**Categoria de serviço:** SSPR  
**Capacidade de produto:** autenticação de utilizador
 
Esse novo recurso ajuda as pessoas a gerir as suas informações de segurança (como número de telefone, aplicação móvel etc.) para SSPR e o MFA numa única localização e experiência; em comparação com a anteriormente, onde ele foi feito em duas localizações diferentes.

Esta experiência convergida também funciona para as pessoas que utilizam o SSPR ou MFA. Além disso, se sua organização não impor o registo MFA ou SSPR, as pessoas ainda podem registar quaisquer métodos de informações de segurança MFA ou SSPR permitidos pela sua organização a partir do portal as minhas aplicações.

Esta é uma participação ativa pré-visualização pública. Os administradores podem ativar a nova experiência (se desejado) para um grupo selecionado ou para todos os utilizadores num inquilino. Para obter mais informações sobre a experiência de convergida, consulte o [convergido experiência blog](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Nova definição de cookies de apenas HTTP nas aplicações proxy da aplicação Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** controlo de acesso

Há uma nova definição chamado, **HTTP-Only Cookies** nas suas aplicações de Proxy de aplicações. Esta definição ajuda a fornecer segurança adicional, incluindo o sinalizador de HTTPOnly no cabeçalho de resposta HTTP para ambos os cookies de sessão de acesso e de Proxy de aplicações, acesso a parar o cookie de um script do lado do cliente e ainda mais a impedir ações como copiar ou modificando o cookie. Embora este sinalizador não tiver sido usado anteriormente, os cookies sempre foram encriptadas e transmitidos através de uma conexão SSL para ajudar a proteger contra modificações impróprias.

Esta definição não é compatível com aplicações que utilizam controles ActiveX, como o ambiente de trabalho remoto. Se estiver nessa situação, recomendamos que Desative esta definição.

Para obter mais informações sobre a definição de HTTP-Only Cookies, consulte [publicar aplicações com o Proxy de aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>O Privileged Identity Management (PIM) para os recursos do Azure suporta tipos de recurso do Grupo de Gestão

**Tipo:** novo recurso  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Definições de ativação e a atribuição de Just-In-Time agora podem ser aplicadas a tipos de recursos do grupo de gestão, tal como já não fizesse para subscrições, grupos de recursos e recursos (como VMs, os serviços de aplicações e muito mais). Além disso, qualquer pessoa com uma função que fornece acesso de administrador para um grupo de gestão pode detetar e gerir esse recurso no PIM.

Para obter mais informações sobre o PIM e recursos do Azure, consulte [detetar e gerir recursos do Azure com o Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>O acesso de aplicação (pré-visualização) fornece um acesso mais rápido ao portal do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Hoje em dia, ao ativar uma função usando o PIM, pode demorar mais de 10 minutos para as permissões para entrar em vigor. Se optar por utilizar o acesso de aplicação, o que está atualmente em pré-visualização pública, os administradores podem aceder portal do Azure AD assim que a solicitação de ativação é concluída.

Atualmente, o acesso de aplicação apenas suporta a experiência do portal do Azure AD e os recursos do Azure. Para obter mais informações sobre o PIM e acesso a aplicações, consulte [o que é o Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Novas Aplicações Federadas disponíveis na galeria de aplicações do Azure AD - Agosto de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Agosto de 2018, adicionámos que suportam a 16 novos aplicativos com a Federação para a Galeria de aplicações:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline desvinculada](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [molho laboratórios - Mobile e o teste da Web](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta redes conector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [como fazemos](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (por Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [dossiê](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - despesas relatórios](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100, Chat ao vivo](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [ Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial). Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>O suporte do Tableau Nativo está agora disponível no proxy de aplicações do Azure AD

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** controlo de acesso

Com a nossa atualização a partir do OpenID Connect para o protocolo de concessão de código do OAuth 2.0 para o nosso protocolo de pré-autenticação, já não tem de efetuar qualquer configuração adicional para utilizar o Tableau com o Proxy de aplicações. Esta alteração de protocolo também ajuda a Proxy de aplicações mais suporte a aplicativos mais modernos utilizando apenas a redirecionamentos HTTP, que normalmente são suportados nas marcas HTML e JavaScript.

Para obter mais informações sobre o nosso suporte nativo para o Tableau, consulte [Proxy de aplicações do Azure AD agora com suporte nativo a Tableau](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Novo suporte para adicionar o Google como fornecedor de identidade para utilizadores convidados B2B no Azure Active Directory (pré-visualização)

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C

Ao configurar a Federação com o Google na sua organização, pode permitir início de sessão de utilizadores convidados Gmail às suas aplicações partilhadas e os recursos através das respetivas contas Google existente, sem ter de criar uma Account Microsoft pessoais (MSAs) ou uma conta do Azure AD.

Esta é uma participação ativa pré-visualização pública. Para obter mais informações sobre o Federação Google, consulte [Google adicionar como um fornecedor de identidade para utilizadores convidados B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Julho de 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Melhorias às notificações de e-mail do Azure Active Directory

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** outros  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade
 
E-mails de Active Directory (Azure AD) do Azure apresentam agora um design atualizado, bem como as alterações para o endereço de e-mail do remetente e o nome de exibição do remetente, quando enviados a partir os seguintes serviços:
 
- Revisões de acesso do Azure AD
- Azure AD Connect Health 
- Azure AD Identity Protection 
- Azure AD Privileged Identity Management
- Notificações de certificado do prestes a expirar de aplicações da empresa
- Notificações serviço de aprovisionamento da aplicação do Enterprise
 
As notificações de e-mail serão enviadas o seguinte endereço de e-mail e nome a apresentar:

- Endereço de e-mail: azure-noreply@microsoft.com
- Nome a apresentar: Microsoft Azure
 
Para obter um exemplo de alguns dos novos designs de email e obter mais informações, consulte [notificações no Azure AD PIM por E-Mail](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Os Registos de Atividade do Azure AD estão agora disponíveis através do Azure Monitor

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório

Os registos de atividades do Azure AD estão agora disponíveis em pré-visualização pública para o Azure Monitor (serviço de monitorização ao nível da plataforma do Azure). Monitor do Azure oferece-lhe retenção de longa duração e a integração totalmente integrada, além destas melhorias:

- Retenção de longo prazo ao encaminhar os ficheiros de registo para a sua própria conta de armazenamento do Azure.

- Integração total dos SIEM, sem que seja necessário escrever ou manter scripts personalizados.

- Integração total com suas próprias soluções personalizadas, ferramentas de análise ou soluções de gestão de incidentes.

Para obter mais informações sobre estas novas capacidades, consulte o nosso blogue [registos de atividades do Azure AD no Azure Monitor, diagnóstico está agora em pré-visualização pública](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) e a nossa documentação, [registos de atividades do Azure Active Directory no Azure Monitor (pré-visualização)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Informações de acesso condicional adicionadas ao relatório de inícios de sessão do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** identidade de segurança e proteção
 
Esta atualização permite-lhe ver que políticas são avaliadas quando um utilizador inicia sessão, juntamente com o resultado de política. Além disso, o relatório inclui agora o tipo de aplicação de cliente utilizado pelo utilizador, para que possa identificar o tráfego de protocolo de legado. Agora também podem ser pesquisadas entradas de relatório para um ID de correlação, que pode ser encontrado na mensagem de erro destinada ao utilizador e pode ser utilizado para identificar e resolver problemas relacionados com o pedido de início de sessão correspondente.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Ver autenticações legadas através dos registos de atividade de Inícios de sessão

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório
 
Com a introdução do **aplicação de cliente** registos de campo da atividade de início de sessão, os clientes podem agora ver os utilizadores que estão a utilizar as autenticações de legado. Os clientes poderão aceder a estas informações com a Graph API do inícios de sessão MS ou através do início de sessão registos de atividades no portal do Azure AD em que pode utilizar o **aplicação de cliente** controlo para filtrar em autenticações herdadas. Consulte a documentação para obter mais detalhes.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Novas Aplicações Federadas disponíveis na galeria de aplicações do Azure AD - Julho de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Em Julho de 2018, adicionámos que suportam a 16 novos aplicativos com a Federação para a Galeria de aplicações:

[Hub de inovação](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [determinados administrador SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), teste PSUC, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Matic Screencast-S](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial) , PowerSchool sala de aula, a unificação [Eli inclusão](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [suporte remoto Bomgar](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial) , [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow conector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [Base de habilidades](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial). Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Novas integrações de aplicações SaaS para aprovisionamento de utilizadores - Julho de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3º integração de terceiros
 
O Azure AD permite-lhe automatizar a criação, a manutenção e a remoção de identidades de utilizador em aplicações de SaaS, como o Dropbox, Salesforce, ServiceNow e muito mais. Julho de 2018, adicionámos suporte para as seguintes aplicações na Galeria de aplicações do Azure AD de provisionamento de usuários:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Para obter uma lista de todas as aplicações que suportam o aprovisionamento de utilizadores na galeria do Azure AD, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health para Sincronização - Uma forma mais fácil de corrigir os erros de sincronização de atributos duplicados e órfãos

**Tipo:** novo recurso  
**Categoria de serviço:** AD Connect  
**Capacidade de produto:** monitoramento e relatório
 
O Azure AD Connect Health apresenta a solução de gestão personalizada para o ajudar a realçar e corrigir erros de sincronização. Esta funcionalidade soluciona problemas em erros de sincronização de atributo duplicado e correções de objetos que ficarão órfãos do Azure AD. Este diagnóstico tem as seguintes vantagens:

- Restringe os erros de sincronização de atributo duplicado, fornecer correções específicas

- Aplica-se uma correção dedicam a cenários do Azure AD, resolução de erros numa única etapa

- Nenhuma atualização ou a configuração é necessária para ativar e utilizar esta funcionalidade

Para obter mais informações, consulte [diagnosticar e resolver erros de sincronização de atributo duplicado](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Atualizações visuais às experiências de início de sessão do Azure AD e MSA

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** autenticação de utilizador

Atualizámos a interface do Usuário para a experiência da Microsoft online services início de sessão, por exemplo, para o Office 365 e Azure. Esta alteração torna os ecrãs mais simples e menos confuso. Para obter mais informações sobre esta alteração, consulte a [futuras melhorias na experiência de início de sessão do Azure AD](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) blog.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Nova versão do Azure AD Connect - Julho de 2018

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade

A versão mais recente do Azure AD Connect inclui: 

- Correções de erros e atualizações da capacidade de suporte 

- Disponibilidade geral da integração federar de Ping

- Atualizações para o cliente mais recente do SQL 2012 

Para obter mais informações sobre esta atualização, consulte [do Azure AD Connect: histórico de versões](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Atualizações à IU de utilizador final dos Termos de Utilização (TdU)

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação

Estamos a atualizar a cadeia de caracteres de aceitação na IU do utilizador final de termos de utilização.

**Texto atual.** Para poder aceder aos recursos de [tenantName], tem de aceitar os termos de utilização.<br>**Novo texto.** Para poder aceder ao recurso de [tenantName], deve ler os termos de utilização.

**Texto atual:** optando por aceitar significa que concorda com todos os termos de utilização acima.<br>**O texto:** clique em aceitar para confirmar que tenha lido e compreendido os termos de utilização.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>A autenticação pass-through suporta agora aplicações e protocolos de rede legados

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Agora a autenticação pass-through suporta protocolos legados e aplicações. As seguintes limitações agora são totalmente suportadas:

- Utilizador inícios de sessão para o Office cliente aplicativos herdados, Office 2010 e o Office 2013, sem a necessidade de autenticação moderna.

- Acesso a partilha de calendário e informações de disponibilidade no Exchange ambientes híbridos no Office 2010 apenas.

- Utilizador inícios de sessão ao Skype para aplicativos de cliente de negócio sem a necessidade de autenticação moderna.

- Utilizador inícios de sessão para o PowerShell versão 1.0.

- A Apple registo de aparelho (DEP) da Apple, utilizando o Assistente de configuração de iOS. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Gestão de informações de segurança convergidas para reposição de palavra-passe self-service e Autenticação Multifator

**Tipo:** novo recurso  
**Categoria de serviço:** SSPR  
**Capacidade de produto:** autenticação de utilizador

Esta nova funcionalidade permite aos utilizadores gerir as suas informações de segurança (por exemplo, número de telefone, endereço de e-mail, aplicações móveis e assim por diante) para a reposição de palavra-passe self-service (SSPR) e o multi-factor Authentication (MFA) numa única experiência. Os utilizadores já não terá de registar as informações de segurança mesmo para SSPR e o MFA no duas experiências diferentes. Esta nova experiência também se aplica aos utilizadores que têm o SSPR ou MFA.

Se uma organização não é impor o registo MFA ou SSPR, os utilizadores podem registar as suas informações de segurança através da **as minhas aplicações** portal. A partir daí, os utilizadores podem registar quaisquer métodos ativados para MFA ou SSPR. 

Esta é uma participação ativa pré-visualização pública. Os administradores podem ativar a nova experiência (se desejado) para um grupo selecionado de utilizadores ou de todos os utilizadores num inquilino.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Utilize a aplicação Microsoft Authenticator para verificar a sua identidade ao repor a palavra-passe

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** SSPR  
**Capacidade de produto:** autenticação de utilizador

Esta funcionalidade permite que os não-administradores verificar a respetiva identidade ao repor uma palavra-passe através de uma notificação ou o código a partir do Microsoft Authenticator (ou qualquer outra aplicação de autenticador). Depois de ativassem os administradores de método de repor esta palavra-passe self-service, os utilizadores que registou uma aplicação móvel por meio de aka.ms/mfasetup ou aka.ms/setupsecurityinfo podem utilizar a aplicação móvel como método de verificação ao repor a palavra-passe.

Notificação de aplicação móvel apenas pode ser ligada como parte de uma política que requer dois métodos para repor a palavra-passe.

---

## <a name="june-2018"></a>junho de 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Alterar o aviso: a correção de segurança para o fluxo de autorização delegada para aplicações que utilizam a API de registos de atividade do Azure AD

**Tipo:** plano de alteração  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório

Devido à nossa imposição de segurança mais forte, tivemos de fazer uma alteração às permissões para aplicações que utilizam um fluxo de autorização delegada para acessar [APIs de registos de atividade do Azure AD](https://aka.ms/aadreportsapi). Esta alteração irá ocorrer por **26 de Junho de 2018**.

Se qualquer uma das suas aplicações a utilizar APIs de Log de atividade do Azure AD, siga estes passos para garantir que o aplicativo não é corrompido após a alteração ocorre.

**Para atualizar as suas permissões de aplicação**

1. Inicie sessão no portal do Azure, selecione **do Azure Active Directory**e, em seguida, selecione **registos das aplicações**.
2. Selecione a sua aplicação que utiliza a API do Azure AD atividade registos, selecione **configurações**, selecione **permissões obrigatórias**e, em seguida, selecione o **Windows Azure Active Directory** API.
3. Na **permissões delegadas** área da **ativar o acesso** painel, selecione a caixa junto a **diretórios leia** dados e, em seguida, selecione **guardar**.
4. Selecione **conceder permissões**e, em seguida, selecione **Sim**.
    
    >[!Note]
    >Tem de ser um Administrador Global para conceder permissões à aplicação.

Para obter mais informações, consulte a [conceder permissões](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) área dos pré-requisitos para acessar o artigo do API de geração de relatórios do Azure AD.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Configurar as definições de TLS para ligar aos serviços do Azure AD para conformidade PCI DSS

**Tipo:** novo recurso  
**Categoria de serviço:** n/d  
**Capacidade de produto:** plataforma

Transport Layer Security (TLS) é um protocolo que fornece privacidade e integridade dos dados entre duas aplicações de comunicação e é o protocolo de segurança implementadas mais amplamente usado hoje em dia.

O [PCI Security Standards Council](https://www.pcisecuritystandards.org/) determinou que as versões mais antigas do TLS e do Secure Sockets Layer (SSL) tem de ser desativadas em favor de habilitar os protocolos de aplicação nova e mais seguro, com a partir de conformidade **30 de Junho, 2018**. Esta alteração significa que, se ligar aos serviços do Azure AD e exigir conformidade do PCI DSS, tem de desativar TLS 1.0. Existem várias versões do TLS, mas o TLS 1.2 é a versão mais recente disponível para os serviços do Active Directory do Azure. É altamente recomendável mudar diretamente para o TLS 1.2 para combinações de cliente/servidor e o browser/servidor.

Browsers desatualizados podem não suportar versões mais recentes do TLS, por exemplo, o TLS 1.2. Para ver quais as versões do TLS são suportadas pelo seu browser, vá para o [laboratórios de SSL de Qualys](https://www.ssllabs.com/) do site e clique em **testar seu navegador**. Recomendamos que Atualize para a versão mais recente do seu navegador da web e, de preferência ative apenas TLS 1.2.

**Para ativar o TLS 1.2, por browser**

- **Microsoft Edge e o Internet Explorer (ambos são definidos usando o Internet Explorer)**

    1. Abra o Internet Explorer, selecione **ferramentas** > **opções da Internet** > **avançadas**.
    2. Na **Security** área, selecione **utilizar TLS 1.2**e, em seguida, selecione **OK**.
    3. Feche todas as janelas do browser e reinicie o Internet Explorer. 

- **Google Chrome**

    1. Abra Google Chrome, tipo *chrome://settings/* na barra de endereço e prima **Enter**.
    2. Expanda a **avançadas** opções, vá para o **System** área e selecione **abrir as definições de proxy**.
    3. No **propriedades de Internet** caixa, selecione a **avançadas** separador, vá para o **segurança** área, selecione **utilizar TLS 1.2**e, em seguida, selecione  **OK**.
    4. Feche todas as janelas do browser e reinicie o Google Chrome.

- **Mozilla Firefox**

    1. Firefox aberto, tipo *sobre: config* na barra de endereço e, em seguida, prima **Enter**.
    2. Procure o termo *TLS*e, em seguida, selecione a **security.tls.version.max** entrada.
    3. Defina o valor como **3** para forçar o browser para utilizar a cópia de segurança para a versão do TLS 1.2 e, em seguida, selecione **OK**.

        >[!NOTE]
        >Firefox versão 60.0 oferece suporte a TLS 1.3, portanto, também pode definir o valor de security.tls.version.max **4**.

    4. Feche todas as janelas do browser e reinicie o Mozilla Firefox.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD - Junho de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Junho de 2018, adicionámos que 15 novos aplicativos com a Federação de suporte para a Galeria de aplicações:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [liquidação de música](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 Token de aplicação LOB ativada](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ Cópia de segurança de ponto final](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh redes](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho um](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [ SharePoint no local](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [previr CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial). Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>A proteção de palavra-passe do Azure AD está disponível em pré-visualização pública

**Tipo:** novo recurso  
**Categoria de serviço:** Identity Protection  
**Capacidade de produto:** autenticação de utilizador

Utilize a proteção de palavra-passe do Azure AD para ajudar a eliminar facilmente adivinhado palavras-passe do seu ambiente. Eliminar estas palavras-passe ajuda a reduzir o risco de comprometimento de um tipo de spray de palavra-passe de ataque.

Especificamente, a proteção de palavra-passe do Azure AD ajuda-o:

- Proteger contas da sua organização do Azure AD e Windows Server Active Directory (AD). 
- Deixa os usuários usem senhas numa lista de mais de 500 das palavras-passe utilizadas com mais frequência e variações de substituição de caracteres de mais de 1 milhão dessas palavras-passe.
- Administrar a proteção de palavra-passe do Azure AD a partir de uma única localização no portal do Azure AD, para ambas do Azure AD e AD do Windows Server no local.

Para obter mais informações sobre a proteção de palavra-passe do Azure AD, consulte [eliminar palavras-passe incorretas na sua organização](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Novo "todos os convidados" acesso condicional modelo de política criado durante a criação de termos de utilização (ToU)

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação

Durante a criação de seus termos de utilização (ToU), é também criado um novo modelo de política de acesso condicional para "todos os convidados" e "todas as aplicações". Este novo modelo de política aplica-se os termos de utilização recém-criado, simplificando a criação e o processo de imposição para convidados.

Para obter mais informações, consulte [do Azure Active Directory termos da funcionalidade de utilização](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Novo modelo de política de acesso condicional "personalizado" criado durante a criação de termos de utilização (ToU)

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação

Durante a criação de seus termos de utilização (ToU), também é criado um novo modelo de política de acesso condicional "personalizado". Este novo modelo de política permite-lhe criar os termos de utilização e, em seguida, imediatamente aceda ao painel de criação da política de acesso condicional, sem precisar navegar manualmente através do portal.

Para obter mais informações, consulte [do Azure Active Directory termos da funcionalidade de utilização](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Novo e abrangente orientações sobre como implementar o Azure multi-factor Authentication

**Tipo:** novo recurso  
**Categoria de serviço:** outros  
**Capacidade de produto:** identidade de segurança e proteção
 
Lançámos o novo orientações passo a passo sobre como implementar o Azure multi-factor Authentication (MFA) na sua organização.

Para ver o guia de implementação do MFA, vá para o [guias de implantação de identidade](https://aka.ms/DeploymentPlans) repositório no GitHub. Para fornecer comentários sobre os guias de implementação, utilize o [formulário de comentários de plano de implementação](https://aka.ms/deploymentplanfeedback). Se tiver quaisquer perguntas sobre os guias de implementação, contacte-nos [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Gestão de aplicações, funções estão em pré-visualização pública de delegado do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** controlo de acesso

Os administradores agora podem delegar tarefas de gestão de aplicações sem atribuir a função de Administrador Global. As novas funções e funcionalidades são:

- **Novo padrão de funções de administrador do Azure AD:**

    - **Administrador da aplicação.** Concede a capacidade de gerir todos os aspetos de todas as aplicações, incluindo o registro, as definições de SSO, atribuições de aplicações e licenciamento, definições de proxy de aplicações e consentimento (exceto para recursos do Azure AD).

    - **Administrador da aplicação de cloud.** Concede todas as capacidades de administrador da aplicação, com exceção do proxy de aplicação pois não fornece acesso no local.

    - **Desenvolvedor de aplicativos.** Concede a capacidade de criar registos de aplicações, mesmo que o **permitir que os utilizadores registem aplicações** opção está desativada.

- **Propriedade (configurar por aplicação por empresarial e de registo de aplicação, semelhante ao processo de propriedade de grupo:**
 
    - **Proprietário do registo de aplicação.** Concede a capacidade de gerenciar todos os aspectos do registo de aplicações pertencentes à empresa, incluindo o manifesto da aplicação e adicionar proprietários adicionais.

    - **Proprietário da aplicação empresarial.** Concede a capacidade de gerir muitos aspectos de aplicações do enterprise pertencentes à empresa, incluindo as definições de SSO, atribuições de aplicações e consentimento (exceto para recursos do Azure AD).

Para obter mais informações sobre a pré-visualização pública, consulte o [do Azure AD delegado funções estão em pré-visualização pública de gestão de aplicações!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) Blog. Para obter mais informações sobre as funções e permissões, consulte [atribuir funções de administrador no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Maio de 2018

### <a name="expressroute-support-changes"></a>Alterações de suporte do ExpressRoute

**Tipo:** plano de alteração  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** plataforma  

Software como um oferta de serviço, como o Azure Active Directory (Azure AD) foram concebidas para funcionar melhor indo diretamente através da Internet, sem exigir o ExpressRoute ou quaisquer outros privadas túneis VPN. Por este motivo, no **dia 1 de Agosto de 2018**, vamos deixar de suportar o ExpressRoute para serviços do Azure AD com o peering público do Azure e comunidades do Azure em peering da Microsoft. Quaisquer serviços afetados por esta alteração poderão notar Azure tráfego AD gradualmente mudando de ExpressRoute à Internet.

Enquanto estamos a alterar a nosso suporte, podemos também sabe disso são ainda situações em que poderá ter de utilizar um conjunto dedicado de circuitos para o tráfego de autenticação. Por este motivo, do Azure AD irá continuar suportar restrições de intervalo IP por inquilino utilizar o ExpressRoute e de serviços já no peering da Microsoft com a Comunidade de "Outros serviços Online do Office 365". Se os serviços são afetados, mas exigir o ExpressRoute, terá de efetuar o seguinte:

- **Se estiver em peering público do Azure.** Mover para o peering da Microsoft e inscrever-se a **outras Online do Office 365 services (12076:5100)** Comunidade. Para obter mais informações sobre como mover de peering público do Azure para o peering da Microsoft, consulte a [mover um peering público para o peering da Microsoft](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) artigo.

- **Se estiver no peering da Microsoft.** Inscreva-se a **serviço outras Online do Office 365 (12076:5100)** Comunidade. Para obter mais informações sobre os requisitos de encaminhamento, consulte a [suporte para a secção de Comunidades BGP](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) do artigo de requisitos encaminhamento do ExpressRoute.

Se deve continuar a utilizar circuitos dedicados, terá de comunicar com a equipa da Microsoft Account sobre como obter autorização para utilizar o **serviço outras Online do Office 365 (12076:5100)** Comunidade. A MS geridos pelo Office banca examinadora irá verificar se tem os circuitos e certifique-se de que compreende as implicações técnicas de mantê-los. As assinaturas não autorizadas tentar criar filtros de rota para o Office 365 irão receber uma mensagem de erro. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph APIs para cenários administrativos para termos de utilização

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** experiência do desenvolvedor
 
Adicionamos o Microsoft Graph APIs para a operação de administração do Azure AD termos de utilização. É possível criar, atualizar e eliminar o objeto de termos de utilização.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Adicionar ponto final de multi-inquilino do Azure AD como fornecedor de identidade no Azure AD B2C

**Tipo:** novo recurso  
**Categoria de serviço:** B2C - gestão de identidades de consumidor  
**Capacidade de produto:** B2B/B2C
 
Utilizar políticas personalizadas, agora pode adicionar o ponto de extremidade comum do Azure AD como fornecedor de identidade no Azure AD B2C. Isto permite-lhe ter um único ponto de entrada para todos os utilizadores do Azure AD que está a iniciar sessão nas suas aplicações. Para obter mais informações, consulte [do Azure Active Directory B2C: permitir que os utilizadores iniciem sessão a um fornecedor de identidade do multi-inquilino do Azure AD com as políticas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Utilizar URLs interno para aceder às aplicações a partir de qualquer lugar com a nossa extensão My Apps início de sessão e o Proxy de aplicações do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** SSO
 
Os utilizadores podem agora aceder às aplicações através de URLs internos, mesmo quando fora da rede empresarial através da utilização segura de aplicações meu início de sessão extensão para o Azure AD. Isso funcionará com qualquer aplicação que publicou com o Proxy de aplicações do Azure AD, em qualquer browser que também tenha a extensão de browser do painel de acesso instalada. A funcionalidade de redirecionamento de URL é ativada automaticamente assim que um utilizador iniciar sessão a extensão. A extensão está disponível para download no [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367), e [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>O Azure Active Directory - dados na Europa para os clientes da Europa

**Tipo:** novo recurso  
**Categoria de serviço:** outros  
**Capacidade de produto:** GoLocal

Os clientes na Europa necessitam de seus dados para se manter na Europa e não são replicados fora europeus centros de dados para a privacidade de reunião e da legislação europeia. Isso [artigo](https://go.microsoft.com/fwlink/?linkid=872328) fornece detalhes específicos nos quais informações de identidade serão armazenados dentro da Europa e também fornecem detalhes sobre informações que serão armazenados fora centros de dados europeus. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Novo utilizador aprovisionamento integrações de aplicação SaaS - Maio de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3º integração de terceiros
 
O Azure AD permite-lhe automatizar a criação, a manutenção e a remoção de identidades de utilizador em aplicações de SaaS, como o Dropbox, Salesforce, ServiceNow e muito mais. Maio de 2018, adicionámos suporte para as seguintes aplicações na Galeria de aplicações do Azure AD de provisionamento de usuários:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Alicerce OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Para obter uma lista de todas as aplicações que suportam o aprovisionamento de utilizadores na galeria do Azure AD, consulte [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Revisões de acesso do Azure AD de grupos e acesso de aplicação fornece agora revisões periódicas

**Tipo:** novo recurso  
**Categoria de serviço:** revisões de acesso  
**Capacidade de produto:** governação
 
Revisão de acesso de grupos e aplicações está agora geralmente disponível como parte do Azure AD Premium P2.  Os administradores poderão configurar as revisões de acesso de atribuições de aplicações e associações de grupo para repetir automaticamente em intervalos regulares, por exemplo, mensal ou trimestral.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Registos de atividade do AD do Azure (inícios de sessão e auditoria) estão agora disponíveis através do MS Graph

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório
 
Registos de atividade do AD do Azure, nomeadamente, inícios de sessão e registos de auditoria, estão agora disponíveis através do MS Graph. Podemos ter expostos dois pontos finais por meio do MS Graph para aceder a estes registos. Confira nosso [documentos](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) para acesso programático a APIs do Azure AD de relatórios para começar a utilizar. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Experiência de melhorias para o resgate de B2B e deixam uma organização

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C

**Just-in resgate de tempo:** depois de partilhar um recurso com um utilizador convidado com a API de B2B e não precisa de enviar um e-mail de convite especial. Na maioria dos casos, o utilizador convidado pode aceder ao recurso e será direcionado através da experiência de resgate just-in-time. Não existem mais impacto devido a mensagens de e-mail em falta. Não existem mais solicite aos utilizadores convidados "Fez clicar no link resgate que enviou-o o sistema?". Isso significa que uma vez SPO utiliza o Gestor de convite – anexos nublados podem ter o mesmo URL canônico para todos os utilizadores – internos e externos – em qualquer Estado de resgate.

**Experiência de resgate moderna:** não mais dividir resgate de tela, página de destino. Os utilizadores verão um moderno consentir a experiência com a declaração de privacidade da organização de convite, tal como fazem para aplicações de terceiros.

**Os utilizadores convidados podem deixar a organização:** assim que a relação de um utilizador com uma organização terminar, podem personalizada abandonar a organização. Não existem mais chamando o administrador da organização convite "ser removidas", não existem mais aumentar pedidos de suporte.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD - Maio de 2018

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros
 
Em Maio de 2018, adicionámos que suportam a essas 18 novas aplicações com a Federação para nossa Galeria de aplicações:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty regular, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate primos](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [arco publicação - SSO ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Novos guias de implementação passo a passo para o Azure Active Directory

**Tipo:** novo recurso  
**Categoria de serviço:** outros  
**Capacidade de produto:** diretório
 
Orientações de novo, passo a passo sobre como implementar o Azure Active Directory (Azure AD), incluindo reposição de palavra-passe self-service (SSPR), single sign-on (SSO), acesso condicional (AC), proxy de aplicações, aprovisionamento de utilizadores, Active Directory Federation Services (ADFS) para Autenticação pass-through (PTA) e o ADFS para sincronização de hash de palavra-passe (PHS).

Para ver os guias de implementação, vá para o [guias de implantação de identidade](https://aka.ms/DeploymentPlans) repositório no GitHub. Para fornecer comentários sobre os guias de implementação, utilize o [formulário de comentários de plano de implementação](https://aka.ms/deploymentplanfeedback). Se tiver quaisquer perguntas sobre os guias de implementação, contacte-nos [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Aplicações empresariais de pesquisa – carregar mais aplicações

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO
 
Encontrar as suas aplicações / principais de serviço? Adicionámos a capacidade de carregar mais aplicações nas suas aplicações empresariais todas as listas de aplicações. Por predefinição, vamos mostrar 20 aplicações. Pode agora clicar, **carregar mais** para ver as aplicações adicionais. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>Maio lançamento do AADConnect contém uma pré-visualização pública da integração com o PingFederate, atualizações de segurança importante, muitas correções de erros e ótimo novo novo solucionar problemas de ferramentas. 

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** AD Connect  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade
 
Maio lançamento do AADConnect contém uma pré-visualização pública da integração com o PingFederate, atualizações de segurança importante, muitas correções de erros e ótimo novo novo solucionar problemas de ferramentas. Pode encontrar as notas de versão [aqui](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Revisões de acesso do Azure AD: automática

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** revisões de acesso  
**Capacidade de produto:** governação

Revisões de acesso de grupos e aplicações estão agora geralmente disponíveis como parte do Azure AD Premium P2. Um administrador pode configurar para aplicar automaticamente as alterações do revisor a esse grupo ou aplicação que a revisão de acesso for concluída. O administrador também pode especificar o que acontece com acesso contínuo do utilizador se os revisores não responder, remover o acesso, manter o acesso ou efetuar recomendações de sistema. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>Tokens de ID já não podem ser devolvidos com o response_mode de consulta para novas aplicações. 

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Aplicações criadas em ou depois de 25 de Abril de 2018 já não poderá solicitar uma **id_token** utilizando o **consulta** response_mode.  Isso traz inline do Azure AD com as especificações de OIDC e ajuda a reduzir a superfície de ataque de aplicações.  Aplicações criadas antes de 25 de Abril de 2018 não estão impedidas de utilizar o **consulta** response_mode com um response_type de **id_token**.  O erro devolvido, ao pedir uma id_token do AAD, foi **AADSTS70007: "query" não é um valor suportado de "response_mode" ao pedir um token**.

O **fragmento** e **form_post** response_modes continuar a trabalhar - quando criar novos objetos de aplicação (por exemplo, para utilização de Proxy de aplicação), certifique-se de uso de uma destas response_modes antes de ser criar um nova aplicação.  

---