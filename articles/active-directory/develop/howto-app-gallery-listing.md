---
title: Inclua a sua aplicação na Galeria de aplicações do Azure Active Directory | Documentos da Microsoft
description: Saiba como listar uma aplicação que suporta início de sessão único na Galeria de aplicações do Azure Active Directory
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
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: elisol, bryanla
ms.custom: aaddev
ms.openlocfilehash: d5c00e9df9c1bfee0c665cafc763c52a36f98052
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44345850"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Listar a aplicação na galeria de aplicações do Azure Active Directory


##  <a name="what-is-the-azure-ad-application-gallery"></a>O que é a Galeria de aplicações do Azure AD?

Azure Active Directory (Azure AD) é um serviço de identidade com base na cloud. O [Galeria de aplicações do Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) é na loja de aplicações do Azure Marketplace, onde todos os conectores de aplicações são publicados para início de sessão único e o aprovisionamento de utilizadores. Os clientes que utilizam o Azure AD como fornecedor de identidade encontrar os conectores de aplicações SaaS diferentes publicados aqui. Os administradores de TI adicionar conectores a partir da Galeria de aplicações e, em seguida, configurar e utilizam os conectores para início de sessão único e o aprovisionamento. O Azure AD suporta todos os protocolos de Federação principal para início de sessão único, incluindo SAML 2.0, OpenID Connect, OAuth e WS-Fed.

## <a name="what-are-the-benefits-of-listing-an-application-in-the-gallery"></a>Quais são os benefícios de uma aplicação na Galeria de listagem?

*  Os clientes encontrar a melhor única início de sessão experiência possível.

*  Configuração do aplicativo é simples e minimalista.

*  Uma pesquisa rápida localiza a sua aplicação na galeria.

*  Gratuito, básico, e todos os clientes Premium do Azure AD podem utilizar esta integração.

*  Os clientes mútuos obtém um tutorial passo a passo de configuração.

*  Os clientes que utilizam SCIM podem utilizar o aprovisionamento para a mesma aplicação.

## <a name="prerequisites"></a>Pré-requisitos

- Para aplicativos de federados (ID de aberto e SAML/WS-Fed), a aplicação tem de suportar o modelo de SaaS para ser listado na galeria do Azure AD. As aplicações de galeria enterprise devem suportar várias configurações de cliente e não qualquer cliente específico.

- Para abrir ID Connect, a aplicação deve estar vários inquilinos e [framework de consentimento do Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md#overview-of-the-consent-framework) devem ser implementadas corretamente para a aplicação. O utilizador pode enviar o pedido de início de sessão para um ponto de extremidade comum para que todos os clientes podem fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador com base no ID de inquilino e o UPN do utilizador recebido no token.

- Para SAML 2.0/WS-Fed, a aplicação tem de ter a capacidade de fazer a integração de SSO SAML/WS-Fed no modo de SP ou IDP. Certifique-se de que isso está funcionando corretamente antes de submeter o pedido.

- Para SSO de palavra-passe, certifique-se que a aplicação suporta a autenticação de formulário para que os cofres de palavras de palavra-passe podem ser feito para fazer o trabalho de logon único conforme esperado.

- Para pedidos de aprovisionamento de utilizadores automática, o aplicativo deve listado na galeria com recurso de início de logon único habilitado a utilizar o protocolo de Federação descrito acima. Pode pedir para SSO e aprovisionamento em conjunto no portal, se ainda não estiver na lista de utilizadores.

##  <a name="implementing-sso-using-federation-protocol"></a>A implementação do SSO através do protocolo de Federação

Para listar uma aplicação na Galeria de aplicações do Azure AD, tem primeiro de implementar um dos seguintes protocolos de Federação suportados pelo Azure AD e concordo com os termos de Galeria de aplicações do Azure AD e condições. Leia os termos e condições da Galeria de aplicações do Azure AD partir [aqui](https://azure.microsoft.com/en-us/support/legal/active-directory-app-gallery-terms/).

*   **OpenID Connect**: para integrar a aplicação com o Azure AD através do protocolo Abrir ID Connect, siga os [instruções de desenvolvedores](authentication-scenarios.md).

    ![Linha cronológica de listagem aplicação OpenID Connect para a Galeria](./media/howto-app-gallery-listing/openid.png)

    * Se pretender adicionar a sua aplicação à lista na Galeria através do OpenID Connect, selecione **OpenID Connect e OAuth 2.0** como anteriormente.

    * Se tiver quaisquer problemas de acesso, entre em contato com o [equipa de integração do SSO do Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

*   **SAML 2.0** ou **WS-Fed**: se a sua aplicação suporta SAML 2.0, pode integrá-lo diretamente com um inquilino do Azure AD utilizando o [instruções para adicionar uma aplicação personalizada](../manage-apps/configure-single-sign-on-non-gallery-applications.md).

    ![Linha cronológica de listagem aplicação SAML 2.0 ou WS-Fed na Galeria](./media/howto-app-gallery-listing/saml.png)

    * Se pretende adicionar a sua aplicação à lista de sessão com a galeria **SAML 2.0** ou **WS-Fed**, selecione **SAMl 2.0/WS-Fed** como anteriormente.

    * Se tiver quaisquer problemas de acesso, entre em contato com o [equipa de integração do SSO do Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implementing-sso-using-password-sso"></a>A implementação do SSO através do SSO de palavra-passe

Criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [baseado em palavra-passe de início de sessão único](../manage-apps/what-is-single-sign-on.md). SSO de palavra-passe, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe para aplicações web que não suportam o Federação de identidades. Também é útil para cenários em que vários utilizadores tem de partilhar uma única conta, tal como para contas de aplicação de comunicação social da sua organização.

![Linha cronológica de listagem aplicação SSO de palavra-passe na Galeria](./media/howto-app-gallery-listing/passwordsso.png)

* Se pretender adicionar a sua aplicação à lista na Galeria através do SSO de palavra-passe, selecione **SSO de palavra-passe** como anteriormente.

* Se tiver quaisquer problemas de acesso, entre em contato com o [equipa de integração do SSO do Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

##  <a name="updateremove-existing-listing"></a>Atualizar/remover listagem existente

Para atualizar ou remover uma aplicação existente na Galeria de aplicações do Azure AD, primeiro tem de submeter o pedido no [Portal de rede da aplicação](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se tiver uma conta do Office 365, usá-lo para iniciar sessão neste portal. Caso contrário, utilize a sua conta Microsoft (como o Outlook ou Hotmail) para iniciar sessão.

* Selecione a opção adequada da abaixo da imagem

    ![Linha cronológica de listagem aplicação de saml numa galeria de](./media/howto-app-gallery-listing/updateorremove.png)

    * Se quiser atualizar um aplicativo existente, selecione **atualizar a lista de aplicações existente**.

    * Se pretender remover um aplicativo existente a partir da galeria do Azure AD, selecione **remover listagem de aplicação existente**

    * Se tiver quaisquer problemas de acesso, entre em contato com o [equipa de integração do SSO do Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). 

## <a name="submit-the-request-in-the-portal"></a>Submeter o pedido no portal

Depois de o testar que a sua integração de aplicações funciona com o Azure AD, submeter o seu pedido de acesso no nosso [Portal de rede da aplicação](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Se tiver uma conta do Office 365, usá-lo para iniciar sessão neste portal. Caso contrário, utilize a sua conta Microsoft (como o Outlook ou Hotmail) para iniciar sessão.

Depois de iniciar sessão, é apresentada a página seguinte. Forneça uma justificativa comercial para que precisam de acesso na caixa de texto e, em seguida, selecione **pedir acesso**. Nossa equipe analisa os detalhes e dá-lhe acesso em conformidade. Depois disso, pode iniciar sessão portal e submeter o pedido detalhado para a aplicação.

Se tiver quaisquer problemas de acesso, entre em contato com o [equipa de integração do SSO do Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

![Pedido de acesso no portal do SharePoint](./media/howto-app-gallery-listing/accessrequest.png)

## <a name="timelines"></a>Linhas cronológicas
    
A linha cronológica para o processo de listagem, uma aplicação de WS-Fed na galeria ou SAML 2.0 é 7-10 dias úteis.

   ![Linha cronológica de listagem aplicação de saml numa galeria de](./media/howto-app-gallery-listing/timeline.png)

A linha cronológica para o processo de uma aplicação de ligação OpenID na Galeria de listagem é 2 a 5 dias úteis.

   ![Linha cronológica de listagem aplicação de saml numa galeria de](./media/howto-app-gallery-listing/timeline2.png)

A linha cronológica para o processo de listagem a aplicação na galeria, com suporte de aprovisionamento de utilizadores é 40 a 45 dias de negócios.

   ![Linha cronológica de listagem aplicação de saml numa galeria de](./media/howto-app-gallery-listing/provisioningtimeline.png)

## <a name="escalations"></a>Escalonamento

Para qualquer escalonamentos, envie um e-mail para o [equipa de integração do SSO do Azure AD](mailto:SaaSApplicationIntegrations@service.microsoft.com) que é SaaSApplicationIntegrations@service.microsoft.com e vamos responder mais rapidamente possível.