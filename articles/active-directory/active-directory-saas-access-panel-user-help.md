---
title: Obter ajuda para aceder e utilizar o portal de aplicações My no Azure Active Directory | Microsoft Docs
description: Obter ajuda com a iniciar sessão e executar tarefas comuns no painel de acesso.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 681831dcc65fa74d8d2e26f58849140498843c49
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="troubleshoot-issues-with-accessing-and-using-the-my-apps-portal"></a>Resolver problemas com a aceder e utilizar o portal de aplicações My

Se estiver a ter problemas com a iniciar sessão ou utilizar o portal de aplicações My, tente estas sugestões de resolução de problemas antes de contactar o suporte técnico ou o seu administrador para obter ajuda.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Estou com dificuldades a iniciar sessão no portal do meu aplicações

Experimente estas sugestões gerais:

- Primeiro, de verificação para ver se estão a utilizar o URL correto, [ https://myapps.microsoft.com ](https://myapps.microsoft.com).
- Tente adicionar o URL para sites fidedignos do browser.
- Certifique-se de que a palavra-passe está correta e não expirou. Para obter mais informações, consulte [repor a palavra-passe de conta escolar ou profissional](active-directory-passwords-update-your-own-password.md).
- Certifique-se certifique-se de que as informações de contacto de autenticação até Data e não a bloquear o acesso. Para obter mais informações, consulte [que Azure multi-factor Authentication significa para mim?](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user).
- Tente limpar os cookies do browser e, em seguida, tente voltar a iniciar sessão.

Se ainda tiver com problemas ao tentar iniciar sessão, contacte o administrador.


## <a name="i-seem-to-be-having-password-issues"></a>Posso parecem estar a ter problemas de palavra-passe

Se esqueceu a palavra-passe, nunca recebeu um da sua equipa de TI, é bloqueado a sua conta ou para alterar a palavra-passe, consulte [ajuda, esqueci minha palavra-passe do Azure AD](active-directory-passwords-update-your-own-password.md).

## <a name="i-need-to-register-for-password-reset"></a>Necessário para se registar para a reposição de palavra-passe

Pode repor a palavra-passe ou desbloquear a conta sem ter de enunciar para alguém através da utilização de reposição de palavra-passe self-service (SSPR). Antes de poder utilizar esta funcionalidade, tem de registar os métodos de autenticação ou confirme os métodos de autenticação predefinido que requer que o administrador. Para obter mais informações, consulte [registar para a reposição de palavra-passe self-service](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Estou com dificuldades em instalar segura de aplicações My início de sessão extensão

O portal de aplicações My necessita de um browser que suporte JavaScript e ativou CSS. Se estiver a utilizar palavra-passe de aplicações baseadas no único início de sessão, a extensão associada tem de estar instalada, bem como. Esta extensão é transferida automaticamente quando iniciar uma aplicação que está configurada para a palavra-passe de aplicações baseadas no único início de sessão.

Verifique para se certificar de que está a cumprir os seguintes requisitos do browser:
- **Limite**: aniversário da edição do Windows 10 ou posterior.
- **Chrome**: no Windows 7 ou posterior e no Mac OS X ou posterior.
- **Firefox 26.0 ou posterior**: no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior.
- **O Internet Explorer 11**: no Windows 7 ou posterior (suporte limitado).

Também pode transferir a extensão para Chrome e limite diretamente a partir de sites seguintes:

- [Extensões do Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Extensão de limite](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Se tiver instalado a extensão e ainda está a ter problemas, experimente o seguinte:

- Verifique as definições de extensão de browser para se certificar de que a extensão está ativada.
- Reinicie o seu browser e inicie sessão no portal as minhas aplicações.
- Limpar os cookies do browser e inicie sessão no portal as minhas aplicações.
- Para aceder a uma ferramenta de diagnóstico e instruções passo a passo sobre como configurar a extensão para o Internet Explorer, consulte [resolver problemas relacionados com a extensão do painel de acesso para o Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

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
> Estas opções estão disponíveis apenas para o limite, o Chrome e o Firefox.

## <a name="how-do-i-add-a-new-app"></a>Como posso adicionar uma nova aplicação?

1.  No **aplicações** página, selecione **Adicionar aplicação**.
2.  Procure a aplicação que pretende adicionar e, em seguida, selecione **adicionar**.

   > [!NOTE]
   > * Pode aceder a esta opção apenas se o administrador o tenha ativado para a sua conta.
   > * Se a aplicação necessita de permissão, poderá ter de aguardar a aprovação do administrador.
   > 

## <a name="how-do-i-manage-my-group-memberships"></a>Como gerir a minha associações?

Selecione o **grupos** mosaico e, em seguida, efetue um dos seguintes procedimentos: 
* Para criar um grupo, em **grupos posso proprietário**, selecione **criar grupo**e, em seguida, siga as instruções.
* Para aderir a um grupo, em **grupos sou no**, selecione **aderir a grupo**e, em seguida, siga as instruções.

   > [!NOTE]
   > * Pode aceder a esta opção apenas se o administrador o tenha ativado para a sua conta.
   > * Se for um membro de um grupo, pode ver os detalhes e deixe o grupo.
   > * Se for um proprietário de um grupo, pode ver os detalhes, adicionar ou remover membros e deixe o grupo.
   >


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações de resolução de problemas, consulte [problemas ao utilizar a aplicação móvel ou um site do painel de acesso de aplicação](active-directory-application-access-panel-content-map.md).

