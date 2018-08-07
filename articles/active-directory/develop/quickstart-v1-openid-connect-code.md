---
title: Aplicação web do Azure node. js do AD, introdução | Documentos da Microsoft
description: Saiba como criar uma aplicação web MVC de Express node. js que se integra com o Azure AD para início de sessão.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 04/20/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 91cb7df9d38432d660930c21f6a9d0d64215d6e6
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577444"
---
# <a name="azure-ad-nodejs-web-app-getting-started"></a>Aplicação web do Azure node. js do AD, introdução
Aqui usamos Passport para:

* Sessão do utilizador para a aplicação no Azure Active Directory (Azure AD).
* Apresentar informações sobre o utilizador.
* Iniciar a sessão do utilizador fora da aplicação.

Passport é middleware de autenticação para node. js. Flexível e modular, Passport pode ser discretamente soltos para qualquer baseada em Express ou restify aplicação web. Um conjunto abrangente de estratégias suporta a autenticação que utiliza um nome de utilizador e palavra-passe, Facebook, Twitter e muito mais. Desenvolvemos uma estratégia para o Microsoft Azure Active Directory. Podemos instalar este módulo e, em seguida, adicione o Microsoft Azure Active Directory `passport-azure-ad` Plug-in.

Para tal, siga os passos seguintes:

1. Registe uma aplicação.
2. Configure a sua aplicação para utilizar o `passport-azure-ad` estratégia.
3. Utilizar o Passport para emitir pedidos de início de sessão e fim de sessão para o Azure AD.
4. Dados sobre o utilizador de impressão.

O código deste tutorial [é mantido no GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS). Para acompanhar, pode [transferir a estrutura da aplicação como um ficheiro. zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) ou clonar a estrutura:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

A aplicação concluída é fornecida no final deste tutorial também.

## <a name="step-1-register-an-app"></a>Passo 1: Registar uma aplicação
1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No menu na parte superior da página, selecione a sua conta. Sob o **Directory** lista, escolha o inquilino do Active Directory onde pretende registar a sua aplicação.

3. Selecione **todos os serviços** no menu no lado esquerdo do ecrã e, em seguida, selecione **Azure Active Directory**.

4. Selecione **registos de aplicações**e, em seguida, selecione **Add**.

5. Siga as instruções para criar uma **aplicação Web** e/ou **WebAPI**.
  * O **nome** do aplicativo, descreva a aplicação aos utilizadores.

  * O **URL de início de sessão** é o URL base da sua aplicação. Predefinição a estrutura é `http://localhost:3000/auth/openid/return`.

6. Depois de registar, do Azure AD atribui a aplicação um ID de aplicação único. Este valor é necessário nas secções seguintes, por isso, copie-o partir da página de aplicativo.
7. Partir do **definições** -> **propriedades** página para a sua aplicação, atualize o URI de ID de aplicação. O **URI de ID de aplicação** é um identificador exclusivo para a sua aplicação. A Convenção é utilizar o formato `https://<tenant-domain>/<app-name>`, por exemplo: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Partir do **definições** -> **URLs de resposta** página para a sua aplicação, adicione o URL, adicionado no URL de início de sessão do passo 5 e clique em Guardar.

9. Para criar uma chave secreta, siga o passo 4 [para adicionar credenciais de aplicativo ou permissões para aceder a web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis).

   > [!IMPORTANT]
   > Copie o valor da chave de aplicação. Este é o valor para o `clientSecret`, que precisará para **passo 3** abaixo. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>Passo 2: Adicionar pré-requisitos ao diretório
1. A linha de comandos, altere os diretórios para a pasta raiz se não ainda exista, e, em seguida, execute os seguintes comandos:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Além disso, precisa `passport-azure-ad`:
    * `npm install passport-azure-ad`

Esta ação instala as bibliotecas que `passport-azure-ad` depende.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Passo 3: Configure a sua aplicação para utilizar a estratégia passport-nó-js
Aqui, podemos configurar o Express para utilizar o protocolo de autenticação OpenID Connect. Passport for utilizado para executar várias ações, incluindo pedidos de início de sessão e fim de sessão do problema, gerir a sessão do utilizador e obter informações sobre o utilizador.

1. Para começar, abra a `config.js` arquivo na raiz do projeto e, em seguida, introduza os valores de configuração da sua aplicação a `exports.creds` secção.

  * O `clientID` é o **Id da aplicação** que é atribuído à sua aplicação no portal de registo.

  * O `returnURL` é o **URL de resposta** que introduziu no portal.

  * O `clientSecret` é o segredo que gerou no portal.

2. Em seguida, abra o `app.js` ficheiro na raiz do projeto. Em seguida, adicione a seguinte chamada para invocar a `OIDCStrategy` estratégia que vem com `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Depois disso, utilize a estratégia que acabou de referenciar para processar os nossos pedidos de início de sessão.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
   O Passport utiliza um padrão semelhante para todas as respetivas estratégias (Twitter, Facebook etc.) que todos os escritores aderem. Observando a estratégia, verá que passamos a ele uma função que tem um token e um concluído como os parâmetros. A estratégia é-nos depois de seu trabalho. Em seguida, queremos armazenar o utilizador e stash o token, pelo que não temos de pedi-lo.

   > [!IMPORTANT]
   > O código anterior aceita qualquer utilizador que ocorre autenticar com o nosso servidor. Isso é conhecido como registo automático. Recomendamos que não permitiremos que qualquer utilizador autenticado num servidor de produção sem primeiro passá-los, registe-se através de um processo que decidir qual. Isto é, normalmente, o padrão normal que vê nas aplicações de consumidor, o que lhe permitem registar com o Facebook, mas, em seguida, a pedir-lhe para fornecer informações adicionais. Se isso não fosse um aplicativo de exemplo, poderíamos extrair o endereço de e-mail do utilizador do objeto do token que é devolvido e, em seguida, pedir ao utilizador para preencher informações adicionais. Como se trata de um servidor de teste, podemos adicioná-los para a base de dados na memória.


4. Em seguida, vamos adicionar os métodos que permitem-nos controlar os utilizadores com sessão iniciada como requerido pelo Passport. Estes métodos incluem serializando e desserializando as informações do utilizador.

    ```JavaScript
    // Passport session setup. (Section 2)

    //   To support persistent sign-in sessions, Passport needs to be able to
    //   serialize users into the session and deserialize them out of the session. Typically,
    //   this is done simply by storing the user ID when serializing and finding
    //   the user by ID when deserializing.
    passport.serializeUser(function(user, done) {
        done(null, user.email);
    });

    passport.deserializeUser(function(id, done) {
        findByEmail(id, function (err, user) {
            done(err, user);
        });
    });

    // array to hold signed-in users
    var users = [];

    var findByEmail = function(email, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            log.info('we are using user: ', user);
            if (user.email === email) {
                return fn(null, user);
            }
        }
        return fn(null, null);
    };
    ```

5. Em seguida, vamos adicionar o código para carregar o motor Express. Aqui usamos o /views padrão e padrão de /routes Express fornece.

    ```JavaScript
    // configure Express (section 2)

        var app = express();
        app.configure(function() {
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.logger());
      app.use(express.methodOverride());
      app.use(cookieParser());
      app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
      app.use(bodyParser.urlencoded({ extended : true }));
      // Initialize Passport!  Also use passport.session() middleware, to support
      // persistent login sessions (recommended).
      app.use(passport.initialize());
      app.use(passport.session());
      app.use(app.router);
      app.use(express.static(__dirname + '/../../public'));
    });
    ```

6. Por fim, vamos adicionar as rotas que entregam os pedidos de início de sessão reais para o `passport-azure-ad` mecanismo:

    ```JavaScript
    // Our Auth routes (section 3)

    // GET /auth/openid
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. The first step in OpenID authentication involves redirecting
    //   the user to their OpenID provider. After authenticating, the OpenID
    //   provider redirects the user back to this application at
    //   /auth/openid/return.
    app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
        log.info('Authentication was called in the Sample');
        res.redirect('/');
    });

    // GET /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.get('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });

    // POST /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.post('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });
    ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Passo 4: Utilizar Passport para emitir pedidos de início de sessão e fim de sessão para o Azure AD
A aplicação está agora corretamente configurada para comunicar com o ponto final, utilizando o protocolo de autenticação OpenID Connect. `passport-azure-ad` tem Tratado de todos os detalhes de mensagens de autenticação de composição, tokens de validação do Azure AD e manutenção de sessões de utilizador. Tudo o que permanece é fornecer aos utilizadores uma forma de iniciar sessão e terminar sessão e coleta de informações adicionais sobre os utilizadores com sessão iniciada.

1. Em primeiro lugar, vamos adicionar o padrão, início de sessão, conta e fim de sessão métodos para nosso `app.js` ficheiro:

    ```JavaScript
    //Routes (section 4)

    app.get('/', function(req, res){
      res.render('index', { user: req.user });
    });

    app.get('/account', ensureAuthenticated, function(req, res){
      res.render('account', { user: req.user });
    });

    app.get('/login',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('Login was called in the Sample');
        res.redirect('/');
    });

    app.get('/logout', function(req, res){
      req.logout();
      res.redirect('/');
    });
    ```

2. Vamos rever em detalhe:

  * O `/`rota redireciona para a vista de index.ejs, passando o utilizador no pedido (se existir).
  * O `/account` encaminhar primeiro *garante que são autenticados* (implementamos que no exemplo a seguir) e, em seguida, transmite o utilizador no pedido para que podemos obter informações adicionais sobre o utilizador.
  * O `/login` rota chama nosso authenticator azuread openidconnect de `passport-azuread`. Se o que não for bem sucedido, redirecionará o usuário voltar ao /login.
  * O `/logout` encaminhar simplesmente chamadas a logout.ejs (e a rota), que limpa cookies e, em seguida, devolve o utilizador ao index.ejs.

3. Para a última parte da `app.js`, vamos adicionar a **EnsureAuthenticated** método que é utilizado na `/account`, conforme mostrado anteriormente.

    ```JavaScript
    // Simple route middleware to ensure user is authenticated. (section 4)

    //   Use this route middleware on any resource that needs to be protected. If
    //   the request is authenticated (typically via a persistent sign-in session),
    //   the request proceeds. Otherwise, the user is redirected to the
    //   sign-in page.
    function ensureAuthenticated(req, res, next) {
      if (req.isAuthenticated()) { return next(); }
      res.redirect('/login')
    }
    ```

4. Por fim, vamos criar o próprio servidor no `app.js`:

```JavaScript
app.listen(3000);
```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Passo 5: Para apresentar o nosso utilizador no Web site, criar as vistas e rotas no Express
Agora `app.js` estiver concluída. Precisamos simplesmente de adicionar as rotas e vistas que apresentam as informações que podemos obter ao usuário, bem como lidar com o `/logout` e `/login` rotas que criamos.

1. Crie a rota `/routes/index.js` no diretório de raiz.

    ```JavaScript
    /*
     * GET home page.
     */

    exports.index = function(req, res){
      res.render('index', { title: 'Express' });
    };
    ```

2. Crie a rota `/routes/user.js` no diretório de raiz.

    ```JavaScript
    /*
     * GET users listing.
     */

    exports.list = function(req, res){
      res.send("respond with a resource");
    };
    ```

 Estes passam o pedido para as exibições, incluindo o utilizador se estiver presente.

3. Crie a vista `/views/index.ejs` no diretório de raiz. Esta é uma página simples que chama nossos métodos de início de sessão e fim de sessão e permite-nos obter informações da conta. Tenha em atenção que podemos utilizar política de acesso `if (!user)` como o utilizador que está a ser transferido no pedido é evidências que temos um utilizador com sessão iniciada.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. Criar a `/views/account.ejs` visualizado sob o diretório de raiz, de modo que podemos exibir informações adicionais que `passport-azure-ad` colocou no pedido de utilizador.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. Vamos tornar essa análise boa ao adicionar um layout. Crie a vista `/views/layout.ejs` no diretório de raiz.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

## <a name="next-steps"></a>Passos Seguintes
Por fim, crie e execute a sua aplicação. Execute `node app.js`e, em seguida, aceda a `http://localhost:3000`.

Inicie sessão com uma conta Microsoft pessoal ou uma conta escolar ou profissional e repare como a identidade do utilizador é refletida na lista /account. Tem agora uma aplicação web que está protegida com protocolos padrão da indústria que podem autenticar utilizadores com ambas as suas contas pessoais e de trabalho/escola.

Para sua referência, o exemplo concluído (sem os valores de configuração) [é fornecido como um ficheiro .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Em alternativa, pode clonar a partir do GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Agora pode passar para tópicos mais avançados. Talvez queira experimentar:

[Proteger uma Web API com o Azure AD](quickstart-v1-nodejs-webapi.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
