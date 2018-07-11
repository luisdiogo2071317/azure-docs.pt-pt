---
title: 'Do Azure AD Connect: Autenticação de pass-through | Documentos da Microsoft'
description: Este artigo descreve a autenticação de pass-through do Azure Active Directory (Azure AD) e como ele permite inícios de sessão do Azure AD ao validar as senhas dos usuários contra do Active Directory no local.
services: active-directory
keywords: o que é o Azure AD Connect pass-through Authentication, instalar o Active Directory, os componentes necessários para o Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2d88bf5d20beb9de9bf4a0cdcb43548d0d582779
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917283"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Utilizador inicie sessão com a autenticação de pass-through do Azure Active Directory

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>O que é a autenticação de pass-through do Azure Active Directory?

Autenticação de pass-through do Azure Active Directory (Azure AD) permite aos utilizadores iniciar sessão no local e aplicações baseadas na nuvem usando as mesmas palavras-passe. Esta funcionalidade fornece aos utilizadores uma experiência melhor - um menos palavra-passe para se lembrar e reduz os custos de suporte técnico de TI, uma vez que os usuários têm menos probabilidade de esquecer como iniciar sessão. Quando os utilizadores iniciam sessão com o Azure AD, esta funcionalidade valida senhas diretamente no seu Active Directory no local.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Esta funcionalidade é uma alternativa à [sincronização de Hash de palavra-passe do Azure AD](active-directory-aadconnectsync-implement-password-hash-synchronization.md), que fornece o benefício de autenticação na nuvem para as organizações. No entanto, as políticas de segurança e conformidade em certas organizações não permitem essas organizações para enviar as senhas dos usuários, mesmo num formulário com hash, fora os limites internos. Autenticação pass-through é a solução certa para organizações.

![Autenticação do Azure AD pass-through](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Pode combinar a autenticação pass-through com o [totalmente integrada Single Sign-On](active-directory-aadconnect-sso.md) funcionalidade. Dessa forma, quando os utilizadores estão a aceder a aplicações nos seus computadores empresariais no interior da rede empresarial, não precisam de digitar as palavras-passe para iniciar sessão.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Principais vantagens de utilizar a autenticação pass-through do Azure AD

- *Grande experiência de usuário*
  - Os utilizadores utilizam as mesmas palavras-passe para iniciar sessão no local e aplicações baseadas na nuvem.
  - Os utilizadores gastar menos tempo conversando com os problemas relacionados com a palavra-passe com o IT suporte técnico resolução.
  - Os utilizadores podem concluir [gestão de palavras-passe self-service](../authentication/active-directory-passwords-overview.md) tarefas na cloud.
- *Fácil de implementar e administrar*
  - Não é necessário para implementações no local complexas ou configuração de rede.
  - Tem apenas um agente de simples para ser instalado no local.
  - Não existem custos de gestão. O agente recebe automaticamente melhorias e correções de erros.
- *Proteger*
  - Palavras-passe no local nunca são armazenadas na cloud de nenhuma forma.
  - O agente só faz ligações de saída a partir de dentro da sua rede. Por conseguinte, não existe nenhum requisito para instalar o agente numa rede de perímetro, também conhecido como uma rede de Perímetro.
  - Protege as contas de utilizador ao trabalhar de forma totalmente integrada com [políticas de acesso condicional do Azure AD](../active-directory-conditional-access-azure-portal.md), incluindo multi-factor Authentication (MFA) e pelo [filtrar ataques de palavra-passe de força bruta](../authentication/howto-password-smart-lockout.md).
- *Elevada disponibilidade*
  - Agentes adicionais podem ser instalados em vários servidores no local para proporcionar elevada disponibilidade de pedidos de início de sessão.

## <a name="feature-highlights"></a>Destaques de recursos

- Oferece suporte a sessão do utilizador para todos os aplicativos baseados em navegador da web e em aplicativos de cliente do Microsoft Office que usam [autenticação moderna](https://aka.ms/modernauthga).
- Início de sessão de nomes de utilizador podem ser qualquer um do nome de utilizador de predefinição de no local (`userPrincipalName`) ou outro atributo que configurou no Azure AD Connect (conhecido como `Alternate ID`).
- O recurso funciona perfeitamente com o [acesso condicional](../active-directory-conditional-access-azure-portal.md) funcionalidades, como o multi-factor Authentication (MFA) para ajudar a proteger os seus utilizadores.
- Integrado com base na cloud [gestão de palavras-passe self-service](../authentication/active-directory-passwords-overview.md), incluindo a repetição de escrita de palavra-passe para locais do Active Directory e a proteção de palavra-passe ao banir palavras-passe utilizadas frequentemente.
- Ambientes de várias florestas são suportadas se existem relações de confiança de floresta entre florestas do AD e, se o encaminhamento de sufixo de nome está configurado corretamente.
- É uma funcionalidade gratuita e não é necessário qualquer nas edições pagas do Azure AD para utilizá-lo.
- Pode ser ativada através de [do Azure AD Connect](active-directory-aadconnect.md).
- Ele usa um agente de simples no local que escuta e responde a pedidos de validação da palavra-passe.
- Instalar agentes de vários fornece elevada disponibilidade de pedidos de início de sessão.
- Ele [protege](../authentication/howto-password-smart-lockout.md) suas contas no local relativamente bruta forçar ataques de palavra-passe na cloud.

## <a name="next-steps"></a>Passos Seguintes

- [**Início Rápido** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - colocar em funcionamento autenticação pass-through do Azure AD.
- [**Bloqueio do smart** ](../authentication/howto-password-smart-lockout.md) -capacidade de configurar o bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [**Limitações atuais** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -Saiba quais cenários são suportados e quais não são.
- [**Technical Deep Dive** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -compreender como funciona esse recurso.
- [**Perguntas mais frequentes** ](active-directory-aadconnect-pass-through-authentication-faq.md) -respostas para perguntas mais frequentes.
- [**Resolução de problemas** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -Saiba como resolver problemas comuns com a funcionalidade.
- [**Segurança Deep Dive** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -informações técnicas detalhadas adicionais sobre a funcionalidade.
- [**O Azure AD Seamless SSO** ](active-directory-aadconnect-sso.md) -Saiba mais sobre esta funcionalidade complementar.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – para preenchimento de pedidos de novas funcionalidades.
