---
title: 'Azure AD Connect: Totalmente integrada início de sessão único | Documentos da Microsoft'
description: Este tópico descreve o Azure Active Directory (Azure AD) totalmente integrada início de sessão único e como ele permite que forneça verdadeiro início de sessão único para utilizadores empresariais de área de trabalho no interior da rede empresarial.
services: active-directory
keywords: o que é o Azure AD Connect, a instalação do Active Directory, necessário componentes para o Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 88869fbeef0475f2c674e0f154a3624545182363
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213236"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>O Azure Active Directory totalmente integrada início de sessão único

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>O que é o Azure Active Directory totalmente integrada início de sessão único?

Azure Active Directory totalmente integrada início de sessão único (Azure AD Seamless SSO) inicia automaticamente os utilizadores quando estão nos respetivos dispositivos empresariais ligados à sua rede empresarial. Quando ativada, os utilizadores não precisam digitar as palavras-passe para iniciar sessão com o Azure AD e, em geral, até mesmo digitar seus nomes de utilizador. Esta funcionalidade fornece aos utilizadores acesso fácil aos seus aplicativos baseados na nuvem sem a necessidade de quaisquer componentes no local adicionais.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

SSO totalmente integrado pode ser combinada com ambos os [sincronização de Hash de palavra-passe](active-directory-aadconnectsync-implement-password-hash-synchronization.md) ou [autenticação pass-through](active-directory-aadconnect-pass-through-authentication.md) métodos de início de sessão.

![Totalmente integrado início de sessão único](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>É o SSO totalmente integrado _não_ aplicável ao Active Directory Federation Services (ADFS).

## <a name="key-benefits"></a>Principais vantagens

- *Grande experiência de usuário*
  - Os utilizadores são iniciados automaticamente no local e aplicações baseadas na nuvem.
  - Os utilizadores não têm de introduzir as palavras-passe repetidamente.
- *Fácil de implementar e administrar*
  - Não existem componentes adicionais necessários no local para fazer isso funcionar.
  - Funciona com qualquer método de autenticação na nuvem - [sincronização de Hash de palavra-passe](active-directory-aadconnectsync-implement-password-hash-synchronization.md) ou [autenticação pass-through](active-directory-aadconnect-pass-through-authentication.md).
  - Podem ser implementadas para alguns ou todos os seus utilizadores através da política de grupo.
  - Registe dispositivos não Windows 10 no Azure AD sem a necessidade de qualquer infraestrutura do AD FS. Esta capacidade tem de utilizar a versão 2.1 ou posterior dos [cliente associação à área de trabalho](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Destaques de recursos

- Início de sessão de nome de utilizador pode ser qualquer um do nome de utilizador de predefinição de no local (`userPrincipalName`) ou outro atributo que configurou no Azure AD Connect (`Alternate ID`). Ambos utilizam o trabalho de casos porque utiliza o SSO totalmente integrado a `securityIdentifier` de afirmação na permissão Kerberos para procurar o objeto de utilizador correspondente no Azure AD.
- SSO totalmente integrado é uma funcionalidade oportunista. Se falhar por algum motivo, a experiência de início de sessão do utilizador tem a ver com seu comportamento regular - ou seja, o utilizador tem de introduzir a palavra-passe na página de início de sessão.
- Se um aplicativo (por exemplo, https://myapps.microsoft.com/contoso.com) encaminha uma `domain_hint` (OpenID Connect) ou `whr` parâmetro (SAML) - identificar o seu inquilino, ou `login_hint` parâmetro - a identificação do usuário, no seu Azure AD início de sessão pedido, os usuários são Inicie automaticamente sem eles introduzir nomes de utilizador ou palavras-passe.
- Os utilizadores também obtém uma experiência de início de sessão silenciosa se um aplicativo (por exemplo, https://contoso.sharepoint.com) envia pedidos de início de sessão para pontos finais de inquilinos do Azure AD - ou seja, https://login.microsoftonline.com/contoso.com/<..> ou https://login.microsoftonline.com/<tenant_ID>/<..> - em vez ponto de extremidade comum do Azure AD - ou seja, https://login.microsoftonline.com/common/<...>.
- Fim de sessão é suportada. Isto permite aos utilizadores escolher outra conta do Azure AD para iniciar sessão, em vez de automaticamente a ser iniciado sessão com o SSO totalmente integrado automaticamente.
- Clientes do Office 365 Win32 (Outlook, Word, Excel e outras pessoas) com as versões 16.0.8730.xxxx e superior são suportados com um fluxo não interativo. Para o OneDrive, terá de ativar a [funcionalidade de configuração automática do OneDrive](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) para uma experiência de início de sessão silenciosa.
- Pode ser ativada através do Azure AD Connect.
- É uma funcionalidade gratuita e não é necessário qualquer nas edições pagas do Azure AD para utilizá-lo.
- É suportada em clientes baseados em navegador da web e clientes do Office que suportam [autenticação moderna](https://aka.ms/modernauthga) nas plataformas e browsers com capacidade de autenticação do Kerberos:

| OS\Browser |Internet Explorer|Microsoft Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Sim|Não|Sim|Sim\*|N/A
|Windows 8.1|Sim|N/A|Sim|Sim\*|N/A
|Windows 8|Sim|N/A|Sim|Sim\*|N/A
|Windows 7|Sim|N/A|Sim|Sim\*|N/A
|Mac OS X|N/A|N/A|Sim\*|Sim\*|Sim\*

\*Requer [configuração adicional](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Para Windows 10, a recomendação é usar [associação do Azure AD](../active-directory-azureadjoin-overview.md) para a única início de sessão numa experiência ideal com o Azure AD.

## <a name="next-steps"></a>Passos Seguintes

- [**Início Rápido** ](active-directory-aadconnect-sso-quick-start.md) - colocar em funcionamento o SSO totalmente integrado do Azure AD.
- [**Technical Deep Dive** ](active-directory-aadconnect-sso-how-it-works.md) -compreender como funciona esse recurso.
- [**Perguntas mais frequentes** ](active-directory-aadconnect-sso-faq.md) -respostas para perguntas mais frequentes.
- [**Resolução de problemas** ](active-directory-aadconnect-troubleshoot-sso.md) -Saiba como resolver problemas comuns com a funcionalidade.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – para preenchimento de pedidos de novas funcionalidades.
