---
title: Onde AD do Azure armazena os dados de identidade para os clientes que | Microsoft Docs
description: Saiba mais sobre onde o Microsoft Azure Active Directory armazena dados relacionados com a identidade para que os seus clientes Europa.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.custom: it-pro
ms.openlocfilehash: 2ae60c620db4fb3d88554ad5653729c7c1325e97
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713495"
---
# <a name="where-does-microsoft-azure-active-directory-azure-ad-store-identity-data-for-european-customers"></a>Onde Microsoft Azure Active Directory (Azure AD) armazenar dados de identidade para os clientes que
Azure AD ajuda-o para gerir identidades de utilizador e criar políticas de acesso condicionada por intelligence que ajudam a proteger recursos da sua organização. Dados de identidade são armazenados numa localização que tem com base no endereço de organização fornecida ao subscrever o serviço. Por exemplo, quando lhe subscrever o Office 365 ou do Azure. Para obter informações específicas sobre armazenar os dados de identidade, pode utilizar o [é onde os dados localizados?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) secção do Microsoft Trust Center.

Enquanto permanecem dados de mais para o Azure AD relacionadas com identidade Europeu nos centros de dados alfabetos, existem cinco atributos relacionados com o utilizador que normalmente são armazenados nos E.U.A. centros de dados. Estes atributos são GivenName, apelido, userPrincipalName, domínio e PasswordHash. O atributo PasswordHash pode ser uma exceção e não armazenadas nos E.U.A. Se alguém utiliza no local, o método de autenticação federada que deixa o valor PasswordHash a sincronizar com o Azure AD. Além disso, há alguns dados operacionais, específicos do serviço que necessita para normal operação do Azure AD, que é armazenada nos E.U.A. e não inclui todos os dados pessoais.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Dados armazenados fora alfabetos centros de dados para os clientes que

Dados relacionados com o AD alfabetos identidade mais para o Azure, para organizações com endereços com base em Europeia, permanecem na Europa centros de dados. Os dados do Azure AD que não são armazenados nos centros de dados Europa, incluem:

- **Atributos relacionadas com identidade**

    Os seguintes atributos relacionados com a identidade serão replicados em Estados Unidos:

    - givenName
    - Apelido
    - userPrincipalName
    - Domínio
    - PasswordHash
    - sourceAnchor
    - accountEnabled
    - passwordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Autenticação multifator do Microsoft Azure (MFA) e de reposição de palavra-passe self-service do Azure AD (SSPR)**
    
    Todos os dados de utilizador em-rest nos centros de dados que os arquivos de MFA. No entanto, alguns dados específicos do serviço MFA são armazenados nos EUA, incluindo:
    
    - Autenticação de dois fatores e os respetivos dados pessoais relacionados poderão ser armazenados nos E.U.A. Se estiver a utilizar o MFA ou SSPR.
        - Autenticação de dois fatores todas as chamadas telefónicas ou um SMS a utilizar pode ser concluída pelos operadores de E.U.A.
        - Notificações push utilizando a aplicação Microsoft Authenticator requerem notificações a partir serviço de notificação do fabricante, o (Apple ou da Google), que podem estar fora da Europa.
        - Os códigos OATH sempre são validados nos E.U.A. 
    - Alguns registos de MFA e SSPR são armazenados nos E.U.A. durante 30 dias, independentemente do tipo de autenticação.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Azure AD B2C os arquivos de todos os dados de utilizador em-rest nos centros de dados Europa. No entanto, os registos operacionais (com dados pessoais removidos) permanecem no local de onde a pessoa que está a aceder os serviços. Por exemplo, se um utilizador de B2C acede ao serviço nos E.U.A., os registos operacionais permanecem nos E.U.A. Além disso, todos os dados de configuração de política que não contenha dados pessoais são armazenadas apenas nos E.U.A. Para obter mais informações sobre as configurações da política, consulte o [Azure Active Directory B2C: políticas incorporadas](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) artigo.

- **B2B do Microsoft Azure Active Directory (Azure AD B2B)** 
    
    Azure AD B2B os arquivos de todos os dados de utilizador em-rest nos centros de dados Europa. No entanto, B2B armazena os metadados não pessoais nas tabelas nos E.U.A. centros de dados. Esta tabela inclui campos como redeemUrl, invitationTicket, inquilino de recursos Id, InviteRedirectUrl e InviterAppId.

- **Serviços de domínio do Microsoft Azure Active Directory (Azure AD DS)**

    Azure AD DS armazena dados de utilizador na mesma localização que o cliente seleccionados Azure Virtual Network. Por isso, se a rede for fora da Europa, os dados são replicados e armazenados fora Europa.

- **Os serviços e aplicações integradas com o Azure AD**

    Os serviços e aplicações que se integram com o Azure AD tem acesso a dados de identidade. Avalie cada serviço e a aplicação para determinar como os dados de identidade são processados por esse serviço específico e a aplicação e se as mesmas cumprem os requisitos de armazenamento de dados da sua empresa.

    Para mais informações sobre residency de dados de serviços da Microsoft, consulte o [é onde os dados localizados?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) secção do Microsoft Trust Center.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre qualquer um das funções e funcionalidades descritas acima, consulte estes artigos.
- [Introdução ao Azure Active Directory](get-started-azure-ad.md)
- [O que é a multi-factor Authentication?](https://docs.microsoft.com/en-us/azure/active-directory/authentication/multi-factor-authentication)
- [Repor o Azure AD self-service palavra-passe](https://docs.microsoft.com/en-us/azure/active-directory/authentication/active-directory-passwords-overview)
- [O que é o Azure Active Directory B2C?](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview)
- [O que é a colaboração B2B do Azure AD?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Serviços de domínio do Azure Active Directory (AD)](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-overview)
