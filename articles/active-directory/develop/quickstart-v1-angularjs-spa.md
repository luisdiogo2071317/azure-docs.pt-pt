---
title: Criar uma aplicação de página única AngularJS para início de sessão e fim de sessão com o Azure Active Directory | Microsoft Docs
description: Saiba como criar uma aplicação de página única AngularJS que se integra com o Azure AD para início de sessão e chama APIs protegidas pelo Azure AD através de OAuth.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c508c402707663489fc383dccd958559df6b217f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194687"
---
# <a name="quickstart-build-an-angularjs-single-page-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Início rápido: Criar uma aplicação de página única AngularJS para início de sessão e fim de sessão no Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Com o Azure Active Directory (Azure AD), pode adicionar de forma simples e direta início de sessão, fim de sessão e chamadas seguras à API de OAuth às suas aplicações de página única. Permite que as suas aplicações autentiquem os utilizadores com as respetivas contas do Windows Server Active Directory e consumam qualquer API Web que o Azure AD ajuda a proteger, como as APIs do Office 365 ou a API do Azure.

Para aplicações JavaScript em execução num browser, o Azure AD fornece a Biblioteca de Autenticação do Active Directory (ADAL) ou adal.js. O único propósito de adal.js é tornar a obtenção de tokens de acesso mais fácil para a sua aplicação.

Neste início rápido, vai aprender a criar uma aplicação de Lista de Tarefas AngularJS, que:

* Inicia a sessão do utilizador na aplicação utilizando o Azure AD como fornecedor de identidade.
* Apresenta algumas informações sobre o utilizador.
* Chama em segurança a API de Lista de Tarefas da aplicação utilizando os tokens de portador do Azure AD.
* Termina a sessão do utilizador na aplicação.

Para criar a aplicação funcional e completa, precisa de:

1. Registar a aplicação com o Azure AD.
2. Instalar a ADAL e configurar a aplicação de página única.
3. Utilizar a ADAL para ajudar a proteger as páginas na aplicação de página única.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, conclua estes pré-requisitos:

* [Transfira a estrutura da aplicação](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) ou [transfira o exemplo concluído](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).
* Tenha um inquilino do Azure AD no qual possa criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino, [saiba como obter um](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Passo 1: Registar a aplicação de DirectorySearcher

Para permitir que a aplicação autentique utilizadores e obtenha tokens, primeiro tem de registá-la no seu inquilino do Azure AD:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Se tiver sessão iniciada em vários diretórios, poderá ter de certificar-se de que está a ver o diretório correto. Para tal, na barra superior, clique na sua conta. Na lista **Diretório**, escolha o inquilino do Azure AD onde pretende registar a aplicação.
1. Clique em **Todos os serviços** no painel esquerdo e selecione **Azure Active Directory**.
1. Clique em **Registos de aplicações** e selecione **Adicionar**.
1. Siga os avisos e crie uma nova aplicação Web e/ou API Web:

    * O **Nome** descreve a sua aplicação aos utilizadores.
    * O **URL de início de sessão** é a localização na qual o Azure AD irá devolver tokens. A localização predefinida para este exemplo é `https://localhost:44326/`.

1. Depois de terminar o registo, o Azure AD atribui um ID exclusivo à sua aplicação. Este valor vai ser preciso nas secções seguintes, por isso, copie-o a partir do separador da aplicação.
1. Adal.js utiliza o fluxo implícito de OAuth para comunicar com o Azure AD. Tem de ativar o fluxo implícito para a sua aplicação:

    1. Clique na aplicação e selecione **Manifesto** para abrir o editor de manifesto inline.
    1. Localize a propriedade `oauth2AllowImplicitFlow`. Defina o respetivo valor como `true`.
    1. Clique em **Guardar** para guardar o manifesto.

1. Conceda permissões no seu inquilino para a sua aplicação. Vá para **Definições > Permissões obrigatórias** e selecione o botão **Conceder permissões** na barra superior.
1. Selecione **Sim** para confirmar.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>Passo 2: Instalar o ADAL e configurar a aplicação de página única

Agora que já tem uma aplicação no Azure AD, pode instalar o adal.js e escrever o seu código relacionado com identidade.

### <a name="configure-the-javascript-client"></a>Configurar o cliente JavaScript

Comece por adicionar o adal.js ao projeto TodoSPA através da Consola do Gestor de Pacotes:

1. Transfira [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) e adicione ao diretório do projeto `App/Scripts/`.
2. Transfira [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) e adicione ao diretório do projeto `App/Scripts/`.
3. Carregue cada um dos scripts antes do fim do `</body>` em `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Configurar o servidor back-end

Para que a API de Lista de Tarefas back-end da aplicação de página única aceite tokens do browser, o back-end precisa de informações de configuração sobre o registo da aplicação. No projeto TodoSPA, abra `web.config`. Substitua os valores dos elementos na secção `<appSettings>` para refletir os valores que utilizou no portal do Azure. O seu código vai fazer referência a estes valores sempre que utilizar a ADAL.

   * `ida:Tenant` é o domínio do inquilino do Azure AD, por exemplo, contoso.onmicrosoft.com.
   * `ida:Audience` é o ID de cliente da sua aplicação que copiou do portal.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>Passo 3: Utilizar a ADAL para o ajudar a proteger páginas na aplicação de página única

Adal.js integra-se com os fornecedores de HTTP e a rota de AngularJS, para que possa ajudar a proteger as vistas individuais na sua aplicação de página única.

1. Em `App/Scripts/app.js`, introduza o módulo adal.js:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Inicialize `adalProvider` com os valores de configuração do seu registo de aplicação, também em `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Para ajudar a proteger a vista `TodoList` na aplicação, utilize apenas uma linha de código: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Resumo

Agora, tem uma aplicação de página única segura que pode iniciar sessão dos utilizadores e emitir pedidos protegidos por tokens de portador para a API back-end. Quando um utilizador clica na hiperligação **TodoList**, o adal.js irá redirecionar automaticamente para o Azure AD para início de sessão, caso seja necessário. Além disso, o adal.js irá associar automaticamente um token de acesso aos pedidos Ajax enviados para o back-end da aplicação.

Os passos anteriores são o mínimo necessário para criar uma aplicação de página única com o adal.js. Mas existem outras funcionalidades úteis na aplicação de página única:

* Para emitir explicitamente os pedidos de início de sessão e fim de sessão, pode definir funções nos seus controladores que invocam o adal.js. Em `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Poderá querer apresentar informações do utilizador na IU da aplicação. O serviço ADAL já foi adicionado ao controlador `userDataCtrl`, pelo que pode aceder ao objeto `userInfo` na vista associada, `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Existem muitos cenários em que vai querer saber se o utilizador tem sessão iniciada ou não. Também pode utilizar o objeto `userInfo` para recolher estas informações. Por exemplo, em `index.html`, pode mostrar ou botão **Iniciar sessão** ou **Terminar sessão** com base no estado de autenticação:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

A aplicação de página única integrada no Azure AD pode autenticar utilizadores, chamar em segurança o respetivo back-end através de OAuth 2.0 e obter informações básicas sobre o utilizador. Caso ainda não o tenha feito, está na altura de preencher o seu inquilino com alguns utilizadores. Execute a aplicação de página única Lista de Tarefas e inicie sessão com um desses utilizadores. Adicione tarefas à lista de tarefas do utilizador, termine sessão volte a iniciar sessão.

O adal.js facilita a incorporação de funcionalidades de identidade comum na sua aplicação. Faz todo o trabalho desagradável por si: gestão de cache, suporte do protocolo OAuth, apresentar ao utilizador uma IU de início de sessão, atualizar os tokens expirados e muito mais.

Para sua referência, o exemplo concluído (sem os valores de configuração) está disponível no [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Passos Seguintes

Agora, pode avançar para cenários adicionais.

> [!div class="nextstepaction"]
> [Chamar uma API Web CORS a partir de uma aplicação de página única](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).
