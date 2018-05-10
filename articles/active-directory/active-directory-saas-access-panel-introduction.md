---
title: O que é o painel de acesso no Azure Active Directory? | Microsoft Docs
description: Saiba como utilizar variações do painel de acesso (web browser, aplicação Android, aplicação iPhone e iPad) para aceder a aplicações SaaS.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/18
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 442bfa7081865b2549c07a9436296ba2385a0b66
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="what-is-the-access-panel"></a>O que é o painel de acesso?

O painel de acesso é um portal baseado na web. Se tiver um trabalho ou escola conta no Azure Active Directory (Azure AD), pode utilizar o painel de acesso para ver e iniciar a aplicações baseadas na nuvem que um administrador do Azure AD concedeu que acesso ao. Também pode utilizar grupos self-service e capacidades de gestão de aplicações através do painel de acesso.

O painel de acesso está separado do portal do Azure. Não requer que tenha uma subscrição do Azure.

![Painel de acesso][1] utilizando o painel de acesso, pode editar algumas das suas definições de perfil e efetue o seguinte:

- Altere a palavra-passe associada a uma conta escolar ou profissional.

- Edite definições de reposição de palavra-passe.

- Edite definições de contacto e preferências relacionadas com a autenticação multifator (para contas que foram necessário utilizá-lo por um administrador).

- Ver a conta alternativa de detalhes, tais como o ID de utilizador, dispositivos, móveis e office números de telefone e e-mail.

- Ver e iniciar a aplicações baseadas na nuvem que o administrador do Azure AD concedeu que acesso ao. 

- Self-gerir grupos. Os administradores podem criar e gerir grupos de segurança e as associações de grupo de segurança de pedido no Azure AD. Para obter mais informações, consulte [gestão de grupos self-service para utilizadores no Azure AD](active-directory-accessmanagement-self-service-group-management.md) e [gerir os grupos](active-directory-manage-groups.md).




## <a name="access-the-access-panel"></a>Aceder ao painel de acesso

Pode aceder ao painel de acesso, acedendo a `http://myapps.microsoft.com`.

Se tiver personalizado uma imagem corporativa configurado para a sua página de início de sessão, pode carregar a imagem corporativa, acrescentando o domínio da sua organização para o URL (por exemplo, `http://myapps.microsoft.com/<your domain>.com`).

Pode utilizar qualquer nome de domínio do Active Directory ou verificado que tenha sido configurado no portal do Azure, conforme mostrado aqui: ![nome de domínio no diretório Wingtip Toys][2]  

Distribua o URL para todos os utilizadores que iniciam sessão aplicações que estão integradas com o Azure AD.

## <a name="authentication"></a>Autenticação

Para alcançar o painel de acesso, tem de ser autenticado através de uma conta escolar ou profissional no Azure AD. Pode ser autenticado para o Azure AD diretamente. Em alternativa, se uma organização tiver configurado a Federação ao utilizar serviços de Federação do Active Directory (AD FS) ou outras tecnologias, pode ser autenticada pelo Windows Server Active Directory.

Se tiver uma subscrição do Azure ou do Office 365 e utiliza o portal do Azure ou uma aplicação do Office 365, pode ver a lista de aplicações sem iniciar sessão novamente. Se não são autenticados, lhe for pedido que inicie sessão com o nome de utilizador e palavra-passe para a sua conta no Azure AD. Se a sua organização tiver configurado a Federação, escrever o nome de utilizador é suficiente.

Quando são autenticados, pode interagir com as aplicações que o administrador tem integrado com o diretório. Para saber como integrar aplicações com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Requisitos de Web browsers

No mínimo, o painel de acesso necessita de um browser que suporte JavaScript e ativou CSS. Para iniciar sessão no aplicações através do baseada em palavra-passe-início de sessão único (SSO), tem de ter a extensão do painel de acesso instalada no seu browser. A extensão é transferida automaticamente quando seleciona uma aplicação que está configurada para SSO baseada em palavra-passe.

O instalador é específica da arquitetura. Se clicar na hiperligação de transferência, apenas obter o instalador para a arquitetura de SO que estão atualmente em execução no. Se for um administrador de implementação de aplicação, certifique-se de que visite a ligação de transferência de um dispositivo de 64 bits e de 32 bits para obter ambos os programas de instalação.


A extensão do painel de acesso está atualmente disponível para:
- **Limite**: aniversário da edição do Windows 10 ou posterior. 
- **Chrome**: no Windows 7 ou posterior e no MacOS X ou posterior.
- **Firefox 26.0 ou posterior**: no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior.
- **O Internet Explorer 11**: no Windows 7 ou posterior (suporte limitado).

## <a name="my-apps-secure-sign-in-extension"></a>Extensão de Início de Sessão Seguro das Minhas Aplicações
Para iniciar sessão baseada em palavra-passe de início de sessão, tem de utilizar a extensão. Depois da extensão é instalada, pode iniciar sessão-la para ativar funcionalidades adicionais ao selecionar **iniciar sessão começar a utilizar**. 

- Pode iniciar sessão numa aplicação diretamente utilizando a aplicação **URL de início de sessão**. Quando utiliza o URL da aplicação, a extensão Deteta a ação e dá-lhe a opção de iniciar sessão a partir de extensão.
- Pode iniciar qualquer das suas aplicações a partir do painel de acesso utilizando o *pesquisa rápida* funcionalidade da extensão. 
- A extensão mostra as últimas três aplicações que foi iniciado no **utilizados recentemente** secção.
- Pode utilizar URLs internas da empresa ao remoto através de [Proxy de aplicações](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

> [!NOTE]
> Funcionalidades adicionais estão disponíveis apenas para o limite, o Chrome e o Firefox.
>

Se estiver a utilizar um URL de aplicações My diferente de `https://myapps.microsoft.com`, configurar o URL predefinido efetuando o seguinte procedimento:
1. Enquanto estiver *não* sessão para a extensão, faça duplo clique no ícone de extensão.
2. No menu, selecione **minhas aplicações URL**.
3. Selecione o seu URL de predefinição.
4. Selecione o ícone de extensão.
5. Selecione **iniciar sessão começar a utilizar**.

## <a name="mobile-app-support"></a>Suporte de aplicações móveis

A equipa do Azure Active Directory publica a aplicação móvel da minha aplicações. Quando instalar a aplicação, pode iniciar sessão aplicações de SSO baseada em palavra-passe em dispositivos iOS e Android.

> [!NOTE]
> Pode iniciar sessão aplicações que suportam a Federação com o Azure AD (incluindo o Salesforce, Google Apps, Dropbox, caixa, Concur, Workday, Office 365 e 70 mais do que outras pessoas) em praticamente qualquer browser web, em qualquer dispositivo, sem necessitar de uma aplicação de plug-in ou móvel. Para ser utilizado num dispositivo móvel, outras [experiências do painel de acesso](https://myapps.microsoft.com/) também não requerem a aplicação móvel da minha aplicações.
>
>

### <a name="my-apps-for-android"></a>Minhas aplicações para Android

As minhas aplicações para Android é suportada em todos os dispositivos Android que está a executar o Android versão 4.1 ou posterior.  

Está disponível na [loja do Google Play](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Minhas aplicações para Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Minhas aplicações para iPhone e iPad

As minhas aplicações para iOS é suportada em qualquer iPhone ou iPad, que está a executar o iOS versão 7 ou posterior.  

Está disponível na [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Minhas aplicações para iOS][4]    


## <a name="managed-browser-for-my-apps"></a>Browser gerido para os meus aplicações

As minhas aplicações também está integrado com o Intune Managed Browser. O Browser gerido do Intune para dispositivos iOS e Android desempenha uma função chave ajudar a garantir que os dados em dispositivos móveis permanecem segurados. O browser permite-lhe possam ver e navegue páginas Web que poderão conter informações da empresa e ajuda a proporcionar uma experiência de navegação na web segura.  

Obtém acesso rápido às minhas aplicações na sua home page do Browser gerido e na sua marcadores, pelo menos cliques são necessárias para aceder a qualquer aplicação que pretende aceder.

Browser gerido do Intune está disponível na [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) e [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Browser gerido para os meus aplicações][5]    


## <a name="tips-for-testing-the-user-experience"></a>Sugestões para a experiência de utilizador de teste

Se for um administrador do Azure e tem sessão iniciada portal do Azure, utilizando uma conta no diretório, que é automaticamente sessão painel de acesso como a sua conta atual. Esta vista apresenta todas as aplicações que são atribuídas ao utilizador.

Para testar um *diferentes* utilizador da conta, efetue o seguinte:

1. No canto superior direito do portal do Azure ou o painel de acesso, selecione **terminar sessão**. 
2. Vá para o [painel de acesso](http://myapps.microsoft.com).
3. Na página de início de sessão, escreva o nome de utilizador e palavra-passe para a conta no seu diretório que pretende testar.


## <a name="starting-applications"></a>Início de aplicações

Esta secção descreve vários tipos de aplicações que podem ser apresentado no painel de acesso.

### <a name="office-365-applications"></a>Aplicações do Office 365

Se a sua organização está a utilizar aplicações do Office 365 e são licenciados para os mesmos, as aplicações do Office 365 são apresentados no painel de acesso.

Quando seleciona um mosaico de aplicação para uma aplicação do Office 365, são redirecionados para a aplicação e inicie automaticamente.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Aplicações de terceiros da Microsoft e configuradas com o SSO baseada em Federação

O administrador pode adicionar aplicações na secção do portal do Azure Active Directory com o modo SSO definido como **do Azure AD Single Sign-On**. Pode ver estas aplicações apenas se o administrador concedeu explicitamente que aceder aos mesmos.

Quando seleciona um mosaico para uma aplicação, são redirecionados e inicie automaticamente ao mesmo.

### <a name="password-based-sso-without-identity-provisioning"></a>SSO baseada em palavra-passe sem aprovisionamento de identidade

O administrador pode adicionar aplicações na secção do portal do Azure Active Directory com o modo SSO definido como **baseada em palavra-passe Single Sign-On**. Todos os utilizadores no diretório podem ver todas as aplicações que tenham sido configuradas neste modo.

Na primeira vez, que selecione um mosaico de aplicação, lhe for pedido para instalar o plug-in de SSO de palavra-passe para o Internet Explorer ou o Chrome. A instalação poderá requerer que reinicie o seu browser. Quando regressar ao painel de acesso e selecione o mosaico da aplicação novamente, lhe for pedido um nome de utilizador e palavra-passe para a aplicação. Quando introduzir o nome de utilizador e palavra-passe, as credenciais são armazenadas em segurança e ligadas à sua conta no Azure AD.

Da próxima vez que selecione o mosaico da aplicação, que é automaticamente iniciou sessão na aplicação.  

Não tem de introduzir as suas credenciais novamente e ou instalar o plug-in de SSO de palavra-passe.

Se tem alterado as suas credenciais na aplicação de terceiros de destino, tem também de atualizar as suas credenciais são armazenadas no Azure AD. 

Para atualizar as suas credenciais, efetue o seguinte:

1. Selecione o ícone no mosaico aplicações.
2. Selecione **atualizar credenciais** reintroduzir o nome de utilizador e palavra-passe para a aplicação.


### <a name="password-based-sso-with-identity-provisioning"></a>SSO baseada em palavra-passe com o aprovisionamento de identidade

O administrador pode adicionar aplicações na secção do portal do Azure Active Directory com o modo SSO definido como **baseada em palavra-passe Single Sign-On**, juntamente com o aprovisionamento de identidade.

Na primeira vez, que selecione um mosaico de aplicação, lhe for pedido para instalar o plug-in de SSO de palavra-passe para o Internet Explorer ou o Chrome. A instalação poderá requerer que reinicie o seu browser.  

Quando regressar ao painel de acesso e selecione novamente o mosaico da aplicação, que é automaticamente iniciou sessão na aplicação.

Algumas aplicações podem implicar a sua palavra-passe, o primeiro início de sessão. Se tem alterado as suas credenciais na aplicação de terceiros de destino, tem também de atualizar as credenciais que são armazenadas no Azure AD. 

Para atualizar as suas credenciais, efetue o seguinte:

1. Selecione o ícone no mosaico aplicações.
2. Selecione **atualizar credenciais** reintroduzir o nome de utilizador e palavra-passe para a aplicação.


### <a name="application-with-existing-sso-solutions"></a>Aplicações com soluções de SSO existentes

Para configurar o SSO para uma aplicação, o portal do Azure fornece uma terceira opção, denominada existente Single Sign-On. Esta opção permite que o administrador para criar uma ligação para uma aplicação e coloque-a no painel de acesso para utilizadores selecionados.

Por exemplo, se uma aplicação estiver configurada para autenticar os utilizadores através do AD FS 2.0, o administrador pode utilizar a opção existente Single Sign-On para criar uma ligação ao mesmo no painel de acesso. Quando acedem a ligação, são autenticada através do AD FS 2.0 ou qualquer SSO solução existente que a aplicação fornece.


## <a name="next-steps"></a>Passos Seguintes

- Para ver uma lista de todos os tópicos relacionados com a gestão de aplicações, consulte o [índice de artigos da gestão de aplicações no Azure Active Directory](active-directory-apps-index.md).
 
- Para saber como integrar uma aplicação SaaS com o Azure AD, consulte o [lista de tutoriais sobre como integrar aplicações SaaS](active-directory-saas-tutorial-list.md).
 
- Para saber mais sobre a gestão de aplicações com o Azure AD, consulte o [introdução ao acesso de aplicação único de início de sessão e gerir com o Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- Para saber mais sobre o aprovisionamento de utilizador, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações SaaS](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
