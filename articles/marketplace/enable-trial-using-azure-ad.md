---
title: Ativar uma versão de avaliação no Azure Marketplace utilizando o Azure Active Directory | Azure
description: Ative uma versão de avaliação listagem Tipo com o Azure Active Directory no Azure Marketplace e AppSource para editores de aplicações e serviços.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: c5b7b4967c1acef733d366e651d50706db42aace
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160472"
---
# <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Ativar uma versão de avaliação de listagem com o Azure Active Directory

Azure Active Directory (Azure AD) é um serviço de identidade de cloud que permite a autenticação com um Microsoft trabalhar conta escolar ou profissional ao utilizar as estruturas de norma da indústria. O Azure AD suporta a autenticação OAuth e OpenID Connect. O [do Azure Marketplace](https://azuremarketplace.microsoft.com) utiliza o Azure AD para autenticar e dos seus clientes.

Para obter mais informações sobre o Azure AD, consulte [do Azure Active Directory](https://azure.microsoft.com/services/active-directory).

Depois de um cliente seleciona a versão de avaliação listagem no Marketplace, seu cliente é redirecionado para o seu ambiente de avaliação. No seu ambiente de avaliação, pode configurar o seu cliente diretamente sem a necessidade de passos de início de sessão adicionais. A aplicação ou a oferta recebe um token do Azure AD durante a autenticação. O token inclui informações de utilizador valiosas que são utilizadas para criar uma conta de utilizador na sua aplicação ou oferta. Pode automatizar a instalação de cliente e aumentar a probabilidade de conversão.

Para obter mais informações sobre o token que é enviado do Azure AD durante a autenticação, consulte [tokens de exemplo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Utilize o Azure AD para ativar a autenticação de um clique na sua aplicação ou a versão de avaliação. O Azure AD oferece as seguintes vantagens: 
*   Simplificar a experiência do cliente do Marketplace à versão de avaliação.
*   Manter o funcionamento de uma experiência no produto, mesmo quando o utilizador é redirecionado a partir do Marketplace para o seu ambiente de domínio ou de avaliação.
*   Reduza a probabilidade de abandonment após o redirecionamento, uma vez que existem não existem passos de início de sessão adicionais.
*   Reduza as barreiras de implementação para a população grande de utilizadores do Azure AD.

## <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Certifique-se a sua integração do Azure AD no Marketplace: aplicações multi-inquilino
Utilize o Azure AD para suportar as seguintes opções para a sua solução:
*   Registe a sua aplicação nas lojas no Marketplace.
*   Ative a funcionalidade de suporte de arquitetura "multitenancy" no Azure AD para obter uma experiência de avaliação num único clique.

Para obter mais informações sobre o registo de aplicação, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Se estiver a utilizar o único federado do Azure AD início de sessão (SSO), conclua estes passos:
1.  Registe a sua aplicação no Marketplace. 
2.  Desenvolva SSO com o Azure AD com o OAuth 2.0 ou OpenID Connect.
    *   Para obter mais informações sobre o OAuth 2.0, consulte [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
    *   Para obter mais informações sobre como abrir ID Connect, consulte [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).
3.  Ative a funcionalidade de suporte de arquitetura "multitenancy" no Azure AD para fornecer uma experiência de avaliação num único clique.
    
    Para obter mais informações sobre a certificação do AppSource, consulte [certificação AppSource](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Certifique-se a sua integração do Azure AD no Marketplace: aplicações de inquilino único
Utilize o Azure AD para suportar uma das seguintes opções para a sua solução de inquilino único: 
*   Adicionar utilizadores ao seu diretório como os utilizadores convidados com o Azure Active Directory B2B (Azure AD B2B).
    
    Para obter mais informações sobre o Azure AD B2B, consulte [o que é a colaboração B2B do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configure manualmente as versões de avaliação para os clientes utilizando o contacto-Me a opção de publicação.
*   Desenvolva uma versão de teste de cada cliente.
*   Crie uma aplicação de demonstração de exemplo multi-inquilino que utiliza o SSO.

## <a name="next-steps"></a>Passos Seguintes
*   Reveja os [guia de publicação do Azure Marketplace e AppSource](./marketplace-publishers-guide.md).
