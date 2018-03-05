---
title: "Listar a aplicação na Galeria de aplicações do Azure Active Directory | Microsoft Docs"
description: "Como listar uma aplicação que suporta o início de sessão único na Galeria de aplicações do Azure Active Directory"
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
ms.openlocfilehash: 502fb555bd3b381c9be0ff04e210cc07f9bf6cd8
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Listar a aplicação na galeria de aplicações do Azure Active Directory


##  <a name="what-is-the-azure-ad-application-gallery"></a>O que é a Galeria de aplicações do Azure AD?

Azure Active Directory (Azure AD) é um serviço de identidade baseada na nuvem. O [Galeria de aplicações do Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) na loja de aplicações do Azure Marketplace, onde todos os conectores de aplicações são publicados para o início de sessão único e o aprovisionamento de utilizadores. Os clientes que utilizam o Azure AD como um fornecedor de identidade localizar os conectores de aplicações SaaS diferentes publicados aqui. Os administradores de TI adicionar conectores a partir da Galeria de aplicações e, em seguida, configurar e utilizarem os conectores para o início de sessão único e o aprovisionamento. AD do Azure suporta todos os protocolos de Federação principal para início de sessão, incluindo SAML 2.0, OpenID Connect, OAuth e WS-Fed. 

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Quais são as vantagens de uma aplicação na Galeria de listagem?

*  Os clientes localizar possíveis único início de sessão uma experiência otimizada.

*  A configuração da aplicação é simples e mínimo. 

*  Uma procura rápida localiza a aplicação na galeria.

*  Libertar, básico, e todos os clientes de Premium do Azure AD podem utilizar esta integração. 

*  Os clientes mútua obter um tutorial passo a passo de configuração. 

*  Os clientes que utilizam o SCIM podem utilizar o aprovisionamento para a mesma aplicação.


##  <a name="prerequisites-implement-federation-protocol"></a>Pré-requisitos: Protocolo de Federação de implementar

Para listar uma aplicação na Galeria de aplicações do Azure AD, primeiro terá de implementar um dos seguintes protocolos de Federação suportados pelo Azure AD. Leia os termos e condições da Galeria de aplicações do Azure AD a partir daqui. 

*   **OpenID Connect**: criar a aplicação multi-inquilino no Azure AD e implementar o [framework de consentimento do Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) para a sua aplicação. Envie o pedido de início de sessão para um ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador com base no ID de inquilino e de UPN do utilizador foi recebido no token. Para integrar a aplicação com o Azure AD, siga o [instruções de programadores](active-directory-authentication-scenarios.md).

*   **SAML 2.0** ou **WS-Fed**: A aplicação tem de ter a capacidade para efetuar a integração de SAML/WS-Fed SSO no modo SP ou IDP. Se a sua aplicação suporta SAML 2.0, pode integrá-lo diretamente com um inquilino do Azure AD utilizando o [instruções para adicionar uma aplicação personalizada](../active-directory-saas-custom-apps.md).

*   **Palavra-passe SSO**: criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [baseada em palavra-passe de início de sessão](../active-directory-appssoaccess-whatis.md). Baseado em palavra-passe SSO, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que vários utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais. 

## <a name="submit-the-request-in-the-portal"></a>Submeter o pedido no portal

Depois de ter testado que a integração de aplicações funciona com o Azure AD, submeta o pedido de acesso no nosso [aplicação rede Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se tiver uma conta do Office 365, utilize-o para iniciar sessão neste portal. Caso contrário, utilizar a sua conta Microsoft (tais como o Outlook ou Hotmail) para iniciar sessão.

Depois de iniciar sessão, é apresentada a página seguinte. Forneça uma justificação de negócio para que necessita de acesso na caixa de texto e, em seguida, selecione **pedir acesso**. A nossa equipa revê os detalhes e dá-lhe acesso em conformidade. Depois disso, pode iniciar sessão no portal e submeter o pedido de detalhado para a aplicação.

Se tiver quaisquer problemas relativos ao acesso, contacte o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Pedido de acesso no SharePoint portal](./media/active-directory-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Linhas cronológicas
    
A linha cronológica para o processo de listagem um SAML 2.0 ou a aplicação de WS-Fed na galeria é dias úteis de 7 a 10.

   ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/timeline.png)

A linha cronológica para o processo de uma aplicação OpenID Connect na Galeria de listagem é 2 a 5 dias úteis.

   ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Escalamentos de clientes

Para qualquer Escalamentos de clientes, enviar correio eletrónico para o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e irá respondermos logo que possível.

