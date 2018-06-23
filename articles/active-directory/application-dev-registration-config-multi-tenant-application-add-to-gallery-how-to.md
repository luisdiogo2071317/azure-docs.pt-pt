---
title: Adicionar uma aplicação multi-inquilino para a Galeria de aplicações do Azure AD | Microsoft Docs
description: Explica como pode listar a aplicação multi-inquilino desenvolvidos personalizado na Galeria de aplicações do Azure AD.
services: active-directory
documentationCenter: na
author: barbkess
manager: mtillman
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 4ff64847e057a67f0ed3378baefa7762c159aba8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333355"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Adicionar uma aplicação multi-inquilino para a Galeria de aplicações do Azure AD

## <a name="what-is-the-azure-ad-application-gallery"></a>O que é a Galeria de aplicações do Azure AD?

Azure Active Directory (Azure AD) é um serviço de identidade baseada na nuvem. O [Galeria de aplicações do Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/) na loja de aplicações do Azure Marketplace, onde todos os conectores de aplicações são publicados para o início de sessão único e o aprovisionamento de utilizadores. Os clientes que utilizam o Azure AD como um fornecedor de identidade localizar os conectores de aplicações SaaS diferentes publicados aqui. Os administradores de TI adicionar conectores a partir da Galeria de aplicações e, em seguida, configurar e utilizarem os conectores para o início de sessão único e o aprovisionamento. AD do Azure suporta todos os protocolos de Federação principal, incluindo SAML 2.0, OpenID Connect, OAuth e WS-Fed para o início de sessão único. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Se a aplicação que suporta SAML ou OpenIDConnect
Se tiver uma aplicação multi-inquilino que pretende que sejam apresentados na Galeria de aplicações do Azure AD, tem primeiro de se certificar de que a aplicação suporta um dos seguintes único início de sessão tecnologias:

- **OpenID Connect**: para que a aplicação listada, criar a aplicação multi-inquilino no Azure AD e implementar o [framework de consentimento do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) para a sua aplicação. Envie o pedido de início de sessão para um ponto final comum, para que qualquer cliente pode fornecer consentimento para a aplicação. Pode controlar o acesso de um utilizador com base no ID de inquilino e de UPN do utilizador foi recebido no token. Submeter a aplicação utilizando o processo descrito na [listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: se a aplicação suporta SAML 2.0, a aplicação pode ser apresentada na galeria. Siga as instruções em [listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Se a aplicação não suporta SAML ou OpenIDConnect
As aplicações que não suportam SAML ou OpenIDConnect ainda podem ser integradas no Galeria de aplicações através de palavra-passe único início de sessão tecnologia.

Palavra-passe-início de sessão único, também designado por cofres de palavras de palavra-passe, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que vários utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais. 

Se pretende listar a aplicação com esta tecnologia:
1. Criar uma aplicação web que tem uma página de início de sessão do HTML para configurar [palavra-passe-início de sessão único](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Submeter o pedido, conforme descrito em [listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Escalamentos de clientes

Para qualquer Escalamentos de clientes, enviar correio eletrónico para [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e iremos irá voltar a si logo que possível.

## <a name="next-steps"></a>Passos Seguintes
Saiba como [listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
