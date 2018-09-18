---
title: Onde o Azure AD armazena os dados de identidade dos clientes europeus | Microsoft Docs
description: Saiba mais sobre onde é que o Microsoft Azure Active Directory armazena dados relacionados com identidade dos clientes europeus.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/17/2018
ms.custom: it-pro
ms.openlocfilehash: d0015f40714b639d15245827ae0da3ca0f132df4
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733402"
---
# <a name="where-does-microsoft-azure-active-directory-azure-ad-store-identity-data-for-european-customers"></a>Onde é que o Microsoft Azure Active Directory (Azure AD) armazena os dados de identidade dos clientes europeus
O Azure AD ajuda-o a gerir identidades de utilizadores e a criar políticas de acesso orientadas por inteligência que ajudam a proteger os recursos da sua organização. Os dados de identidade são armazenados numa localização que tem por base a morada que a sua organização indicou quando subscreveu o serviço. Por exemplo, quando subscreveu o Office 365 ou o Azure. Para obter informações específicas sobre onde é que os seus dados de identidade são armazenados, pode ver a secção [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Onde estão localizados os seus dados?) do Microsoft Trust Center.

Embora a maioria dos dados de identidade de utilizadores europeus relacionados com o Azure AD sejam armazenados em datacenters na Europa, há cinco atributos de utilizador que são, normalmente, armazenados em datacenters nos E.U.A. Esses atributos são GivenName, Surname, userPrincipalName, Domain e PasswordHash. O atributo PasswordHash pode ser uma exceção e não ser armazenado nos E.U.A., se alguém utilizar um método de autenticação federada no local que interrompa a sincronização do valor PasswordHash com o Azure AD. Além disso, são necessários alguns dados operacionais específicos do serviço para o normal funcionamento do Azure AD, os quais são armazenados nos E.U.A. e não incluem dados pessoais.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Dados armazenados fora dos datacenters europeus para clientes europeus

A maioria dos dados de identidade de clientes europeus relacionados com o Azure AD, relativamente às organizações que tenham moradas sedeadas na Europa, permanecem em datacenters europeus. Os dados Azure AD que não são armazenados nesses datacenters incluem:

- **Atributos relacionados com a identidade**

    Os seguintes atributos relacionados com a identidade serão replicados nos Estados Unidos:

    - GivenName
    - Apelido
    - userPrincipalName
    - Domain
    - PasswordHash
    - SourceAnchor
    - AccountEnabled
    - PasswordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Autenticação multifator do Microsoft Azure (MFA) e reposição personalizada de palavras-passe do Azure AD (SSPR)**
    
    A MFA armazena todos os dados de utilizador inativos em datacenters europeus. Contudo, alguns dados específicos deste serviço são armazenados nos E.U.A., incluindo:
    
    - A autenticação multifator e os dados pessoais relacionados com a mesma podem ser armazenados nos E.U.A., caso esteja a utilizar a MFA ou a SSPR.
        - As autenticações de dois fatores que utilizem chamadas ou SMS podem ser realizadas por operadoras norte-americanas.
        - As notificações push que utilizem a aplicação Microsoft Authenticator precisam de notificações do serviço de notificações do fabricante (Apple ou Google), que pode estar fora da Europa.
        - Os códigos OATH são sempre validados nos E.U.A. 
    - Alguns registos da MFA e do SSPR são armazenados nos E.U.A. durante 30 dias, independentemente do tipo de autenticação.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    O Azure AD B2C armazena todos os dados de utilizador inativos em datacenters europeus. No entanto, os registos operacionais (com os dados pessoais removidos) permanecem na localização na qual a pessoa está a aceder aos serviços. Por exemplo, se um utilizador do B2C aceder ao serviço nos E.U.A., os registos operacionais permanecem nesse país. Além disso, todos os dados de configuração de políticas que não contenham dados pessoais são armazenados apenas nos E.U.A. Para obter mais informações sobre as configurações de políticas, veja o artigo [Azure Active Directory B2C: Built-in policies](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) (Azure Active Directory B2C: Políticas incorporadas).

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    O Azure AD B2B armazena todos os dados de utilizador inativos em datacenters europeus. No entanto, o B2B armazena os respetivos metadados não pessoais em tabelas em datacenters nos E.U.A. Essas tabelas incluem campos como redeemUrl, invitationTicket, resource tenant Id, InviteRedirectUrl e InviterAppId.

- **Microsoft Azure Active Directory Domain Services (Azure AD DS)**

    O Azure AD DS armazena os dados de utilizador na mesma localização da rede virtual do Azure que os utilizadores selecionam. Assim, se a rede estiver fora da Europa, os dados são replicados e armazenados fora deste continente.

- **Serviços e aplicações integrados no Azure AD**

    Todos os serviços e aplicações que são integrados no Azure AD têm acesso aos dados de identidade. Analise cada serviço e aplicação para determinar de que forma é que processam os dados de identidade e se cumprem os requisitos de armazenamento de dados da sua empresa.

    Para obter mais informações sobre a residência de dados dos serviços Microsoft, veja a secção [Where is your data located?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) (Onde estão localizados os seus dados) do Microsoft Trust Center.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre qualquer uma das funcionalidades e características descritas anteriormente, veja os artigos abaixo.
- [What is Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication) (O que é o Multi-Factor Authentication)
- [Azure AD self-service password reset](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-passwords-overview) (Reposição personalizada de palavras-passe do Azure AD)
- [O que é o Azure Active Directory B2C?](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [O que é a colaboração B2B do Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Azure Active Directory (AD) Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview)
