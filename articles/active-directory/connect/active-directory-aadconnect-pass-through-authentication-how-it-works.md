---
title: 'Do Azure AD Connect: A autenticação pass-through - como funciona | Documentos da Microsoft'
description: Este artigo descreve como funciona a autenticação de pass-through do Azure Active Directory
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
ms.date: 01/24/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 4a98b971367c9f83826e85bdc24bbcfe48483f57
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916270"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Do Azure Active Directory autenticação pass-through: Aprofundamento técnico
Este artigo é uma visão geral de como funciona a autenticação pass-through do Azure Active directory (Azure AD). Para técnica e informações de segurança, consulte a [detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) artigo.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Como funciona a autenticação de pass-through do Azure Active Directory?

Quando um utilizador tenta iniciar sessão a uma aplicação protegida pelo Azure AD, e se a autenticação pass-through está ativada no inquilino, ocorrem os seguintes passos:

1. O utilizador tenta aceder a uma aplicação, por exemplo, [Outlook Web App](https://outlook.office365.com/owa/).
2. Se o utilizador não está já iniciou sessão, o utilizador é redirecionado para o Azure AD **sessão do utilizador** página.
3. O utilizador introduz o respetivo nome de utilizador na página de início de sessão do Azure AD e, em seguida, seleciona a **seguinte** botão.
4. O utilizador introduz a palavra-passe na página de início de sessão do Azure AD e, em seguida, seleciona a **iniciar sessão** botão.
5. Azure AD, na receção do pedido para iniciar sessão, coloca o nome de utilizador e palavra-passe (encriptadas utilizando a chave pública dos agentes de autenticação) numa fila.
6. Um agente de autenticação no local obtém o nome de utilizador e palavra-passe encriptada da fila. Tenha em atenção que o agente não consultar frequentemente pedidos da fila, mas obtém pedidos através de uma ligação persistente previamente estabelecida.
7. O agente desencripta a palavra-passe utilizando a respetiva chave privada.
8. O agente valida o nome de utilizador e palavra-passe no Active Directory, utilizando APIs padrão do Windows, que é um mecanismo semelhante para os serviços de Federação do Active Directory (AD FS) utiliza. O nome de utilizador pode ser qualquer um dos locais padrão nome de usuário, geralmente `userPrincipalName`, ou outro atributo que configurou no Azure AD Connect (conhecido como `Alternate ID`).
9. O controlador de domínio do Active Directory no local (DC) avalia a solicitação e retorna a resposta adequada (êxito, falha, palavra-passe expirada ou utilizador bloqueadas) para o agente.
10. O agente de autenticação, por sua vez, retorna essa resposta para o Azure AD.
11. O Azure AD avalia a resposta e responde ao usuário conforme apropriado. Por exemplo, do Azure AD assina o utilizador imediatamente ou pedidos para o Azure multi-factor Authentication.
12. Se o utilizador inicie sessão for bem-sucedida, o utilizador pode aceder à aplicação.

O diagrama seguinte ilustra a todos os componentes e as etapas envolvidas:

![Autenticação pass-through](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Passos Seguintes
- [Limitações atuais](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Saiba quais cenários são suportados e quais não são.
- [Início Rápido](active-directory-aadconnect-pass-through-authentication-quick-start.md): comece a utilizar na autenticação pass-through do Azure AD.
- [Bloqueio do smart](../authentication/howto-password-smart-lockout.md): configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Perguntas mais frequentes](active-directory-aadconnect-pass-through-authentication-faq.md): encontre respostas para perguntas mais frequentes.
- [Resolver problemas de](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): Obtenha informações técnicas detalhadas sobre a funcionalidade de autenticação pass-through.
- [O Azure AD Seamless SSO](active-directory-aadconnect-sso.md): Saiba mais sobre esta funcionalidade complementar.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para pedidos de novas funcionalidades de ficheiros.

