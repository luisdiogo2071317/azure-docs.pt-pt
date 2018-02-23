---
title: "Como adicionar uma aplicação de multi-inquilino para a Galeria de aplicações do Azure AD | Microsoft Docs"
description: "Explica como pode listar a aplicação multi-inquilino programada personalizada na Galeria de aplicações do Azure AD"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: jeedes
ms.openlocfilehash: f29f7cbf118d4d70c1ea2cca174ff0cf0ba9bd14
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-add-a-multi-tenant-application-to-the-azure-ad-application-gallery"></a>Como adicionar uma aplicação de multi-inquilino para a Galeria de aplicações do Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>O que é a Galeria de aplicações do Azure AD?

Azure AD é um serviço de identidade baseado na nuvem. [Galeria de aplicações do Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) é um arquivo comum, onde todos os conectores de aplicações são publicados para o início de sessão único e o aprovisionamento de utilizadores. Os nossos mútua clientes que estão a utilizar o Azure AD como fornecedor de identidade procure conectores de aplicações SaaS diferentes, que são publicados aqui. Administrador de TI adiciona o conector da Galeria de aplicações e configura e utilizá-lo para o início de sessão único e o aprovisionamento. AD do Azure suporta todos os protocolos de Federação principal como SAML 2.0, OpenID Connect, OAuth e WS-Fed para o início de sessão único. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Se a aplicação que suporta SAML ou OpenIDConnect
Se tiver uma aplicação de multi-inquilino que gostaria de lista na Galeria de aplicações do Azure AD, tem primeiro de se certificar de que a aplicação suporta um dos seguintes único início de sessão tecnologias:

1. **OpenID Connect** - criar a aplicação multi-inquilino no Azure AD e implementar [framework de consentimento do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) para a sua aplicação. Envie o pedido de início de sessão para o ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de utilizador do cliente com base no ID de inquilino e de UPN do utilizador foi recebido no token. Submeta a aplicação tal como mencionado neste [artigo](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing).

2. **SAML** – se a aplicação suporta SAML 2.0, em seguida, iremos pode listar a aplicação na galeria e as instruções são listadas [aqui](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing)

Se a aplicação suporta um destes modos de início de sessão único e pretende listar a aplicação multi-inquilino na Galeria de aplicações do Azure AD, pode seguir os passos mencionados [isto](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artigo. 

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Se a aplicação não suporta SAML ou OpenIDConnect
Mesmo que a aplicação não suporta um dos modos destas, iremos ainda pode integrá-lo na nossa da galeria com a nossa tecnologia de palavra-passe-início de sessão único.

**Palavra-passe SSO** – criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [baseada em palavra-passe de início de sessão](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis). Baseado em palavra-passe SSO, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que várias os utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais. 

Se gostaria de listar a aplicação, em seguida, com esta tecnologia, submeta o pedido, tal como descrito no [isto](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-app-gallery-listing) artigo.

## <a name="escalations"></a>Escalamentos de clientes

Para qualquer Escalamentos de clientes, remova um e-mail para [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e vamos voltar para o utilizador ASAP.

## <a name="next-steps"></a>Passos Seguintes
[Como listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
