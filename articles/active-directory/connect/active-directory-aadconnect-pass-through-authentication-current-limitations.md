---
title: 'Azure AD Connect: Autenticação pass-through-limitações atuais | Documentos da Microsoft'
description: Este artigo descreve as limitações atuais da autenticação de pass-through do Azure Active Directory (Azure AD)
services: active-directory
keywords: Autenticação do Azure AD Connect pass-through, Active Directory, de instalação necessários componentes para o Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 34b83c54e31ed73af3f776a6add8f218dda35cf7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918925"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory autenticação pass-through: Limitações atuais

>[!IMPORTANT]
>Autenticação de pass-through do Azure Active Directory (Azure AD) é uma funcionalidade gratuita e não é necessário qualquer nas edições pagas do Azure AD para utilizá-lo. Autenticação pass-through só está disponível na instância a nível mundial do Azure AD e não no [cloud do Microsoft Azure Alemanha](http://www.microsoft.de/cloud-deutschland) ou o [cloud do Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Cenários suportados

Os cenários seguintes são totalmente suportados:

- Utilizador inícios de sessão para todos os aplicativos baseados em navegador da web.
- Inícios de utilizador para aplicações do Office que suportam [autenticação moderna](https://aka.ms/modernauthga): Office 2016 e Office 2013 _com_ autenticação moderna.
- Utilizador inícios de sessão para os clientes Outlook usando protocolos legados, tais como o Exchange ActiveSync, SMTP, POP e IMAP.
- Início de sessão de utilizador-ins ao Skype para empresas que suportam a autenticação moderna, incluindo online e topologias híbridas. Saiba mais sobre topologias suportadas [aqui](https://technet.microsoft.com/library/mt803262.aspx).
- Domínio do Azure AD é associado para dispositivos Windows 10.
- Palavras-passe de aplicação para o multi-factor Authentication.

## <a name="unsupported-scenarios"></a>Cenários não suportados

Os cenários seguintes são _não_ suportados:

- Inícios de usuário para aplicativos de cliente do Office herdados, excluindo o Outlook (veja **cenários suportados** acima): Office 2010 e o Office 2013 _sem_ autenticação moderna. As organizações são aconselhadas a mudar para a autenticação moderna, se possível. Suporte de autenticação pass-through permite a autenticação moderna. Ele também ajuda a proteger as suas contas de utilizador utilizando [acesso condicional](../active-directory-conditional-access-azure-portal.md) funcionalidades, como o Azure multi-factor Authentication.
- Acesso a partilha de calendário e informações de disponibilidade no Exchange ambientes híbridos no Office 2010 apenas.
- Utilizador inícios de sessão ao Skype para aplicativos de cliente de negócios _sem_ autenticação moderna.
- Utilizador inícios de sessão para o PowerShell versão 1.0. Recomendamos que utilize o PowerShell versão 2.0.
- Deteção de utilizadores com [fuga de credenciais](../active-directory-reporting-risk-events.md#leaked-credentials).
- O Azure AD Domain Services precisa de sincronização de Hash de palavra-passe para ser ativada no inquilino. Portanto, os inquilinos que utilizam a autenticação pass-through _apenas_ não funcionam para cenários que precisam de serviços de domínio do Azure AD.
- Autenticação pass-through não é integrada com [do Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- A Apple registo de aparelho (DEP da Apple) com o Assistente de configuração de iOS não suporta a autenticação moderna. Isto irá falhar inscrever dispositivos DEP da Apple no Intune para domínios geridos através da autenticação pass-through. Considere utilizar o [aplicação Portal da empresa](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) como alternativa.

>[!IMPORTANT]
>Como solução para cenários não suportados _apenas_, ativar a sincronização de Hash de palavra-passe no [funcionalidades opcionais](active-directory-aadconnect-get-started-custom.md#optional-features) página no Assistente do Azure AD Connect. Quando os utilizadores iniciarem sessão nas aplicações listado no "não suportado cenários" seção, esses pedidos de início de sessão específicos são _não_ processado pelos agentes de autenticação pass-through e, portanto, não irá ser registradas no [ Registos de autenticação pass-through](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Ativar a sincronização de hash de palavra-passe dá-lhe a opção de autenticação de ativação pós-falha se a sua infraestrutura no local é interrompida. Esta ativação pós-falha da autenticação pass-through para a sincronização de hash de palavra-passe do Active Directory não é automática. Terá de mudar o método de início de sessão manualmente utilizando o Azure AD Connect. Se o servidor com o Azure AD Connect ficar inativo, é necessária a ajuda de Support da Microsoft para desativar a autenticação pass-through.

## <a name="next-steps"></a>Passos Seguintes
- [Guia de introdução](active-directory-aadconnect-pass-through-authentication-quick-start.md): começar a trabalhar com a autenticação pass-through do Azure AD.
- [Bloqueio do smart](../authentication/howto-password-smart-lockout.md): Saiba como configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Análise técnica aprofundada](active-directory-aadconnect-pass-through-authentication-how-it-works.md): compreender como funciona a funcionalidade de autenticação pass-through.
- [Perguntas mais frequentes sobre](active-directory-aadconnect-pass-through-authentication-faq.md): encontre respostas para perguntas mais frequentes sobre a funcionalidade de autenticação pass-through.
- [Resolver problemas de](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): Obtenha informações técnicas detalhadas sobre a funcionalidade de autenticação pass-through.
- [O Azure AD Seamless SSO](active-directory-aadconnect-sso.md): Saiba mais sobre esta funcionalidade complementar.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para pedidos de novas funcionalidades de ficheiros.
