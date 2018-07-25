---
title: Segurança de palavras-passe em camada do Azure AD | Microsoft Docs
description: Explica a forma como o Azure AD impõe palavras-passe fortes e protege as palavras-passe de utilizador de cibercriminosos.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/28/2017
ms.author: lizross
ms.openlocfilehash: e3e97a5a9b768b3b9d4a36627d28955be1f3d9df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056924"
---
# <a name="a-multi-tiered-approach-to-azure-ad-password-security"></a>Uma abordagem em várias camadas para segurança de palavras-passe do Azure AD

Este artigo aborda algumas das melhores práticas que pode seguir como utilizador ou administrador para proteger o Azure Active Directory (Azure AD) ou a Conta Microsoft.

 > [!NOTE]
 > **Está aqui porque está a ter problemas em iniciar sessão?** Se assim for, [Eis como pode alterar e repor a sua própria palavra-passe](user-help/active-directory-passwords-update-your-own-password.md).
 >
 > Os administradores do Azure AD podem repor as palavras-passe de utilizador através da orientação no artigo [Repor a palavra-passe de um utilizador no Azure Active Directory](fundamentals/active-directory-users-reset-password-azure-portal.md).
 >

## <a name="password-requirements"></a>Requisitos de palavras-passe

O Azure AD incorpora as seguintes abordagens comuns relativamente à proteção de palavras-passe:

* Requisitos de comprimento de palavras-passe
* Requisitos de complexidade de palavras-passe
* Expiração de palavras-passe regular e periódica

Para obter informações sobre a reposição de palavra-passe no Azure Active Directory, veja o tópico [Reposição personalizada de palavras-passe do Azure AD para profissionais de TI](user-help/active-directory-passwords-update-your-own-password.md).

## <a name="azure-ad-password-protections"></a>Proteção de palavras-passe do Azure AD

O Azure AD e o Sistema de Contas Microsoft utilizam abordagens comprovadas da indústria para garantir a proteção das palavras-passe de utilizador e administrador, incluindo:

* Palavras-passe banidas dinamicamente
* Smart Password Lockout

Para obter informações sobre a gestão de palavras-passe com base nas investigações atuais, veja o documento técnico [Orientações Sobre Palavras-passe](https://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Palavras-passe banidas dinamicamente

O Azure AD e as Contas Microsoft banem dinamicamente as palavras-passe utilizadas frequentemente, para salvaguardar a proteção das palavras-passe. A equipa do Azure AD Identity Protection analisa, rotineiramente, as listas de palavras-passe banidas, impedindo os utilizadores de selecionar as que são mais utilizadas. Este serviço está disponível para clientes do Azure AD e do Serviço de Conta Microsoft.

Ao criar palavras-passe, é importante que os administradores incentivem os utilizadores a escolher expressões para palavras-passe que incluam uma combinação única de letras, números, carateres ou palavras. Esta abordagem ajuda a tornar as palavras-passe de utilizador praticamente impossíveis de ficarem comprometidas, mas fáceis de memorizar pelos utilizadores.

#### <a name="password-breaches"></a>Violações de palavras-passe

A Microsoft está sempre a trabalhar para estar um passo à frente dos cibercriminosos.

A equipa do Azure AD Identity Protection analisa continuamente as palavras-passe que são utilizadas mais vezes. Os cibercriminosos também utilizam estratégias semelhantes para informar os respetivos ataques, que podem incluir a criação de [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table), para descodificar hashes de palavras-passe.

A Microsoft analisa continuamente [violações a dados](https://www.privacyrights.org/data-breaches), para manter uma lista de palavras-passe banidas dinamicamente atualizada, assegurando que as palavras-passe vulneráveis são banidas antes de se tornarem uma ameaça real aos clientes do Azure AD. Para obter mais informações sobre os nossos esforços de segurança atuais, veja [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx) (Relatório de Informações de Segurança da Microsoft).

### <a name="smart-password-lockout"></a>Smart Password Lockout

Quando o Azure AD deteta que um potencial cibercriminoso está a tentar piratear uma palavra-passe de utilizador, bloqueamos essa conta com o Smart Password Lockout. O Azure AD foi concebido para determinar o risco associado a sessões de início de sessão específicas. Com a utilização dos dados de segurança mais atualizados, aplicamos semântica de bloqueio às ameaças cibernéticas.

Se um utilizador for bloqueado no Azure AD, o ecrã do mesmo será semelhante ao seguinte:

  ![Bloqueado do Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

Relativamente a outras contas Microsoft, o ecrã será parecido com o seguinte:

  ![Bloqueado de uma conta Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Para obter informações sobre a reposição de palavra-passe no Azure Active Directory, veja o tópico [Reposição personalizada de palavras-passe do Azure AD para profissionais de TI](user-help/active-directory-passwords-update-your-own-password.md).

  >[!NOTE]
  >Se for administrador do Azure AD, poderá ser útil utilizar o [Windows Hello](https://www.microsoft.com/windows/windows-hello) para impedir que os seus utilizadores criem palavras-passe tradicionais.
  >

## <a name="next-steps"></a>Passos seguintes

* [Como atualizar a sua própria palavra-passe](user-help/active-directory-passwords-update-your-own-password.md)
* [The fundamentals of Azure identity management](fundamentals-identity.md) (As noções básicas da gestão de identidades do Azure)
* [Reportar atividade de reposição de palavras-passe](authentication/howto-sspr-reporting.md)
