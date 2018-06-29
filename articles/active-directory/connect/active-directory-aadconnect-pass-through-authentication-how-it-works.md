---
title: 'Do Azure AD Connect: A autenticação pass-through - como funciona | Microsoft Docs'
description: Este artigo descreve como funciona a autenticação pass-through do Active Directory do Azure
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
ms.date: 01/24/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 5fde0ea00aacbb791836fc1076b88dafd3728454
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063481"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Authentication do Azure Active Directory pass-through: Descrição profunda técnica
Este artigo é uma descrição geral sobre como funciona o Azure Active directory (Azure AD) autenticação pass-through. Para avançada técnica e informações de segurança, consulte o [detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) artigo.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Como funciona a autenticação pass-through do Active Directory do Azure?

Quando um utilizador tenta iniciar sessão a uma aplicação protegida pelo Azure AD e se a autenticação pass-through estiver ativada no inquilino, ocorrem os seguintes passos:

1. O utilizador tenta aceder a uma aplicação, por exemplo, [Outlook Web App](https://outlook.office365.com/owa/).
2. Se o utilizador não está já iniciou sessão, o utilizador é redirecionado para o Azure AD **sessão do utilizador** página.
3. O utilizador introduz o nome de utilizador para o página de início de sessão do Azure AD e, em seguida, seleciona o **seguinte** botão.
4. O utilizador introduz a palavra-passe para o página de início de sessão do Azure AD e, em seguida, seleciona o **sessão** botão.
5. Azure AD, no receber o pedido para iniciar sessão, coloca o nome de utilizador e palavra-passe (encriptadas utilizando a chave pública dos agentes de autenticação) numa fila.
6. Um agente de autenticação no local obtém o nome de utilizador e palavra-passe encriptada da fila. Tenha em atenção que o agente não frequentemente consulta pedidos da fila, mas obtém pedidos através de uma ligação persistente previamente estabelecida.
7. O agente desencripta a palavra-passe utilizando a respetiva chave privada.
8. O agente valida o nome de utilizador e palavra-passe no Active Directory, utilizando APIs padrão do Windows, que é um mecanismo semelhante para os serviços de Federação do Active Directory (AD FS) utiliza. O nome de utilizador pode ser qualquer um do local no nome de utilizador predefinido, normalmente, `userPrincipalName`, ou outro atributo configurado no Azure AD Connect (conhecido como `Alternate ID`).
9. O controlador de domínio do Active Directory no local (DC) avalia o pedido e devolve a resposta adequada (êxito, falha, palavra-passe expirou ou utilizador bloqueadas) para o agente.
10. O agente de autenticação, por sua vez, devolve esta resposta para o Azure AD.
11. Azure AD avalia a resposta e responde ao utilizador conforme apropriado. Por exemplo, do Azure AD inicia imediatamente o utilizador sessão ou pedidos de Azure multi-factor Authentication.
12. Se o utilizador inicie sessão com êxito, o utilizador pode aceder à aplicação.

O diagrama seguinte ilustra a todos os componentes e os passos envolvidos:

![Autenticação pass-through](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Passos Seguintes
- [Limitações atuais](active-directory-aadconnect-pass-through-authentication-current-limitations.md): saber que cenários são suportados e aqueles que não são.
- [Início Rápido](active-directory-aadconnect-pass-through-authentication-quick-start.md): começar a trabalhar na autenticação pass-through do Azure AD.
- [Bloqueio do smart](../authentication/howto-password-smart-lockout.md): configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Perguntas mais frequentes](active-directory-aadconnect-pass-through-authentication-faq.md): encontre respostas a perguntas mais frequentes.
- [Resolver problemas](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Descrição detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): obter particularmente técnicas informações sobre a funcionalidade de autenticação pass-through.
- [Azure SSO totalmente integrada de AD](active-directory-aadconnect-sso.md): saber mais sobre esta funcionalidade complementares.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para novos pedidos de funcionalidade de ficheiros.

