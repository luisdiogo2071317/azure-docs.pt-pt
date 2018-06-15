---
title: Authentication do Azure Active Directory baseada em certificado no Android
description: Saiba mais sobre os cenários suportados e os requisitos para configurar a autenticação baseada em certificado em soluções com dispositivos Android
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: annaba
ms.openlocfilehash: ef58fd6c4701f367c6b8664c9cf9ee76e15fbd70
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33880754"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Authentication do Azure Active Directory baseada em certificado no Android

Dispositivos Android podem utilizar autenticação baseada em certificado (CBA) para autenticação no Azure Active Directory utilizando um certificado de cliente nos respetivos dispositivos ao ligar a:

* Aplicações móveis do Office, tais como o Microsoft Outlook e o Microsoft Word
* Clientes do Exchange ActiveSync (EAS)

Configurar esta funcionalidade elimina a necessidade de introduzir uma combinação de nome de utilizador e palavra-passe na determinadas correio e aplicações do Microsoft Office no seu dispositivo móvel.

Este tópico fornece os requisitos e os cenários suportados para a configuração CBA num dispositivo iOS(Android) dos utilizadores de inquilinos no Office 365 Enterprise, empresas, educação, US Government, China e planos de Datacenters.

Esta funcionalidade está disponível na pré-visualização no Office 365 US Government defesa e Federal planos.

## <a name="microsoft-mobile-applications-support"></a>Suporte de aplicações móveis da Microsoft

| Aplicações | Suporte |
| --- | --- |
| Azure Information Protection aplicação |![Marcar][1] |
| Portal da empresa do Intune |![Marcar][1] |
| Microsoft Teams |![Marcar][1] |
| OneNote |![Marcar][1] |
| OneDrive |![Marcar][1] |
| Outlook |![Marcar][1] |
| Power BI |![Marcar][1] |
| Skype para Empresas |![Marcar][1] |
| Word / Excel / PowerPoint |![Marcar][1] |
| Yammer |![Marcar][1] |

### <a name="implementation-requirements"></a>Requisitos de implementação

A versão de SO do dispositivo tem de ser Android 5.0 (Lollipop) e superior.

Tem de ser configurado um servidor de Federação.

Para o Azure Active Directory revogar um certificado de cliente, o token do AD FS tem de ter as seguintes afirmações:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (O número de série do certificado de cliente)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (A cadeia para o emissor do certificado de cliente)

Azure Active Directory adiciona estas afirmações para o token de atualização, se estiverem disponíveis, o token do AD FS (ou qualquer outro token SAML). Quando o token de atualização tem de ser validada, estas informações são utilizadas para verificar a revogação.

Como melhor prática, deve atualizar páginas de erro do AD FS da sua organização com as seguintes informações:

* O requisito para instalar o Microsoft Authenticator no Android.
* Instruções sobre como obter um certificado de utilizador.

Para obter mais informações, consulte [personalizar as páginas de AD FS início de sessão](https://technet.microsoft.com/library/dn280950.aspx).

Algumas aplicações do Office (com a autenticação moderna ativado) enviar '*perguntar = o início de sessão*' para o Azure AD no seu pedido. Por predefinição, o Azure AD traduz '*perguntar = o início de sessão*'no pedido para ADFS como'*wauth = usernamepassworduri*' (pede-lhe ADFS para efetuar a autenticação de U/P) e '*wfresh = 0*' (pede-lhe ADFS para Ignorar o estado SSO e efetue a autenticação de raiz). Se pretender ativar a autenticação baseada em certificado para estas aplicações, terá de modificar o comportamento predefinido do AD do Azure. Defina o '*PromptLoginBehavior*'nas definições do domínio federado para'*desativado*'.
Pode utilizar o [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet para efetuar esta tarefa:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Suporte de clientes do Exchange ActiveSync

São suportadas determinadas aplicações do Exchange ActiveSync no Android 5.0 (Lollipop) ou posterior. Para determinar se a aplicação de correio eletrónico suporta esta funcionalidade, contacte o programador da aplicação.

## <a name="next-steps"></a>Passos Seguintes

Se pretender configurar a autenticação baseada em certificado no seu ambiente, consulte [introdução à autenticação baseada em certificado no Android](active-directory-certificate-based-authentication-get-started.md) para obter instruções.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
