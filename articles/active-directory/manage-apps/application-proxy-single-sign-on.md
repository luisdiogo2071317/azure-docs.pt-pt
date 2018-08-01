---
title: Gerir o SSO para o Proxy de aplicações do Azure AD | Documentos da Microsoft
description: Saiba mais sobre as noções básicas de início de sessão único com o Proxy de aplicações
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: dbdbe8b83af20f66ad2cc99d2a5665262479b4a9
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364153"
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Como o Proxy de aplicações do Azure AD fornece início de sessão único?

Início de sessão único é um elemento importante do Proxy de aplicações do Azure AD.  Ele fornece a melhor experiência de utilizador, uma vez que os utilizadores apenas têm de iniciar sessão no Azure Active Directory na cloud. Depois de se autenticar-se ao Azure Active Directory, o conector do Proxy de aplicação processa a autenticação para a aplicação no local. A aplicação de back-end não pode informar a diferença entre um usuário remoto, iniciar sessão através do Proxy de aplicações e uma utilização regular num dispositivo associado a um domínio. 

Para utilizar o Azure Active Directory para início de sessão único às suas aplicações, tem de selecionar **do Azure Active Directory** como o método de pré-autenticação. Se selecionou **pass-through** , em seguida, os utilizadores não autenticar para o Azure Active Directory em todas, mas são direcionados diretamente para a aplicação. Pode configurar esta definição quando primeiro publicar uma aplicação, ou navegue até à sua aplicação no portal do Azure e editar as definições de Proxy de aplicações. 

Para ver as opções de início de sessão únicas, siga estes passos:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Navegue para **do Azure Active Directory** > **aplicações empresariais** > **todas as aplicações**.
3. Selecione a aplicação cujas únicas início de sessão nas opções que pretende gerir.
4. Selecione **início de sessão único**.

   ![Menu de lista pendente SSO](./media/application-proxy-single-sign-on/single-sign-on-mode.png)

O menu suspenso mostra cinco opções de início de sessão único para a sua aplicação:

* Início de sessão único do Azure AD desativado
* Com base em palavra-passe de início de sessão
* O início de sessão ligado
* Autenticação Integrada do Windows
* Com base no cabeçalho de início de sessão

## <a name="azure-ad-single-sign-on-disabled"></a>Início de sessão único do Azure AD desativado

Se não pretender utilizar a integração do Azure Active Directory para início de sessão único para a sua aplicação, escolha **do Azure AD início de sessão único desativado**. Com esta opção selecionada, os utilizadores podem autenticar duas vezes. Primeiro, autenticar-se ao Azure Active Directory e, em seguida, inicie sessão no aplicativo propriamente dito. 

Esta opção é uma boa opção se a aplicação no local não necessitar dos utilizadores sejam autenticados, mas que pretende adicionar o Azure Active Directory como uma camada de segurança de acesso remoto. 

## <a name="password-based-sign-on"></a>Com base em palavra-passe de início de sessão

Se pretender utilizar o Azure Active Directory como um cofre de palavras-passe para as suas aplicações no local, escolha **baseado em palavra-passe de início de sessão**. Esta opção é uma boa opção se a aplicação autentica-se com uma combinação de nome de utilizador/palavra-passe em vez de tokens de acesso ou cabeçalhos. Com baseado em palavra-passe de início de sessão, os utilizadores precisam para iniciar sessão, o tempo de aplicativo a primeira acederem ao mesmo. Depois disso, o Azure Active Directory fornece o nome de utilizador e palavra-passe em nome do utilizador. 

Para obter informações sobre como configurar baseado em palavra-passe de início de sessão, consulte [vaulting para início de sessão único com o Proxy da aplicação de palavra-passe](application-proxy-configure-single-sign-on-password-vaulting.md).

## <a name="linked-sign-on"></a>O início de sessão ligado

Se já tiver uma única início de sessão solução definir para as identidades no local, escolha **ligado início de sessão**. Esta opção permite que o Azure Active Directory para tirar partido das soluções existentes do SSO, mas ainda disponibiliza os seus utilizadores acesso remoto para a aplicação. 

Para obter informações sobre ligado início de sessão (formalmente conhecido como existente início de sessão único), consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Autenticação Integrada do Windows

Se as aplicações no local utiliza Authentication(IWA) integrada do Windows ou se pretender utilizar o Kerberos Constrained Delegation (KCD) para início de sessão único, escolha **a autenticação integrada do Windows**. Com esta opção, os utilizadores só tem de autenticar para o Azure Active Directory e, em seguida, o conector do Proxy de aplicações representa o usuário para obter um token de Kerberos e inicie sessão na aplicação. 

Para obter informações sobre como configurar a autenticação integrada do Windows, consulte [delegação restrita de Kerberos para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-with-kcd.md).

## <a name="header-based-sign-on"></a>Com base no cabeçalho de início de sessão 

Se as aplicações utilizam cabeçalhos para autenticação, escolha **com base no cabeçalho de início de sessão**. Com esta opção, os utilizadores só precisam de autenticação do Azure Active Directory. Parceiros da Microsoft com um serviço de autenticação de terceiros chamado PingAccess, que traduzido o token de acesso do Azure Active Directory num formato de cabeçalho para a aplicação. 

Para obter informações sobre como configurar a autenticação baseada em cabeçalho, consulte [autenticação baseada em cabeçalho para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-with-ping-access.md).

## <a name="next-steps"></a>Passos Seguintes

- [Palavra-passe vaulting para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Delegação restrita de Kerberos para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-with-kcd.md)
- [Autenticação baseada em cabeçalho para início de sessão único com o Proxy de aplicações](application-proxy-configure-single-sign-on-with-ping-access.md) 
