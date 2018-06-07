---
title: Ativar avaliação de utilizar o Azure AD | Azure
description: Ativar o tipo de listagem de avaliação com o Azure Active Directory (Azure AD) no Azure Marketplace e AppSource para publicadores de aplicações e serviço
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
ms.openlocfilehash: 4140ba98c0c65c22674c61dc7266818af904e777
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825231"
---
# <a name="enable-trial-using-azure-ad"></a>Ativar avaliação de utilizar o Azure AD  
Azure Active Directory (Azure AD) é um serviço de identidade de nuvem que permite a autenticação com a Microsoft, uma conta escolar ou profissional, utilizando as estruturas norma da indústria: OAuth e o OpenID Connect.  
*   Para obter mais informações acerca do AD do Azure, visite o Azure Active Directory página localizada em [azure.microsoft.com/services/active-directory](https://azure.microsoft.com/services/active-directory).  

O utilizador e os clientes são autenticados no marketplace utilizar o Azure AD. Depois do cliente seleciona a versão de avaliação listagem no marketplace, o cliente é redirecionado para o seu ambiente de avaliação.  No seu ambiente de avaliação, pode configurar o seu cliente diretamente, sem necessidade de início de sessão passos adicionais. A aplicação ou a oferta recebe um token do Azure AD durante a autenticação, que inclui informações importantes de utilizador para criar uma conta de utilizador na sua aplicação ou a oferta. Pode automatizar a configuração e aumentar a probabilidade de conversão.  
*   Para mais informações sobre o token enviado a partir do Azure AD durante a autenticação, visite os Tokens de exemplo secção localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens)

Utilize o Azure AD para ativar a autenticação de um clique na sua aplicação ou a versão de avaliação.  
*   Simplificar a experiência do cliente do marketplace para avaliação.  
*   Manter a funcionalidade de uma experiência no produto, mesmo quando o utilizador é redirecionado no Marketplace ao seu ambiente de domínio ou a versão de avaliação.  
*   Reduza a probabilidade de abandonment no redirecionamento, porque existem não existem passos adicionais do início de sessão.  
*   Reduza barreiras as eficazes de implementação para o número elevado de utilizadores do Azure AD.  

## <a name="verify-your-azure-ad-integration-on-the-marketplace-multitenant-apps"></a>Verificar o Azure AD integração no marketplace: aplicações de multi-inquilino  
Suporta as seguintes opções para a sua solução com o Azure AD.  
*   Registe a sua aplicação no storefronts no marketplace.  
*   Ative a funcionalidade de suporte de arquitetura "multitenancy" no Azure AD para obter uma experiência de avaliação de um clique.  
    *   Para obter mais informações sobre o registo de aplicação, visite as aplicações de Integrating com o Azure Active Directory página localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Se estiver a utilizar o Azure AD federado início de sessão único (SSO), em seguida, siga estes passos.  
1.  Registe a sua aplicação no marketplace. 
2.  Desenvolva SSO com o Azure AD através da utilização de OAuth 2.0 ou o OpenID Connect.  
    *   Para obter mais informações sobre o OAuth 2.0, visite o OAuth 2.0 página localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).  
    *   Para obter mais informações sobre como abrir ID Connect, visite o OpenID Connect página localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  
3.  Ative a funcionalidade de suporte de arquitetura "multitenancy" no Azure AD para fornecer uma experiência de avaliação de um clique.  
    *   Para obter mais informações sobre AppSource certificação, visite AppSource página certificação localizada em [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-on-the-marketplace-single-tenant-apps"></a>Verificar o Azure AD integração no marketplace: aplicações de inquilino único  
Suporta uma das seguintes opções para a sua solução de inquilino único.  
*   Adicione utilizadores ao seu diretório como os utilizadores convidados através do Azure AD B2B.  
    *   Para mais informações sobre o Azure AD B2B, visite o que é o Azure AD B2B página de colaboração, localizada em [docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configurar manualmente as avaliações de clientes que utilizam me de contacto.  
*   Desenvolva uma unidade de teste por cliente.  
*   Crie uma aplicação de demonstração de exemplo multi-inquilino com o SSO.  

## <a name="next-steps"></a>Passos Seguintes
*   Visite o [Azure Marketplace e AppSource publicador guia](./marketplace-publishers-guide.md) página.  
 
---  

