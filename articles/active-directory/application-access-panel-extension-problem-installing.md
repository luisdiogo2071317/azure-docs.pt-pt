---
title: "Extensão de browser de painel de problema a instalar o acesso de aplicação | Microsoft Docs"
description: "Como resolver erros comuns encontrados quando instalar a extensão de browser do painel de acesso"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 26dc5d5ffce84206450123132c0633c2aa323e9f
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/06/2018
---
# <a name="problem-installing-the-application-access-panel-browser-extension"></a>Problema de acesso da aplicação a instalar painel extensão de browser

O painel de acesso é um portal baseado na web que permite que um utilizador que tenha uma conta escolar ou profissional no Azure Active Directory (Azure AD) para ver e iniciar baseado na nuvem as aplicações que o administrador do Azure AD concedeu-lhes acesso. Um utilizador que possua as edições do Azure AD também pode utilizar grupos self-service e capacidades de gestão de aplicações através do painel de acesso. O painel de acesso separado do portal do Azure e não exige que os utilizadores tenham uma subscrição do Azure.

Para utilizar com base em palavra-passe-início de sessão único (SSO) no painel de acesso, a extensão do painel de acesso tem de estar instalada no browser do utilizador. Esta extensão é transferida automaticamente quando um utilizador seleciona uma aplicação que está configurada para SSO baseada em palavra-passe.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Que cumprem os requisitos de browser para o painel de acesso

O painel de acesso necessita de um browser que suporte JavaScript e ativou CSS. Para utilizar com base em palavra-passe-início de sessão único (SSO) no painel de acesso, a extensão do painel de acesso tem de estar instalada no browser do utilizador. Esta extensão é transferida automaticamente quando um utilizador seleciona uma aplicação que está configurada para SSO baseada em palavra-passe.

Para SSO baseada em palavra-passe, os browsers do utilizador final podem ser:

-   Limite de aniversário da edição do Windows 10 ou posterior 

-   Chrome – No Windows 7 ou posterior e no MacOS X ou posterior

-   Firefox 26.0 ou posterior – no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior

-   Internet Explorer 8, 9, 10, 11 - no Windows 7 ou posterior (suporte limitado)
## <a name="how-to-install-the-access-panel-browser-extension"></a>Como instalar a extensão de Browser do painel de acesso

Para instalar a extensão de Browser do painel de acesso, siga os passos abaixo:

1.  Abra o [painel de acesso](https://myapps.microsoft.com) dos browsers suportados e início de sessão como um **utilizador** no seu Azure AD.

2.  Clique num **aplicação de palavra-passe SSO** no painel de acesso.

3.  Na linha de pedir instalar o software, selecione **instalar agora**.

4.  Com base no seu browser, direcionado para a hiperligação de transferência. **Adicionar** a extensão para o seu browser.

5.  Se o browser pede-lhe, selecione como **ativar** ou **permitir** a extensão.

6.  Uma vez instalado, **reiniciar** a sessão do browser.

7.  Inicie sessão no painel de acesso e veja se pode **iniciar** as aplicações de SSO de palavra-passe

Também pode transferir a extensão para Chrome e limite de ligações diretas abaixo:

-   [Extensão de painel de acesso do Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Extensão de painel de acesso de limite](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="how-do-i-use-the-my-apps-secure-sign-in-extension"></a>Como utilizar o Secure de aplicações My início de sessão extensão?
Alterar o URL predefinido do meu aplicações para a extensão

Se estiver a utilizar outro URL aplicações My que https://myapps.microsoft.com, em seguida, tem de configurar o URL predefinido embora os seguintes passos:
1. Enquanto não tiver sessão iniciada para a extensão, **certo** o ícone de extensão.
2. Clique em **URL para selecionar os meus aplicações** no menu.
3. **Selecione** o URL predefinido.
4. Clique no ícone de extensão.
5. Início de sessão para a extensão, selecionando **iniciar sessão começar a utilizar**.

Iniciar sessão diretamente numa aplicação do browser
1. Depois de instalar a extensão, início de sessão para a extensão, selecionando **iniciar sessão começar a utilizar**.
2. Navegue para o **início de sessão URL** da aplicação que pretende iniciar sessão, este é normalmente o URL da aplicação que apresenta o formulário de início de sessão.
3. A extensão deve alterar o estado e permitem-lhe a conhecer uma palavra-passe está disponível, clique em de **ícone de extensão** para iniciar sessão.

Iniciar uma aplicação da extensão do
1. Depois de instalar a extensão, início de sessão para a extensão, selecionando **iniciar sessão começar a utilizar**.
2. Clique no ícone de extensão para abrir o respetivo **menu**.
3. **Pesquisa** para uma aplicação disponível no portal do meu aplicações.
4. Clique na aplicação do **resultados da pesquisa** para o iniciar.
5. As últimas três aplicações iniciadas também irão aparecer no **utilizados recentemente** lista de atalho

> [!NOTE]
> Estas opções só estão disponíveis para o limite, o Chrome, o Firefox.

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>Configurar uma política de grupo para o Internet Explorer

Pode configurar uma política de grupo que lhe permitem instalar remotamente a extensão do painel de acesso para o Internet Explorer em máquinas dos seus utilizadores.

Os pré-requisitos incluem:

-   Configurou [serviços de domínio do Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), e associar máquinas dos utilizadores ao seu domínio.

-   Tem de ter a permissão "Definições de editar" para editar o objeto de política de grupo (GPO). Por predefinição, os membros dos grupos de segurança seguinte têm esta permissão: os administradores do domínio, administradores da empresa e proprietários de criador de política de grupo. [Saiba mais](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Siga o tutorial [como implementar a extensão do painel de acesso para o Internet Explorer utilizando a política de grupo](active-directory-saas-ie-group-policy.md) para instruções passo a passo sobre como configurar a política de grupo e implementá-la aos utilizadores.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Resolver problemas relacionados com a extensão do painel de acesso no Internet Explorer

Siga o [resolver problemas relacionados com a extensão do painel de acesso para o Internet Explorer](active-directory-saas-ie-troubleshooting.md) guia para acesso de uma ferramenta de diagnóstico e instruções passo a passo sobre como configurar a extensão de i/e.

> [!NOTE]
> I/e está no suporte limitado e deixará de receber novas atualizações de software. Limite é o browser recomendado.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Se estes passos de resolução de problemas não resolverem o problema

Abra um pedido de suporte com as seguintes informações se disponíveis:

-   ID de correlação de erro

-   UPN (endereço de e-mail do utilizador)

-   TenantID

-   Tipo de browser

-   Fuso horário e tempo/período de tempo durante o erro ocorre

-   Rastreios de fiddler

## <a name="next-steps"></a>Passos Seguintes
[O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)
