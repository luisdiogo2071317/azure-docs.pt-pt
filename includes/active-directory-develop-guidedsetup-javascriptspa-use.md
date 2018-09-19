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
ms.openlocfilehash: 94d57abc95dabf1da579f6d2105ca6c74140a86f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293465"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Utilizar o Microsoft Authentication Library (MSAL) para iniciar a sessão do utilizador

1.  Crie um ficheiro com o nome `app.js`. Se estiver a utilizar o Visual Studio, selecione o projeto (pasta raiz do projeto), clique com botão direito e selecione: `Add`  >  `New Item`  >  `JavaScript File`:
2.  Adicione o seguinte código para seu `app.js` ficheiro:

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show sign-out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });
    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from sign-in
 * @param {object} error - The error string
 * @param {string} tokenType - The token type: For loginRedirect, tokenType = "id_token". For acquireTokenRedirect, tokenType:"access_token".
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a>Mais Informações

Depois de um usuário clica o *'Chamar o Microsoft Graph API'* botão pela primeira vez, `callGraphApi` chamadas de método `loginRedirect` para iniciar a sessão do utilizador. Este método resulta em redirecionar o utilizador para o *ponto final do Microsoft Azure Active Directory v2* para solicitar e validar as credenciais do utilizador. Como resultado de um sessão com êxito-, o utilizador é redirecionado para o original *Index. HTML* página e um token é recebida, processados pelo `msal.js` e as informações contidas no token é colocado em cache. Este token é conhecido como o *token de ID* e contém informações básicas sobre o usuário, como o nome de exibição do usuário. Se planeia utilizar todos os dados fornecidos por este token para fins, terá de certificar-se de que este token é validado pelo seu servidor de back-end para garantir que o token foi emitido para um utilizador válido para a sua aplicação.

O SPA gerado por este guia não faz usar diretamente o token de ID – em vez disso, ele chama `acquireTokenSilent` e/ou `acquireTokenRedirect` adquirir um *token de acesso* usado para consultar o Microsoft Graph API. Se precisar de uma amostra que valida o token de ID, dê uma olhada [isso](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github de exemplo active-directory-javascript-singlepageapp-dotnet-webapi-v2") aplicativo de exemplo no GitHub – o exemplo usa um ASP .NET API da web para validação do token.

#### <a name="getting-a-user-token-interactively"></a>Obter um utilizador token interativamente

Depois do inicial início de sessão, que não pretende peça aos utilizadores para autenticar toda vez que precisam para solicitar um token para aceder a um recurso – então *acquireTokenSilent* deve ser utilizada a maior parte do tempo para adquirir tokens. Há contudo situações que precisa forçar os utilizadores interajam com o ponto de final do Azure Active Directory v2 – alguns exemplos incluem:
- Os utilizadores podem ter de reintroduzir as respetivas credenciais porque a palavra-passe expirou
- A aplicação está a solicitar acesso a um recurso que o usuário precisa para dar consentimento para
- Autenticação de dois fatores é necessário

Chamar o *acquireTokenRedirect(scope)* resultar em redirecionar utilizadores para o ponto de final do Azure Active Directory v2 (ou *acquireTokenPopup(scope)* resultado numa janela de pop-up) em que os utilizadores têm de interagir por confirmar as respetivas credenciais, a dar o consentimento para o recurso necessário ou concluir os dois avaliar autenticação.

#### <a name="getting-a-user-token-silently"></a>Obter um utilizador token silenciosamente
O ` acquireTokenSilent` método processa a aquisições de token e a renovação sem qualquer interação do utilizador. Após `loginRedirect` (ou `loginPopup`) é executado pela primeira vez, `acquireTokenSilent` é o método normalmente usado para obter os tokens utilizados para aceder a recursos protegidos por chamadas subsequentes - como chamadas para pedir ou renovar os tokens são feitas automaticamente.
`acquireTokenSilent` pode falhar em alguns casos – por exemplo, a senha do usuário expirou. Seu aplicativo pode manipular essa exceção de duas formas:

1.  Fazer uma chamada para `acquireTokenRedirect` imediatamente, o que resulta em pedir ao utilizador para iniciar sessão. Este padrão é muito usado em aplicativos online onde não há nenhum conteúdo não autenticado no aplicativo disponível para o utilizador. O exemplo gerado por esta configuração assistida usa esse padrão.

2. Aplicativos também podem tornar uma indicação visual para o usuário que um interativo início de sessão é necessário, para que o usuário pode selecionar o momento certo para iniciar sessão ou a aplicação pode repetir `acquireTokenSilent` num momento posterior. Isto é normalmente utilizado quando o utilizador pode utilizar outras funcionalidades da aplicação sem a ser interrompida – por exemplo, não existe conteúdo não autenticado no aplicativo. Neste caso, o usuário pode decidir quando pretende iniciar sessão para aceder ao recurso protegido ou para atualizar as informações Desatualizadas.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Chamar o Microsoft Graph API com o token de que obteve apenas

Adicione o seguinte código para seu `app.js` ficheiro:

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in the page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in the page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Obter mais informações sobre como fazer uma chamada REST em relação a uma API protegida

No aplicativo de exemplo criado por este guia, o `callWebApiWithToken()` método é utilizado para fazer um HTTP `GET` em relação a um recurso protegido que necessita de um token do pedido e, em seguida, devolver o conteúdo para o chamador. Este método adiciona o token obtido no *cabeçalho de autorização de HTTP*. Para o aplicativo de exemplo criado por este guia, o recurso é o Microsoft Graph API *me* ponto de extremidade – que exibe informações de perfil do usuário.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Adicione um método para terminar sessão do utilizador

Adicione o seguinte código para seu `app.js` ficheiro:

```javascript
/**
 * Sign out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
