---
title: Listar a aplicação na Galeria de aplicações do Azure Active Directory | Microsoft Docs
description: Saiba como listar uma aplicação que suporta o início de sessão único na Galeria de aplicações do Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/14/2018
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.openlocfilehash: b369cdeb3a58db0336d4f6e343599aaa46643b32
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316789"
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

Para listar uma aplicação na Galeria de aplicações do Azure AD, primeiro terá de implementar um dos seguintes protocolos de Federação suportados pelo Azure AD e concordo com os termos de Galeria de aplicações do Azure AD e condições. Leia os termos e condições da Galeria de aplicações do Azure AD de [aqui](https://azure.microsoft.com/en-us/support/legal/active-directory-app-gallery-terms/).

*   **OpenID Connect**: criar a aplicação multi-inquilino no Azure AD e implementar o [framework de consentimento do Azure AD](active-directory-integrating-applications.md#overview-of-the-consent-framework) para a sua aplicação. Envie o pedido de início de sessão para um ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador com base no ID de inquilino e de UPN do utilizador foi recebido no token. Para integrar a aplicação com o Azure AD, siga o [instruções de programadores](active-directory-authentication-scenarios.md).

    ![Linha cronológica de listagem aplicação OpenID Connect para a Galeria](./media/active-directory-app-gallery-listing/openid.png)

    * Se pretender adicionar a sua aplicação para lista na galeria com OpenID Connect, selecione **OpenID Connect e OAuth 2.0** como acima.

    * Se tiver quaisquer problemas relativos ao acesso, contacte o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** ou **WS-Fed**: A aplicação tem de ter a capacidade para efetuar a integração de SAML/WS-Fed SSO no modo SP ou IDP. Se a sua aplicação suporta SAML 2.0, pode integrá-lo diretamente com um inquilino do Azure AD utilizando o [instruções para adicionar uma aplicação personalizada](../active-directory-saas-custom-apps.md).

    ![Linha cronológica de listagem SAML 2.0 ou WS-Fed a aplicação na Galeria](./media/active-directory-app-gallery-listing/saml.png)

    * Se pretende adicionar a aplicação à lista na galeria com **SAML 2.0** ou **WS-Fed**, selecione **SAMl 2.0/WS-Fed** como acima.

    * Se tiver quaisquer problemas relativos ao acesso, contacte o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **Palavra-passe SSO**: criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [baseada em palavra-passe de início de sessão](../manage-apps/what-is-single-sign-on.md). Baseado em palavra-passe SSO, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que vários utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais.

    ![Linha cronológica de listagem aplicação de SSO de palavra-passe para a Galeria](./media/active-directory-app-gallery-listing/passwordsso.png)

    * Se pretender adicionar a sua aplicação para lista na galeria do utilizando palavras-passe SSO, selecione **SSO de palavra-passe** como acima.

    * Se tiver quaisquer problemas relativos ao acesso, contacte o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Atualizar/remover listagem existente

Para atualizar ou remover uma aplicação existente na Galeria de aplicações do Azure AD, primeiro tem de submeter o pedido no [aplicação rede Portal](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se tiver uma conta do Office 365, utilize-o para iniciar sessão neste portal. Caso contrário, utilizar a sua conta Microsoft (tais como o Outlook ou Hotmail) para iniciar sessão.

* Selecione a opção adequada da abaixo da imagem

    ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/updateorremove.png)
    
    * Se pretender atualizar uma aplicação existente, selecione **atualizar a lista de aplicações existentes**.

    * Se pretender remover uma aplicação existente a partir da galeria do Azure AD, selecione **remover listagem de aplicação existente**

    * Se tiver quaisquer problemas relativos ao acesso, contacte o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

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

A linha cronológica para o processo de listagem aplicação na galeria com suporte de aprovisionamento de utilizadores é dias úteis de 40 45.

   ![Linha cronológica de listagem aplicação saml para a Galeria](./media/active-directory-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Escalamentos de clientes

Para qualquer Escalamentos de clientes, enviar correio eletrónico para o [equipa de integração do Azure AD SSO](mailto:SaaSApplicationIntegrations@service.microsoft.com) que é SaaSApplicationIntegrations@service.microsoft.com e irá respondermos logo que possível.