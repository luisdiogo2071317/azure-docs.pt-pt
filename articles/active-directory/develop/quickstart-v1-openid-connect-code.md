---
title: Criar uma aplicação Web Node.js Express para início de sessão e fim de sessão com o Azure Active Directory | Microsoft Docs
description: Aprenda a criar uma aplicação Web MVC Node.js Express que se integra no Azure AD para início de sessão.
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
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 19563f76c261fda1fca53babcb553f2dceeaa345
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990098"
---
# <a name="quickstart-build-a-nodejs-express-web-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Início Rápido: Criar uma aplicação Web Node.js Express para início de sessão e fim de sessão com o Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Passport é o middleware de autenticação do Node.js. É flexível, modular e pode largá-lo discretamente em qualquer aplicação Web baseada no Express ou Restify. Um conjunto abrangente de estratégias suporta a autenticação que utiliza um nome de utilizador e uma palavra-passe, o Facebook, o Twitter e muito mais. Para o Azure Active Directory (Azure AD), vamos instalar este módulo e, em seguida, adicionar o plug-in `passport-azure-ad` do Azure AD.

Neste início rápido, vai aprender a utilizar o Passport para:

* Iniciar a sessão do utilizador na aplicação com o Azure AD.
* Apresentar informações sobre o utilizador.
* Terminar a sessão do utilizador na aplicação.

Para criar a aplicação funcional e completa, precisa de:

1. Registar uma aplicação.
2. Configurar a aplicação para utilizar a estratégia `passport-azure-ad`.
3. Utilizar o Passport para emitir pedidos de início de sessão e fim de sessão para o Azure AD.
4. Imprimir dados sobre o utilizador.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, conclua estes pré-requisitos:

* [Transfira a estrutura da aplicação como um ficheiro .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip)
  
    * Clone a estrutura:

        ```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

    O código deste início rápido é mantido [no GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS). A aplicação concluída é fornecida no final deste início rápido.

* Tenha um inquilino do Azure AD no qual possa criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino, [saiba como obter um](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-app"></a>Passo 1: Registar uma aplicação

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. No menu na parte superior da página, selecione a sua conta. Na lista **Diretório**, escolha o inquilino do Active Directory onde pretende registar a aplicação.
1. Selecione **Todos os serviços** no menu no lado esquerdo do ecrã e, em seguida, selecione **Azure Active Directory**.
1. Selecione **Registos de aplicações** e, em seguida, selecione **Adicionar**.
1. Siga as indicações para criar uma nova **Aplicação Web** e/ou **API Web**.

    * O **Nome** da aplicação descreve a sua aplicação aos utilizadores.
    * O **URL de Início de Sessão** é o URL base da sua aplicação. A predefinição da estrutura é `http://localhost:3000/auth/openid/return`.

1. Após o registo, o Azure AD atribui um ID exclusivo à sua aplicação. Este valor é necessário nas secções seguintes, por conseguinte, copie-o a partir da página da aplicação.
1. Na página **Definições > Propriedades** da aplicação, atualize o URI do ID da Aplicação. 
    
    O **URI do ID da Aplicação** é um identificador exclusivo para a sua aplicação. A convenção é utilizar o formato `https://<tenant-domain>/<app-name>`, por exemplo: `https://contoso.onmicrosoft.com/my-first-aad-app`.

1. Na página **Definições > URLs de Resposta** da sua aplicação, adicione o URL adicionado em URL de Início de Sessão no Passo 5 e selecione **Guardar**.
1. Para criar uma chave secreta, siga o passo 4 em [Para adicionar credenciais de aplicação ou permissões para aceder a APIs Web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis).

   > [!IMPORTANT]
   > Copie o valor da chave da aplicação. Este é o valor para o `clientSecret`, que precisará para o **Passo 3** abaixo. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>Passo 2: Adicionar pré-requisitos ao diretório

1. Na linha de comandos, altere os diretórios para a pasta raiz, caso ainda não exista, e execute os seguintes comandos:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

1. Também precisa de `passport-azure-ad`, por isso, execute o seguinte comando:

    * `npm install passport-azure-ad`

Esta ação instala as bibliotecas de que `passport-azure-ad` depende.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>Passo 3: Configurar a sua aplicação para utilizar a estratégia passport-node-js

Aqui, vai configurar o Express para utilizar o protocolo de autenticação OpenID Connect. O Passport é utilizado para executar várias ações, incluindo emitir pedidos de início e fim de sessão do utilizador, gerir a sessão do utilizador e obter informações sobre o utilizador.

1. Abra o ficheiro `config.js` na raiz do projeto e, em seguida, introduza os valores de configuração da aplicação na secção `exports.creds`.

    * O `clientID` é o **ID da Aplicação** atribuído à sua aplicação no portal de registo.
    * O `returnURL` é o **URL de Resposta** que introduziu no portal.
    * O `clientSecret` é o segredo que gerou no portal.

1. Em seguida, abra o ficheiro `app.js` na raiz do projeto. Adicione a chamada seguinte para invocar a estratégia `OIDCStrategy` que vem com `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

1. Depois, utilize a estratégia que acabámos de referenciar para processar os pedidos de início de sessão.

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
   O Passport utiliza um padrão semelhante para todas as estratégias (Twitter, Facebook etc.) a que todos os escritores aderem. Ao observar a estratégia, pode ver que lhe transmitimos uma função que tem um token e um done como parâmetros. A estratégia regressa depois de fazer o seu trabalho. Em seguida, queremos armazenar o utilizador e guardar o token de modo a não ser necessário voltar a pedi-lo.

   > [!IMPORTANT]
   > O código anterior aceita qualquer utilizador que se autentique no nosso servidor. Este processo é conhecido como auto-registo. Recomendamos que não deixe nenhum utilizador autenticar-se num servidor de produção sem que se registem primeiro através de um processo decidido por si. Trata-se normalmente do padrão que vê nas aplicações de consumidor, que lhe permite registar com o Facebook, mas que, em seguida, lhe pede para fornecer informações adicionais. Se a aplicação não era de exemplo, poderíamos extrair o endereço de e-mail do utilizador a partir do objeto de token devolvido e, em seguida, pedir ao utilizador para preencher informações adicionais. Dado que se trata de um servidor de teste, adicionamos os utilizadores à base de dados em memória.

1. Adicione os métodos que nos permitem controlar os utilizadores com sessão iniciada como requerido pelo Passport. Estes métodos incluem a serialização e desserialização das informações do utilizador.

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

1. Adicione o código para carregar o motor Express. Aqui, utilizamos o padrão /views e /routes predefinido fornecido pelo Express.

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

1. Por fim, adicione as rotas que entregam os pedidos reais de início de sessão ao motor `passport-azure-ad`:

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

## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Passo 4: Utilizar o Passport para emitir pedidos de início e fim de sessão para o Azure AD

A aplicação está agora corretamente configurada para comunicar com o ponto final através do protocolo de autenticação OpenID Connect. `passport-azure-ad` tem tratado de todos os detalhes das mensagens de autenticação de composição, tokens de validação do Azure AD e manutenção das sessões dos utilizadores. Tudo o que falta fazer é conceder aos seus utilizadores uma forma de iniciar e terminar sessão, e recolher informações adicionais sobre os utilizadores com sessão iniciada.

1. Adicione os métodos predefinição, início de sessão, conta e fim de sessão ao ficheiro `app.js`:

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

1. Reveja-os em detalhe:

    * A rota `/` redireciona para a vista index.ejs ao transmitir o utilizador no pedido (se existir).
    * A rota `/account` primeiro *garante que estamos autenticados* (implementamos esta definição no exemplo a seguir) e, em seguida, transmite o utilizador no pedido para que possamos obter informações adicionais sobre o mesmo.
    * A rota `/login` chama o nosso autenticador azuread-openidconnect a partir de `passport-azuread`. Se não for bem sucedido, redireciona o utilizador de volta a /login.
    * A rota `/logout` chama simplesmente logout.ejs, o que limpa os cookies e, em seguida, devolve o utilizador a index.ejs.

1. Na última parte de `app.js`, adicione o método **EnsureAuthenticated** utilizado em `/account`, conforme mostrado anteriormente.

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

1. Por fim, crie o próprio servidor em `app.js`:

    ```JavaScript
    app.listen(3000);
    ```

## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>Passo 5: Para apresentar o utilizador no site, crie as vistas e rotas no Express

Agora, `app.js` está concluído. Apenas tem de adicionar as rotas e vistas que mostram as informações obtidas ao utilizador, bem como lidar com as rotas `/logout` e `/login` que criámos.

1. Crie a rota `/routes/index.js` no diretório de raiz.

    ```JavaScript
    /*
     * GET home page.
     */

    exports.index = function(req, res){
      res.render('index', { title: 'Express' });
    };
    ```

1. Crie a rota `/routes/user.js` no diretório de raiz.

    ```JavaScript
    /*
     * GET users listing.
     */

    exports.list = function(req, res){
      res.send("respond with a resource");
    };
    ```

    Estas transmitem o pedido para as vistas, incluindo o utilizador, se estiver presente.

1. Crie a vista `/views/index.ejs` no diretório de raiz. Esta é uma página simples que chama os métodos de início e fim de sessão, e nos permite obter informações da conta. Tenha em atenção que podemos utilizar o `if (!user)` condicional como o utilizador a transmitir no pedido como prova de que temos um utilizador com sessão iniciada.

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

1. Crie a vista `/views/account.ejs` no diretório raiz, para que possamos ver informações adicionais que `passport-azure-ad` colocou no pedido de utilizador.

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
    <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Adicione um esquema para melhorar o aspeto da página. Crie a vista `/views/layout.ejs` no diretório de raiz.

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

## <a name="step-6-build-and-run-your-app"></a>Passo 6: Criar e executar a aplicação

1. Execute `node app.js` e, em seguida, aceda a `http://localhost:3000`.
1. Inicie sessão com uma conta Microsoft pessoal ou uma conta escolar ou profissional.

    Repare como a identidade do utilizador é refletida na lista /account. Tem agora uma aplicação Web protegida com protocolos de norma da indústria que podem autenticar os utilizadores com contas pessoais e profissionais/escolares.

    Para sua referência, o exemplo concluído (sem os valores de configuração) [é fornecido como um ficheiro .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/master.zip). Em alternativa, pode cloná-lo a partir do GitHub:

    ```git clone --branch master https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

## <a name="next-steps"></a>Passos seguintes

Agora, pode avançar para outros cenários:

> [!div class="nextstepaction"]
> [Proteger uma API Web com o Azure AD](quickstart-v1-nodejs-webapi.md)