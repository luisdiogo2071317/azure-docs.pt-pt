---
title: O Azure AD AngularJS, introdução | Documentos da Microsoft
description: Como criar uma aplicação de página única do AngularJS que se integra com o Azure AD para início de sessão e chama APIs do Azure AD protegida ao utilizar o OAuth.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0c7f6a0e447e3b48cdd1df684dc105ece1e98f66
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502054"
---
# <a name="azure-ad-angularjs-getting-started"></a>O Azure AD AngularJS, introdução

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Simples e direta para adicionar a API de OAuth do início de sessão, fim de sessão e seguro chama às suas aplicações de página única do Azure torna o Active Directory (Azure AD). Permite que as suas aplicações para autenticar os utilizadores com as suas contas do Windows Server Active Directory e consumir qualquer web API do Azure AD ajuda a proteger, tais como as APIs do Office 365 ou a API do Azure.

Para aplicações de JavaScript em execução num browser, o Azure AD fornece o Active Directory Authentication Library (ADAL), ou adal.js. É o único propósito de adal.js torna mais fácil para a sua aplicação obter os tokens de acesso. Para demonstrar apenas como é fácil, aqui vamos criar uma aplicação AngularJS lista de tarefas que:

* Inicia o utilizador a sessão para a aplicação através da utilização do Azure AD como fornecedor de identidade.

* Apresenta algumas informações sobre o utilizador.
* Chama em segurança para fazer API da aplicação de lista utilizando os tokens de portador do Azure AD.
* Inicia o utilizador fora da aplicação.

Para criar a aplicação de trabalho completo, terá de:

1. Registe a sua aplicação com o Azure AD.
2. Instale o ADAL e configurar a aplicação de página única.
3. Utilize a ADAL para o ajudar a proteger páginas na aplicação de página única.

Para começar, [transferir a estrutura de aplicação](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) ou [transfira o exemplo concluído](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Também precisa de um inquilino do Azure AD em que pode criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino [Saiba como obter um](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Passo 1: Registar a aplicação de DirectorySearcher
Para ativar a sua aplicação para autenticar os utilizadores e obter os tokens de, primeiro tem de registá-lo no seu inquilino do Azure AD:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Se tiver entrado vários diretórios, terá de se certificar de que está a ver o diretório correto. Para tal, na barra superior, clique na sua conta. Sob o **Directory** lista, escolha o inquilino do Azure AD onde pretende registar a sua aplicação.
3. Clique em **todos os serviços** no painel esquerdo e, em seguida, selecione **Azure Active Directory**.
4. Clique em **registos de aplicações**e, em seguida, selecione **Add**.
5. Siga as instruções e crie uma nova aplicação web e/ou web API:
  * **Nome** descreve a sua aplicação aos utilizadores.
  * **Início de sessão no URL** é a localização para o qual o Azure AD irá devolver tokens. A localização predefinida para este exemplo é `https://localhost:44326/`.
6. Após ter concluído o registo, o Azure AD atribui um ID exclusivo para a sua aplicação. Precisará desse valor nas próximas seções, por isso, copie-o do separador de aplicação.
7. Adal.js utiliza o fluxo implícito de OAuth para comunicar com o Azure AD. Tem de ativar o fluxo implícito para a sua aplicação:
  1. Clique na aplicação e selecione **manifesto** para abrir o editor de manifesto inline.
  2. Localize o `oauth2AllowImplicitFlow` propriedade. Defina seu valor como `true`.
  3. Clique em **guardar** para guardar o manifesto.
8. Conceder permissões com o seu inquilino para a sua aplicação. Aceda a **configurações** > **permissões obrigatórias**e clique no **conceder permissões** botão na barra superior. Clique em **Sim** para confirmar.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>Passo 2: ADAL instalar e configurar a aplicação de página única
Agora que tem uma aplicação no Azure AD, pode instalar adal.js e escrever seu código relacionadas com a identidade.

### <a name="configure-the-javascript-client"></a>Configurar o cliente de JavaScript
Comece por adicionar adal.js ao projeto TodoSPA utilizando a consola do Gestor de pacotes:
  1. Baixe [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) e adicione-o para o `App/Scripts/` diretório do projeto.
  2. Baixe [adal angular](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) e adicione-o para o `App/Scripts/` diretório do projeto.
  3. Carregar cada script antes de terminar a `</body>` em `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Configurar o servidor de back-end
Para back-end para fazer lista API a aplicação de página única aceitar tokens do navegador, o back-end precisa de informações de configuração sobre o registo de aplicações. No projeto TodoSPA, abra `web.config`. Substitua os valores dos elementos no `<appSettings>` secção para refletir os valores que utilizou no portal do Azure. Seu código irá referenciar estes valores, sempre que utilizar a ADAL.
  * `ida:Tenant` é o domínio de inquilino do Azure AD – por exemplo, contoso.onmicrosoft.com.
  * `ida:Audience` é o ID de cliente da sua aplicação que copiou do portal.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>Passo 3: Utilizar a ADAL para o ajudar a proteger páginas na aplicação de página única
Adal.js integra-se com o AngularJS rota e provedores HTTP, para que possa ajudar seguras vistas individuais na sua aplicação de página única.

1. No `App/Scripts/app.js`, colocar no módulo adal.js:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Inicializar `adalProvider` com os valores de configuração do seu registo de aplicação, também no `App/Scripts/app.js`:

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
3. Ajudar a proteger os `TodoList` vista na aplicação ao utilizar apenas uma linha de código: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Resumo
Tem agora uma aplicação de página única segura que pode iniciar sessão dos utilizadores e emitir pedidos de protegida de token de portador para sua API de back-end. Quando um usuário clica o **TodoList** link, adal.js será automaticamente redirecionada para o Azure AD para início de sessão se for necessário. Além disso, adal.js irá anexar automaticamente um token de acesso a quaisquer pedidos de Ajax que são enviados para o back-end da aplicação. 

Os passos anteriores são o mínimo necessário para criar uma aplicação de página única com adal.js. Mas alguns outros recursos são úteis na aplicação de página única:

* Para emitir explicitamente os pedidos de início de sessão e fim de sessão, pode definir funções em seus controladores que invocam adal.js. No `App/Scripts/homeCtrl.js`:

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
* Pode querer apresentar informações de utilizador na IU da aplicação. Já foi adicionado o serviço da ADAL para o `userDataCtrl` controlador, para que possa aceder a `userInfo` objeto na vista de associados, `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Existem muitos cenários em que vai querer saber se o utilizador tem sessão iniciado ou não. Também pode utilizar o `userInfo` objeto recolher estas informações. Por exemplo, em `index.html`, pode mostrar ou o **início de sessão** ou **fim de sessão** botão com base no estado de autenticação:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

A aplicação de página única integrada no AD do Azure pode autenticar utilizadores, com segurança chamar o respetivo back-end ao utilizar o OAuth 2.0 e obter informações básicas sobre o utilizador. Se ainda não o tiver feito, agora é o tempo para preencher o seu inquilino com alguns usuários. Executar a aplicação de página única de lista de tarefas e iniciar sessão com um desses usuários. Adicionar tarefas à lista de tarefas do utilizador, termine e inicie sessão novamente.

Adal.js torna mais fácil incorporar funcionalidades de identidade comum em seu aplicativo. Ele cuida de todo o trabalho sujo para: gestão de cache, o suporte de protocolo de OAuth, apresentando ao usuário um início de sessão da interface do Usuário, atualizando tokens expirados e muito mais.

Para referência, o exemplo concluído (sem os valores de configuração) está disponível no [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Passos Seguintes
Agora pode passar para cenários adicionais. Talvez queira experimentar: [chamar uma API da web CORS a partir de uma aplicação de página única](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
