---
title: Ativar uma listagem de Microsoft AppSource e do Azure Marketplace utilizando o Azure Active Directory | Azure
description: Ative um tipo de lista com o Azure Active Directory no Azure Marketplace e AppSource para editores de aplicações e serviços.
services: Azure, AppSource, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/12/2018
ms.author: qianw211
ms.openlocfilehash: d7fd09928c0a687755d216e7f10f7eac23677c63
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986426"
---
# <a name="enable-a-microsoft-appsource-and-azure-marketplace-listing-by-using-azure-active-directory"></a>Ativar uma listagem de Microsoft AppSource e do Azure Marketplace utilizando o Azure Active Directory

Microsoft Azure Active Directory (Azure AD) é um serviço de identidade de cloud que permite a autenticação com uma conta Microsoft através de estruturas de norma da indústria.  Para obter mais informações sobre o Azure AD, consulte [do Azure Active Directory](https://azure.microsoft.com/services/active-directory).

## <a name="benefits-of-using-azure-active-directory"></a>Vantagens da utilização do Azure Active Directory

Os clientes do Microsoft AppSource e do Azure Marketplace utilizam experiências no produto para procurar os catálogos de listagem, que exigirão um início de sessão para o produto.  Ao integrar a sua aplicação com o Azure AD, pode agilizar o envolvimento e otimizar a experiência do cliente. O Azure AD:

- Permite início de sessão único (SSO) para milhões de utilizadores empresariais.
- Permite que a experiência de início de sessão de usuário consistente em todas as aplicações publicadas pelos parceiros diferentes.
- Fornece autenticação escalável e multiplataformas para o seu dispositivo móvel e na nuvem aplicações.

Conforme detalhado na secção abaixo, determinadas ofertas são necessárias para implementar o Azure AD para publicar no Marketplace.

## <a name="azure-active-directory-requirements"></a>Requisitos do Azure Active Directory

Existem diferentes [opções de listagem e oferecem tipos](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) para Microsoft AppSource e do Azure Marketplace.  Requisitos do AD do Azure para estas opções de listagem e tipos de oferta são mostrados abaixo:

| **Tipo de oferta**    | **AAD SSO necessária?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Contactar-me | Avaliação | Test Drive | Transact |
| Máquina Virtual | N/A | Não | Não | Não |
| Aplicações do Azure (modelo de solução)  | N/A | N/D | N/D | N/A |
| Aplicações geridas  | N/A | N/D | N/A | Não |
| SaaS  | Não | Sim | Sim | Sim |
| Contentores  | N/A | N/D | N/A | Não |
| Serviços de consultoria  | Não | N/A | N/D | N/A |

Para obter mais informações sobre os requisitos técnicos de SaaS, veja [guia de publicação da oferta de aplicações de SaaS](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

## <a name="integration-with-azure-active-directory"></a>Integração com o Azure Active Directory

Para obter informações sobre como integrar com o Azure AD para ativar o SSO, visite https://aka.ms/aaddev.

Para obter mais informações sobre o SSO do Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)?

## <a name="enable-a-trial-listing-by-using-azure-active-directory"></a>Ativar uma versão de avaliação de listagem com o Azure Active Directory

Depois de um cliente seleciona a versão de avaliação listagem no Marketplace, seu cliente é redirecionado para o seu ambiente de avaliação. No seu ambiente de avaliação, pode configurar o seu cliente diretamente sem a necessidade de passos de início de sessão adicionais. A aplicação ou a oferta recebe um token do Azure AD durante a autenticação. O token inclui informações de utilizador valiosas que são utilizadas para criar uma conta de utilizador na sua aplicação ou oferta. Pode automatizar a instalação de cliente e aumentar a probabilidade de conversão.

Para obter mais informações sobre o token que é enviado do Azure AD durante a autenticação, consulte [tokens de exemplo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens).

Utilize o Azure AD para ativar a autenticação de um clique na sua aplicação ou a versão de avaliação. O Azure AD oferece as seguintes vantagens: 
*   Simplificar a experiência do cliente do Marketplace à versão de avaliação.
*   Manter o funcionamento de uma experiência no produto, mesmo quando o utilizador é redirecionado a partir do Marketplace para o seu ambiente de domínio ou de avaliação.
*   Reduza a probabilidade de abandonment após o redirecionamento, uma vez que existem não existem passos de início de sessão adicionais.
*   Reduza as barreiras de implementação para a população grande de utilizadores do Azure AD.

### <a name="verify-your-azure-ad-integration-in-the-marketplace-multitenant-apps"></a>Certifique-se a sua integração do Azure AD no Marketplace: aplicações multi-inquilino
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

### <a name="verify-your-azure-ad-integration-in-the-marketplace-single-tenant-apps"></a>Certifique-se a sua integração do Azure AD no Marketplace: aplicações de inquilino único
Utilize o Azure AD para suportar uma das seguintes opções para a sua solução de inquilino único: 
*   Adicionar utilizadores ao seu diretório como os utilizadores convidados com o Azure Active Directory B2B (Azure AD B2B). Para obter mais informações sobre o Azure AD B2B, consulte [o que é a colaboração B2B do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Configure manualmente as versões de avaliação para os clientes utilizando o contacto-Me a opção de publicação.
*   Desenvolva uma versão de teste de cada cliente.
*   Crie uma aplicação de demonstração de exemplo do multi-inquilino que utiliza o SSO.

## <a name="next-steps"></a>Passos Seguintes

Se ainda não fez isso, 
- [Registar](https://azuremarketplace.microsoft.com/sell) no marketplace.

Se é registrado e estiver a criar uma nova oferta ou trabalhando num já existente
- [Inicie sessão no Portal de parceiro de Cloud](https://cloudpartner.azure.com/) para criar ou concluir sua oferta.

