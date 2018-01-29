---
title: "Instale a extensão de browser de painel de acesso de aplicação - Azure | Microsoft Docs"
description: "Corrija os erros comuns encontrados quando instalar a extensão de browser do painel de acesso."
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
ms.openlocfilehash: c49cfad5f362f4402be476066f0e8c0158f20d73
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="install-the-access-panel-browser-extension"></a>Instale a extensão de browser do painel de acesso

O painel de acesso é um portal baseado na web. Se tiver um trabalho ou escola conta no Azure Active Directory (Azure AD), pode utilizar o painel de acesso para ver e iniciar a aplicações baseadas na nuvem que um administrador do Azure AD concedeu que acesso ao. 

Se estiver a utilizar edições do Azure AD, pode também utilizar grupos self-service e capacidades de gestão de aplicações através do painel de acesso. 

O painel de acesso está separado do portal do Azure. Não requer que tenha uma subscrição do Azure.

## <a name="web-browser-requirements"></a>Requisitos de Web browsers

No mínimo, o painel de acesso necessita de um browser que suporte JavaScript e ativou CSS. Para iniciar sessão no aplicações através do SSO baseada em palavra-passe no painel de acesso, tem de ter a extensão do painel de acesso instalada no seu browser. A extensão é transferida automaticamente quando seleciona uma aplicação que está configurada para SSO baseada em palavra-passe.

Para SSO baseada em palavra-passe, pode utilizar qualquer um dos browsers seguintes:

- **Limite**: aniversário da edição do Windows 10 ou posterior. 
- **Chrome**: no Windows 7 ou posterior e no MacOS X ou posterior.
- **Firefox 26.0 ou posterior**: no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior.
- **Internet Explorer 8, 9, 10, 11**: no Windows 7 ou posterior (suporte limitado).

## <a name="install-the-access-panel-browser-extension"></a>Instale a extensão de browser do painel de acesso

Para instalar a extensão de browser do painel de acesso, efetue o seguinte:

1.  Dos browsers suportados, abra o [painel de acesso](https://myapps.microsoft.com)e, em seguida, inicie sessão como um utilizador na sua conta do Azure AD.

2.  Selecione uma aplicação de SSO baseada em palavra-passe.

3.  Quando lhe for pedido, selecione **instalar agora**.  
    O utilizador é direcionado para a hiperligação de transferência para o seu browser selecionado. 
    
4.  Selecione **Adicionar**.

5.  Se lhe for indicado, ou **ativar** ou **permitir** a extensão.

6.  Após a instalação estiver concluída, reinicie o seu browser.

7.  Iniciar sessão para o painel de acesso e verificação para ver se é possível iniciar as aplicações de SSO baseada em palavra-passe.

Também pode transferir a extensão para Chrome e limite diretamente a partir de sites seguintes:

- [Extensões do Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Extensão de limite](https://www.microsoft.com/store/apps/9pc9sckkzk84) 

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Utilize o início de sessão da extensão de proteger as minhas aplicações
* Se estiver a utilizar um URL de aplicações My diferente de `https://myapps.microsoft.com`, configurar o URL predefinido efetuando o seguinte procedimento:
   1. Enquanto estiver *não* sessão para a extensão, faça duplo clique no ícone de extensão.
   2. No menu, selecione **minhas aplicações URL**.
   3. Selecione o seu URL de predefinição.
   4. Selecione o ícone de extensão.
   5. Para iniciar sessão para a extensão, selecione **iniciar sessão começar a utilizar**.

* Para iniciar sessão diretamente a uma aplicação do browser, efetue o seguinte:
   1. Depois de instalar a extensão, inicie sessão no mesmo selecionando **iniciar sessão começar a utilizar**.
   2. Inicie sessão na aplicação com o URL de início de sessão.  
       O URL de início de sessão é, normalmente, o URL da aplicação que apresenta o formulário de início de sessão.
      A extensão deve alterar o estado e informá-lo de que uma palavra-passe está disponível.
   3. Para iniciar sessão, selecione o ícone de extensão.

* Para iniciar uma aplicação a partir da extensão, efetue o seguinte:
   1. Depois de instalar a extensão, inicie sessão no mesmo selecionando **iniciar sessão começar a utilizar**.
   2. Selecione o ícone de extensão para abrir o menu.
   3. Procurar uma aplicação que está disponível no portal do meu aplicações.
   4. Na lista de resultados de pesquisa, selecione a aplicação.  
       São apresentadas as últimas três aplicações que utilizou no **utilizados recentemente** lista de atalho.

> [!NOTE]
> As opções anteriores estão disponíveis apenas para o limite, o Chrome e o Firefox.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Configurar uma política de grupo para o Internet Explorer

Pode configurar uma política de grupo que lhe permite instalar remotamente a extensão do painel de acesso para o Internet Explorer em máquinas dos seus utilizadores.

Antes de configurar uma política de grupo, certifique-se de que:

-   Configurou [serviços de domínio do Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), e associar máquinas dos utilizadores ao seu domínio.

-   Para editar o objeto de política de grupo (GPO), tem de ter *editar definições* permissões. Por predefinição, esta permissão é concedida aos membros dos grupos de segurança seguintes: os administradores do domínio, administradores da empresa e proprietários de criador de política de grupo.

Para instruções passo a passo sobre como configurar a política de grupo e implementar a política para utilizadores, consulte [implementar a extensão do painel de acesso para o Internet Explorer utilizando a política de grupo](active-directory-saas-ie-group-policy.md).

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Resolver problemas relacionados com a extensão do painel de acesso no Internet Explorer

Para aceder a uma ferramenta de diagnóstico e informações sobre como configurar a extensão para o Internet Explorer, consulte [resolver problemas relacionados com a extensão do painel de acesso para o Internet Explorer](active-directory-saas-ie-troubleshooting.md).

> [!NOTE]
> Internet Explorer está no suporte limitado e deixará de receber novas atualizações de software. Limite é o browser recomendado.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Se os passos anteriores não resolverem o problema

Se estiver disponível, abra um pedido de suporte com as seguintes informações:

-   ID de correlação de erro
-   UPN (endereço de e-mail do utilizador)
-   TenantID
-   Tipo de browser
-   Fuso horário e a hora ou o período de tempo quando ocorreu o erro
-   Rastreios de fiddler

## <a name="next-steps"></a>Passos Seguintes
[O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)
