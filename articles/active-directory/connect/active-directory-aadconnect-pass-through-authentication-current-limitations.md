---
title: 'Do Azure AD Connect: A autenticação pass-through - limitações atuais | Microsoft Docs'
description: Este artigo descreve as limitações atuais da autenticação do Azure Active Directory (Azure AD) pass-through
services: active-directory
keywords: Authentication do Azure AD Connect pass-through, a instalação do Active Directory, os componentes necessários para o Azure AD, SSO, o início de sessão único
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: billmath
ms.openlocfilehash: 680e9967010771b8e3651c6f4eed81237f8fb4c3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Do Azure autenticação do Active Directory pass-through: Limitações atuais

>[!IMPORTANT]
>Autenticação de pass-through do Azure Active Directory (Azure AD) é uma funcionalidade livre e não precisa de quaisquer edições pagas do Azure AD para utilizá-lo. Autenticação pass-through só está disponível na instância em todo o mundo do Azure AD e não no [cloud do Microsoft Azure Datacenters](http://www.microsoft.de/cloud-deutschland) ou [cloud do Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Cenários suportados

Os cenários seguintes são totalmente suportados:

- Utilizador inícios de sessão para todas as aplicações baseadas no browser do web.
- Utilizador inícios de sessão para aplicações do Office que suportam [autenticação moderna](https://aka.ms/modernauthga): Office 2016 ou Office 2013 _com_ autenticação moderna.
- Utilizador inícios de sessão para clientes do Outlook utilizar protocolos de legado, como o Exchange ActiveSync, SMTP, POP e IMAP.
- Utilizador inícios de sessão ao Skype para empresas que suportam a autenticação moderna, incluindo online e topologias híbridas. Obter mais informações sobre topologias suportadas [aqui](https://technet.microsoft.com/library/mt803262.aspx).
- Domínio do Azure AD junta para dispositivos Windows 10.
- Palavras-passe de aplicação para o multi-factor Authentication.

## <a name="unsupported-scenarios"></a>Cenários não suportados

Os cenários seguintes são _não_ suportados:

- Utilizador inícios de sessão para aplicações de cliente do Office legadas, excluindo o Outlook (consulte **suportado cenários** acima): Office 2010 e Office 2013 _sem_ autenticação moderna. As organizações são encouraged para mudar para a autenticação moderna, se possível. Permite a autenticação moderna para o suporte de autenticação pass-through. Também o ajuda a proteger as contas de utilizador utilizando [acesso condicional](../active-directory-conditional-access-azure-portal.md) funcionalidades, tais como o Azure multi-factor Authentication.
- Acesso a partilha de calendário e libertar/ocupadas informações no Exchange ambientes híbridos no Office 2010 apenas.
- Utilizador inícios de sessão ao Skype para as aplicações de cliente de negócio _sem_ autenticação moderna.
- Utilizador inícios de sessão para o PowerShell na versão 1.0. Recomendamos que utilize o PowerShell versão 2.0.
- Deteção de utilizadores com [fuga credenciais](../active-directory-reporting-risk-events.md#leaked-credentials).
- Serviços de domínio do Azure AD tem de sincronização de Hash de palavra-passe para ser ativada no inquilino. Por conseguinte, os inquilinos que utilizam a autenticação pass-through _apenas_ não funcionam para cenários que precisam de serviços de domínio do Azure AD.
- A autenticação pass-through não está integrada [do Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- A Apple Device Enrollment Program (DEP da Apple) utilizando o Assistente de configuração de iOS não suporta a autenticação moderna. Isto irá falhar inscrever dispositivos de DEP da Apple no Intune para domínios geridos através da autenticação pass-through. Considere utilizar o [aplicação Portal da empresa](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) como alternativa.

>[!IMPORTANT]
>Como uma solução para cenários não suportados _apenas_, ativar a sincronização de Hash de palavra-passe no [funcionalidades opcionais](active-directory-aadconnect-get-started-custom.md#optional-features) página do Assistente do Azure AD Connect. Quando a sessão de utilizadores em aplicações listado no "não suportado cenários" secção, os pedidos de início de sessão específicos são _não_ processado por agentes de autenticação pass-through e, por conseguinte, não irá ser registada no [ Os registos de autenticação pass-through](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Ativar a sincronização de hash de palavra-passe dá-lhe a opção de autenticação de ativação pós-falha se a sua infraestrutura no local é interrompida. Esta ativação pós-falha de autenticação pass-through para a sincronização de hash de palavra-passe do Active Directory não é automática. Terá de mudar o método de início de sessão manualmente utilizando o Azure AD Connect. Se o servidor que executa o Azure AD Connect ficar inativo, irá necessitar de ajuda do Support da Microsoft para desativar a autenticação pass-through.

## <a name="next-steps"></a>Passos Seguintes
- [Início Rápido](active-directory-aadconnect-pass-through-authentication-quick-start.md): começar a trabalhar com a autenticação pass-through do Azure AD.
- [Bloqueio do smart](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): Saiba como configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Descrição detalhada da Technical](active-directory-aadconnect-pass-through-authentication-how-it-works.md): compreender como funciona a funcionalidade de autenticação pass-through.
- [Perguntas mais frequentes](active-directory-aadconnect-pass-through-authentication-faq.md): encontre respostas a perguntas mais frequentes sobre a funcionalidade de autenticação pass-through.
- [Resolver problemas](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Descrição detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): obter particularmente técnicas informações sobre a funcionalidade de autenticação pass-through.
- [Azure SSO totalmente integrada de AD](active-directory-aadconnect-sso.md): saber mais sobre esta funcionalidade complementares.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para novos pedidos de funcionalidade de ficheiros.
