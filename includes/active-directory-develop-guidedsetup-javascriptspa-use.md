---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: be8ffeae1977fb2f56e0f85a716d982a6d2f84dc
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47061075"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Utilizar o Microsoft Authentication Library (MSAL) para iniciar a sessão do utilizador

1. Adicione o seguinte código para seu `index.html` dentro do ficheiro a `<script></script>` etiquetas:

```javascript
//Pass null for default authority (https://login.microsoftonline.com/common)
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack,
    {storeAuthStateInCookie: true, cacheLocation: "localStorage"});

function signIn() {
    myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
        //Login Success
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }, function (error) {
        console.log(error);
    });
}

function acquireTokenPopupAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
        callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        // Call acquireTokenPopup (popup window) in case of acquireTokenSilent failure due to consent or interaction required ONLY
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
                callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
            }, function (error) {
                console.log(error);
            });
        }
    });
}

function graphAPICallback(data) {
    //Display user data on DOM
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += " to Microsoft Graph API!!";
    document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
}

function showWelcomeMessage() {
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += 'Welcome ' + myMSALObj.getUser().name;
    var loginbutton = document.getElementById('SignIn');
    loginbutton.innerHTML = 'Sign Out';
    loginbutton.setAttribute('onclick', 'signOut();');
}

// This function can be removed if you do not need to support IE
function acquireTokenRedirectAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
      callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        //Call acquireTokenRedirect in case of acquireToken Failure
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenRedirect(applicationConfig.graphScopes);
        }
    });
}

function acquireTokenRedirectCallBack(errorDesc, token, error, tokenType)
{
 if(tokenType === "access_token")
 {
     callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
 } else {
     console.log("token type is:"+tokenType);
 }
}


// Browser check variables
var ua = window.navigator.userAgent;
var msie = ua.indexOf('MSIE ');
var msie11 = ua.indexOf('Trident/');
var msedge = ua.indexOf('Edge/');
var isIE = msie > 0 || msie11 > 0;
var isEdge = msedge > 0;

//If you support IE, our recommendation is that you sign-in using Redirect APIs
//If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
if (!isIE) {
    if (myMSALObj.getUser()) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }
}
else {
    document.getElementById("SignIn").onclick = function () {
        myMSALObj.loginRedirect(applicationConfig.graphScopes);
    };
    if (myMSALObj.getUser() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenRedirectAndCallMSGraph();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Mais Informações

Depois de um usuário clica o *iniciar sessão* botão pela primeira vez, `signIn` chamadas de método `loginPopup` para iniciar a sessão do utilizador. Este método resulta em abrir uma janela de pop-up com a *ponto final do Microsoft Azure Active Directory v2* para solicitar e validar as credenciais do utilizador. Como resultado de um sessão com êxito-, o utilizador é redirecionado para o original *Index. HTML* página e um token é recebida, processados pelo `msal.js` e as informações contidas no token é colocado em cache. Este token é conhecido como o *token de ID* e contém informações básicas sobre o usuário, como o nome de exibição do usuário. Se planeia utilizar todos os dados fornecidos por este token para fins, terá de certificar-se de que este token é validado pelo seu servidor de back-end para garantir que o token foi emitido para um utilizador válido para a sua aplicação.

O SPA gerado por este guia de chamadas `acquireTokenSilent` e/ou `acquireTokenPopup` adquirir um *token de acesso* usado para consultar a Graph API da Microsoft para informações de perfil do usuário. Se precisar de uma amostra que valida o token de ID, dê uma olhada [isso](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github de exemplo active-directory-javascript-singlepageapp-dotnet-webapi-v2") aplicativo de exemplo no GitHub – o exemplo usa um ASP .NET API da web para validação do token.

#### <a name="getting-a-user-token-interactively"></a>Obter um utilizador token interativamente

Depois do inicial início de sessão, que não pretende peça aos utilizadores para autenticar toda vez que precisam para solicitar um token para aceder a um recurso – então *acquireTokenSilent* deve ser utilizada a maior parte do tempo para adquirir tokens. Há contudo situações que precisa forçar os utilizadores interajam com o ponto de final do Azure Active Directory v2 – alguns exemplos incluem:
- Os utilizadores podem ter de reintroduzir as respetivas credenciais porque a palavra-passe expirou
- A aplicação está a solicitar acesso a um recurso que o usuário precisa para dar consentimento para
- Autenticação de dois fatores é necessário

Chamar o *acquireTokenPopup(scope)* resulta numa janela de pop-up (ou *acquireTokenRedirect(scope)* resulta em redirecionar utilizadores para o ponto de final do Azure Active Directory v2) em que os utilizadores precisam de interagir por confirmar as respetivas credenciais, a dar o consentimento para o recurso necessário ou concluir a autenticação de dois fatores.

#### <a name="getting-a-user-token-silently"></a>Obter um utilizador token silenciosamente
O ` acquireTokenSilent` método processa a aquisições de token e a renovação sem qualquer interação do utilizador. Após `loginPopup` (ou `loginRedirect`) é executado pela primeira vez, `acquireTokenSilent` é o método normalmente usado para obter os tokens utilizados para aceder a recursos protegidos por chamadas subsequentes - como chamadas para pedir ou renovar os tokens são feitas automaticamente.
`acquireTokenSilent` pode falhar em alguns casos – por exemplo, a senha do usuário expirou. Seu aplicativo pode manipular essa exceção de duas formas:

1.  Fazer uma chamada para `acquireTokenPopup` imediatamente, o que resulta em pedir ao utilizador para iniciar sessão. Este padrão é muito usado em aplicativos online onde não há nenhum conteúdo não autenticado no aplicativo disponível para o utilizador. O exemplo gerado por esta configuração assistida usa esse padrão.

2. Aplicativos também podem tornar uma indicação visual para o usuário que um interativo início de sessão é necessário, para que o usuário pode selecionar o momento certo para iniciar sessão ou a aplicação pode repetir `acquireTokenSilent` num momento posterior. Isto é normalmente utilizado quando o utilizador pode utilizar outras funcionalidades da aplicação sem a ser interrompida – por exemplo, não existe conteúdo não autenticado no aplicativo. Neste caso, o usuário pode decidir quando pretende iniciar sessão para aceder ao recurso protegido ou para atualizar as informações Desatualizadas.

> [!NOTE]
> O código anterior usa o `loginRedirect` e `acquireTokenRedirect` métodos quando o browser utilizado é o Internet Explorer devido a um [problema conhecido](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) relacionadas com a manipulação de janelas pop-ups pelo navegador do Internet Explorer.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API com o token de que obteve apenas

Adicione o seguinte código para seu `index.html` dentro do ficheiro a `<script></script>` etiquetas:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Obter mais informações sobre como fazer uma chamada REST em relação a uma API protegida

No aplicativo de exemplo criado por este guia, o `callMSGraph()` método é utilizado para fazer um HTTP `GET` em relação a um recurso protegido que necessita de um token do pedido e, em seguida, devolver o conteúdo para o chamador. Este método adiciona o token obtido no *cabeçalho de autorização de HTTP*. Para o aplicativo de exemplo criado por este guia, o recurso é o Microsoft Graph API *me* ponto de extremidade – que exibe informações de perfil do usuário.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adicione um método para terminar sessão do utilizador

Adicione o seguinte código para seu `index.html` dentro do ficheiro a `<script></script>` etiquetas:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```
