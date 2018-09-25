---
title: Início de JavaScript rápido do Azure AD v2 | Documentos da Microsoft
description: Saiba como os aplicativos JavaScript podem chamar uma API que precisam de tokens de acesso ao ponto final de v2.0 do Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 1b884571707aab71e8a8d124ba68f938e5a63a43
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063749"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Início rápido: Iniciar sessão dos utilizadores e adquirir um token de acesso a partir de uma aplicação do JavaScript

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Neste início rápido, irá aprender como usar um código de exemplo que demonstra como um JavaScript a aplicação de página única (SPA) pode iniciar sessão em contas pessoais, trabalhar e escolar contas, obter um token de acesso para chamar a API do Microsoft Graph ou qualquer API web.

![Como funciona a aplicação de exemplo gerada por este início rápido](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Registar a sua aplicação e transferir a aplicação de início rápido
>
> ### <a name="step-1-register-your-application"></a>Passo 1: Registar a sua aplicação
>
> 1. Vá para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app) registar uma aplicação.
> 1. Na **nome da aplicação** , introduza um nome para a sua aplicação.
> 1. Certifique-se de que o **configuração interativa** caixa de verificação está desmarcada e, em seguida, selecione **criar**.
> 1. Clique em **adicionar plataforma**, em seguida, selecione **Web**.
> 1. Certifique-se **permitir fluxo implícito** é *verificado*.
> 1. Sob **URLs de redirecionamento** adicionar `http://localhost:30662/`.
> 1. Clique em **Guardar**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Passo 1: Configurar a sua aplicação no portal do Azure
> Para o código de exemplo para este início rápido funcionar, terá de adicionar um redirecionamento URI como `http://localhost:30662/` e ative **concessão implícita**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Efetuar estas alterações para mim]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Já configurado](media/quickstart-v2-javascript/green-check.png) seu aplicativo está configurado com esses atributos

#### <a name="step-2-download-the-project"></a>Passo 2: Transferir o projeto

Pode escolher uma destas opções adequadas ao seu ambiente de desenvolvimento.
* [Transferir os ficheiros de projeto core - para um servidor web, como o node. js](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Transfira o projeto do Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Extraia o ficheiro zip para uma pasta local (por exemplo, **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>Passo 3: Configurar a sua aplicação do JavaScript

> [!div renderon="docs"]
> Editar `index.html` e substitua `Enter_the_Application_Id_here` sob `applicationConfig` com o ID da aplicação que acabou de registar.

> [!div class="sxs-lookup" renderon="portal"]
> Editar `index.html` e substitua `applicationConfig` com:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!NOTE]
>Se usar [node. js](https://nodejs.org/en/download/), o *Server. js* ficheiro está configurado para o servidor iniciar a escuta na porta 30662.
> Se usar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), o exemplo de código *. csproj* ficheiro está configurado para o servidor iniciar a escuta na porta 30662.
>

#### <a name="step-4-run-the-project"></a>Passo 4: Executar o projeto

Se utilizar o node. js, numa linha de comandos pode executar o seguinte do diretório do projeto para iniciar o servidor:
 ```batch
 npm install
 node server.js
 ```
Abra um browser e navegue para `http://localhost:30662/`. Clique em **sessão** botão para iniciar o início de sessão e, em seguida, chamar o Microsoft Graph API.

Se utilizar o Visual Studio, certifique-se selecionar a solução de projeto e, em seguida, prima **F5** para executar o seu projeto.

## <a name="more-information"></a>Mais Informações

### <a name="msaljs"></a>*msal.js*

A MSAL é a biblioteca utilizada para iniciar sessão de utilizadores e tokens utilizados para aceder a uma API protegida pelo Microsoft Azure Active Directory (Azure AD) do pedido. O guia de introdução *Index* contém uma referência à biblioteca:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

Em alternativa, se tiver o nó instalado, pode baixá-lo através do npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>Inicialização de MSAL

O código de início rápido mostra também como inicializar a biblioteca:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Onde  |  |
> |---------|---------|
> |`ClientId`     |Id da aplicação da aplicação registada no portal do Azure|
> |`authority`    |É o URL da autoridade. Passando *nulo* define a autoridade de padrão para `https://login.microsoftonline.com/common`. Se a aplicação for inquilino individual (direcionamento para contas apenas num diretório), defina este valor como `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Método de retorno de chamada chamado após a autenticação redireciona novamente para a aplicação. Aqui `acquireTokenRedirectCallBack` é passado. Isso é nulo se loginPopup a utilizar.|
> |`options`  |Uma coleção de parâmetros opcionais. Neste caso `storeAuthStateInCookie` e `cacheLocation` são configuração opcional. Consulte a [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) para obter mais detalhes sobre as opções. |

### <a name="sign-in-users"></a>Iniciar sessão dos utilizadores

O fragmento de código seguinte mostra como iniciar sessão dos utilizadores:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Onde  |  |
> |---------|---------|
> | `scopes`   | (Opcional) Contém os âmbitos que está a ser pedidos para o consentimento do utilizador em tempo de início de sessão (Ex: `[ "user.read" ]` para o Microsoft Graph ou `[ "<Application ID URL>/scope" ]` para as APIs da Web personalizado (ou seja, `api://<Application ID>/access_as_user` ). Aqui `applicationConfig.graphScopes` é passado. |

> [!TIP]
> Em alternativa, talvez queira usar o `loginRedirect` método para redirecionar a página atual para a página de início de sessão, em vez de uma janela de pop-up.


### <a name="request-tokens"></a>Pedir tokens

A MSAL possui três métodos utilizados para adquirir tokens: `acquireTokenRedirect`, `acquireTokenPopup` e `acquireTokenSilent`:

#### <a name="get-a-user-token-silently"></a>Obter um utilizador token silenciosamente

O `acquireTokenSilent` método processa a aquisições de token e a renovação sem qualquer interação do utilizador. Depois do `loginRedirect` ou `loginPopup` método é executado pela primeira vez, `acquireTokenSilent` é o método normalmente usado para obter os tokens que são utilizados para aceder a recursos protegidos por chamadas subsequentes. Chamadas para pedir ou renovar os tokens são feitas automaticamente.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Onde  |  |
> |---------|---------|
> | `scopes`   | Contém a ser pedidos a serem retornados no token de acesso para a API de âmbitos (Ex: `[ "user.read" ]` para o Microsoft Graph ou `[ "<Application ID URL>/scope" ]` para as APIs da Web personalizado (ou seja, `api://<Application ID>/access_as_user` ). Aqui `applicationConfig.graphScopes` é passado.|

#### <a name="get-a-user-token-interactively"></a>Obter um utilizador token interativamente

 Existem situações em que precisa forçar os utilizadores interajam com o ponto final de v2.0 do Azure AD. Por exemplo:
* Os utilizadores podem ter de reintroduzir as respetivas credenciais porque a palavra-passe expirou
* A aplicação está a solicitar acesso a âmbitos de recursos adicionais que o usuário precisa para dar consentimento para
* Autenticação de dois fatores é necessário

Padrão recomendado normal para a maioria dos aplicativos é chamar `acquireTokenSilent` em primeiro lugar, em seguida, capturar a exceção e, em seguida, chame `acquireTokenRedirect` (ou `acquireTokenPopup`) para iniciar um pedido de interativo.

Chamar o `acquireTokenPopup(scope)` resulta numa janela de pop-up para iniciar sessão (ou `acquireTokenRedirect(scope)` resulta em redirecionar utilizadores para o ponto de final de v2.0 do Azure AD) em que os utilizadores têm de interagir por qualquer uma das suas credenciais, a confirmar a dar o consentimento para o recurso necessário, ou concluir a autenticação de dois fatores.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> Este início rápido utiliza a `loginRedirect` e `acquireTokenRedirect` métodos quando o browser utilizado é o Internet Explorer devido a um [problema conhecido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) relacionadas com a manipulação de janelas pop-ups pelo navegador do Internet Explorer.

## <a name="next-steps"></a>Passos Seguintes

Para obter um guia passo a passo mais detalhado sobre como criar a aplicação para este início rápido, experimente o tutorial de JavaScript abaixo.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Conheça as etapas para criar a aplicação para este início rápido

> [!div class="nextstepaction"]
> [Tutorial de API de gráfico de chamada](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Procurar o repositório MSAL para documentação, perguntas frequentes, problemas e muito mais

> [!div class="nextstepaction"]
> [repositório do GitHub de msal](https://github.com/AzureAD/microsoft-authentication-library-for-js)


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
