---
title: "Listar a aplicação na Galeria de aplicações do Azure Active Directory"
description: "Como listar uma aplicação que suporta o início de sessão único na galeria do Azure Active Directory | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/09/2018
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: feb09aa8f8e22ad6fbda6a490d251c500bedf3ee
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Listar a aplicação na Galeria de aplicações do Azure Active Directory


##  <a name="what-is-azure-ad-app-gallery"></a>O que é Galeria de aplicações do Azure AD?

Azure AD é um serviço de identidade baseado na nuvem. [Galeria de aplicações do Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) é um arquivo comum, onde todos os conectores de aplicações são publicados para o início de sessão único e o aprovisionamento de utilizadores. Os nossos mútua clientes que estão a utilizar o Azure AD como fornecedor de identidade procure conectores de aplicações SaaS diferentes, que são publicados aqui. Administrador de TI adiciona o conector da Galeria de aplicações e configura e utilizá-lo para o início de sessão único e o aprovisionamento. AD do Azure suporta todos os protocolos de Federação principal como SAML 2.0, OpenID Connect, OAuth e WS-Fed para o início de sessão único. 

## <a name="what-are-the-benefits-of-listing-the-application-in-the-gallery"></a>Quais são as vantagens de listagem aplicação na galeria do?

*  Fornece melhor único início de sessão experiência possível aos clientes.

*  Configuração mínima e Simple da aplicação.

*  Os clientes podem procurar a aplicação e encontrá-lo na galeria. 

*  Qualquer cliente pode utilizar esta integração independentemente do Azure AD SKU gratuito, Basic ou Premium.

*  Pelo tutorial de configuração passo a passo para os clientes mútua.

*  Ative o aprovisionamento de utilizador para a mesma aplicação se estiver a utilizar o SCIM.


##  <a name="what-are-the-pre-requisites"></a>Quais são os pré-requisitos?

Para listar uma aplicação na galeria do Azure AD, a aplicação primeiro tem de implementar um dos protocolos Federação suportados pelo Azure AD. Leia os termos e condições da Galeria de aplicações do Azure AD a partir daqui. Se estiver a utilizar: 

*   **OpenID Connect** - criar a aplicação multi-inquilino no Azure AD e implementar [framework de consentimento do Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) para a sua aplicação. Envie o pedido de início de sessão para o ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador do cliente com base no ID de inquilino e de UPN do utilizador foi recebido no token. Para integrar a aplicação com o Azure AD, pode seguir o [instruções de programador](active-directory-authentication-scenarios.md).

*   **SAML 2.0 ou WS-Fed** – a aplicação deve ter uma capacidade de efetuar a integração de SAML/WS-Fed SSO no modo SP ou IDP. Qualquer aplicação que suporta SAML 2.0, pode ser integrado diretamente com um inquilino do Azure AD utilizando o [instruções para adicionar uma aplicação personalizada](../active-directory-saas-custom-apps.md).

*   **Palavra-passe SSO** – criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [baseada em palavra-passe de início de sessão](../active-directory-appssoaccess-whatis.md). Baseado em palavra-passe SSO, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que várias os utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais. 

## <a name="process-for-submitting-the-request-in-the-portal"></a>Processo para submeter o pedido no portal

Assim que tiver testado que a integração de aplicações funciona com o Azure AD, terá de enviar o pedido de acesso no nosso [aplicação rede Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se tiver uma conta do Office 365, pode utilizar que para iniciar sessão neste portal, caso contrário, utilize o seu ID da Microsoft (Live ID, Outlook, Hotmail etc.) para início de sessão. Consulte a seguinte página para solicitar o acesso. Forneça uma justificação de negócio na caixa de texto e clique em **pedir acesso**. A nossa equipa irá rever todos os detalhes e dão-lhe o acesso em conformidade. Depois disso, pode iniciar sessão no portal e submeter o pedido de detalhado para a aplicação.

Se enfrentam qualquer problema de acesso, contacte [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Pedido de acesso no SharePoint portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Linhas cronológicas
    
*   Processo de listagem SAML 2.0 ou a aplicação de WS-Fed para a galeria - **7 a 10 dias de negócio**

   ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/timeline.png)

*   Processo de listagem OpenID Connect aplicação na galeria - **2 a 5 dias úteis**

   ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalamentos de clientes

Para qualquer Escalamentos de clientes, remova um e-mail para [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e vamos voltar para o utilizador ASAP.

