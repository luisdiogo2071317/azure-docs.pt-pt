---
title: Web de node. js do AD introdução da API do Azure | Documentos da Microsoft
description: Como criar uma API que se integra com o Azure AD para autenticação de web do node. js REST.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 3b203e5be82c01e7d586c90bae454aca23ebd630
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581719"
---
# <a name="azure-ad-nodejs-web-api-getting-started"></a>Web de node. js do AD introdução da API do Azure

Este artigo demonstra como proteger uma [Restify](http://restify.com/) ponto final de API com [Passport](http://passportjs.org/) utilizando o [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) módulo para lidar com a comunicação com o Azure Active Directory (AAD). 

O âmbito deste tutorial abrange as preocupações relativas ao proteger pontos finais da API. As preocupações de iniciar sessão e manter os tokens de autenticação não são implementadas aqui e são de responsabilidade de um aplicativo cliente. Para obter detalhes que envolvem uma implementação de cliente, reveja [aplicação web de node. js início de sessão e fim de sessão com o Azure AD](quickstart-v1-openid-connect-code.md).

O exemplo de código completo associado a este artigo está disponível no [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="create-the-sample-project"></a>Criar o projeto de exemplo
Esta aplicação de servidor requer algumas dependências de pacote suporta o Restify e o Passport, bem como informações que são transmitidas para o AAD da conta.

Para começar, adicione o seguinte código para um ficheiro denominado `package.json`:

```Shell
{
  "name": "node-aad-demo",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "3.0.8",
    "restify": "6.0.1",
    "restify-plugins": "1.6.0"
  }
}
```

Uma vez `package.json` é criado, execute `npm install` na sua linha de comandos para instalar as dependências de pacote. 

### <a name="configure-the-project-to-use-active-directory"></a>Configurar o projeto para utilizar o Active Directory

Para começar a configurar a aplicação, existem alguns valores de conta específicos, que pode obter a partir da CLI do Azure. É a maneira mais fácil para começar a utilizar com a CLI utilizar o Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Introduza o seguinte comando no cloud shell: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

O [argumentos](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) para o `create` comando incluem:

| Argumento  | Descrição |
|---------|---------|
|`display-name` | Nome amigável do registo |
|`homepage` | URL em que os utilizadores podem iniciar sessão e utilize a sua aplicação |
|`identifier-uris` | Espaço separados por URIs exclusivas que o Azure AD pode utilizar para esta aplicação |

Antes de poder ligar ao Azure Active Directory, terá das seguintes informações:

| Nome  | Descrição | Nome da variável no arquivo de configuração |
| ------------- | ------------- | ------------- |
| Nome do inquilino  | [Nome do inquilino](quickstart-create-new-tenant.md) que pretende utilizar para autenticação | `tenantName`  |
| ID de Cliente  | ID de cliente é o termo de OAuth utilizado para o AAD _ID da aplicação_. |  `clientID`  |

De resposta do registo no Azure Cloud Shell, copie os `appId` valor e criar um novo ficheiro designado `config.js`. Em seguida, adicione o código a seguir e substitua os valores com os tokens entre parênteses:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```
Para obter mais informações sobre as definições de configuração individuais, reveja os [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) documentação do módulo.

## <a name="implement-the-server"></a>Implementar o servidor
O [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) módulo apresenta duas estratégias de autenticação: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) e [portador](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) estratégias. O servidor implementado neste artigo utiliza a estratégia de portador para proteger o ponto final da API.

### <a name="step-1-import-dependencies"></a>Passo 1: Dependências de importação
Crie um novo ficheiro designado `app.js` e cole o seguinte texto:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require('restify-plugins')
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

Nesta secção do código:

- O `restify` e `restify-plugins` módulos são referenciados para configurar um servidor do Restify.

- O `passport` e `passport-azure-ad` módulos são responsáveis por comunicar com o AAD.

- O `config` variável é inicializada com os valores do `config.js` ficheiro criado no passo anterior.

- É criada uma matriz para `authenticatedUserTokens` para armazenar tokens de utilizador, como eles são transmitidos em pontos de extremidade seguros.

- O `serverPort` é o definido de porta do ambiente de processo ou do ficheiro de configuração.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Passo 2: Criar uma instância de uma estratégia de autenticação
Como proteger um ponto de extremidade, tem de fornecer uma estratégia de responsável por determinar se é ou não a solicitação atual são originados por um usuário autenticado. Aqui o `authenticatonStrategy` variável é uma instância do `passport-azure-ad` `BearerStrategy` classe. Adicione o seguinte código após o `require` instruções.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let currentUser = null;

    let userToken = authenticatedUserTokens.find((user) => {
        currentUser = user;
        user.sub === token.sub;
    });

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, currentUser, token);
});
```
Essa implementação usa o registo automático ao adicionar os tokens de autenticação para o `authenticatedUserTokens` matriz se eles ainda não existam.

Depois de criar uma nova instância da estratégia, deve passá-lo ao Passport via o `use` método. Adicione o seguinte código para `app.js` para utilizar a estratégia no Passport.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Passo 3: Configuração de servidor
Com a estratégia de autenticação definida, pode agora configurar o servidor do Restify com algumas configurações básicas e definido para utilizar o Passport para segurança.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Este servidor é inicializado e configurado para analisar cabeçalhos de autorização e, em seguida, definido para utilizar o Passport.


### <a name="step-4-define-routes"></a>Passo 4: Definir rotas
Agora pode definir rotas e decidir que pretende proteger com o AAD. Esse projeto inclui duas rotas em que o nível de raiz é aberto e o `/api` rota é definida para exigir a autenticação.

No `app.js` adicione o seguinte código para a rota de nível de raiz:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

A rota de raiz permite que todos os pedidos através da rota e devolve uma mensagem que inclui um comando para testar o `/api` rota. Por outro lado, o `/api` rota permanece bloqueada usando [ `passport.authenticate` ](http://passportjs.org/docs/authenticate). Adicione o seguinte código após a rota de raiz.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Esta configuração permite apenas pedidos autenticados, que incluem um acesso de token de portador a `/api`. A opção de `session: false` é usada para desabilitar as sessões para exigir que um token é passado com cada solicitação para a API.

Por fim, o servidor está definido para escutar na porta configurada chamando o `listen` método.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Executar o exemplo

Agora que o servidor estiver implementado, pode iniciar o servidor ao abrir um prompt de comando e introduza:

```Shell
npm start
```

Com o servidor em execução, pode submeter um pedido para o servidor para testar os resultados. Para demonstrar a resposta da rota de raiz, abra uma shell bash e introduza o seguinte código:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Se tiver configurado o seu servidor corretamente, a resposta deve ter um aspeto semelhante a:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Em seguida, pode testar a rota que exija a autenticação ao introduzir o seguinte comando na sua shell do bash:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Se tiver configurado o servidor corretamente, o servidor deverá responder com o estado `Unauthorized`.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Agora que criou uma API segura, pode implementar um cliente que é capaz de transmitir os tokens de autenticação para a API.

## <a name="next-steps"></a>Passos Seguintes
Conforme mencionado na introdução, tem de implementar um equivalente de cliente ao ligar ao servidor que manipula a iniciar sessão, a terminar sessão e gestão de tokens. Para obter exemplos baseados em código, pode consultar as aplicações de cliente no [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) e [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Para obter um tutorial passo a passo, consulte o seguinte artigo:

> [!div class="nextstepaction"]
> [Aplicação web de node. js início de sessão e fim de sessão com o Azure AD](quickstart-v1-openid-connect-code.md)