---
title: Azure Active Directory v 2.0 sessão-in de aplicação de web de Node.js | Microsoft Docs
description: Saiba como criar uma aplicação web Node.js que inicia sessão um utilizador através de uma conta Microsoft pessoal e uma conta escolar ou profissional.
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 04/20/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: eb16502ca255bf99c3780ff3975b6ca7f5a79993
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a>Adicionar início de sessão a uma aplicação web Node.js

> [!NOTE]
> Nem todos os cenários do Azure Active Directory e funcionalidades funcionam com o ponto final v 2.0. Para determinar se deve utilizar o ponto final v 2.0 ou o ponto final v 1.0, leia sobre [limitações de v 2.0](active-directory-v2-limitations.md).
>

Neste tutorial, utilizamos [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) para efetuar as seguintes tarefas:

* Numa aplicação web, inicie sessão no utilizador utilizando o Azure Active Directory (Azure AD) e o ponto final v 2.0.
* Apresentar informações sobre o utilizador.
* O utilizador fora da aplicação de início de sessão.

[Passport](http://passportjs.org/) é um middleware de autenticação de Node.js. Flexível e modulares, Passport pode ser removido discretamente em qualquer baseada em Express ou restify aplicação web. No Passport, um conjunto abrangente de estratégias suporta a autenticação utilizando um nome de utilizador e palavra-passe, Facebook, Twitter ou outras opções. Desenvolvemos uma estratégia para o Azure AD. Neste artigo, vamos mostrar-lhe como instalar o módulo e, em seguida, adicione o Azure AD **passport-azure-ad** Plug-in.

## <a name="download"></a>Transferência
O código deste tutorial [é mantido no GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs). Para seguir o tutorial, pode [transferir a estrutura da aplicação como um ficheiro. zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) ou clonar a estrutura:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Também pode obter a aplicação concluída no final deste tutorial.

## <a name="1-register-an-app"></a>1: registar uma aplicação
Criar uma nova aplicação em [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), ou seguir [detalhadas destes passos](active-directory-v2-app-registration.md) para registar uma aplicação. Certifique-se de que:

* Copiar o **Id da aplicação** atribuída à aplicação. É necessário para este tutorial.
* Adicionar o **Web** plataforma para a sua aplicação.
* Adicione o seguinte URL configurado na amostra `http://localhost:3000/auth/openid/return` como o **URI de redirecionamento**.

## <a name="2-prerequisities-to-run-the-sample"></a>2: Prerequisities para executar o exemplo

Terá de ter instalado para compilar e executar este exemplo de Node.js. Pode instalar Node.js de http://nodejs.org/.

Numa linha de comandos, altere os diretórios para ir para a pasta raiz, se não estiver já existe.
Execute o seguinte comando para instalar os módulos de nó pré-requisitos listados no `package.json` ficheiro:

`npm install`

Esta ação também instala as bibliotecas que `passport-azure-ad` utiliza.

## <a name="3-set-up-your-app-to-use-passport-azure-ad"></a>3: configurar a aplicação a utilizar o passport-azure-ad
Configure o middleware Express para utilizar o protocolo de autenticação OpenID Connect. Utilizar o Passport para emitir pedidos de início de sessão e fim de sessão, gerir a sessão do utilizador e obter informações sobre o utilizador, entre outros.

1.  Na raiz do projeto, abra o ficheiro de config.js. No `exports.creds` secção, introduza os valores de configuração da sua aplicação.

  * `clientID`: O **Id da aplicação** atribuída à sua aplicação no portal.
  * `returnURL`: O **URI de redirecionamento** que introduziu no portal.
  * `clientSecret`: O segredo que gerou no portal.

2.  Na raiz do projeto, abra o ficheiro app.js. A invocar OIDCStrategy vem com `passport-azure-ad`, adicione a seguinte chamada:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  Para processar os pedidos de início de sessão, utilize a estratégia de que acabou de referenciar:

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
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

O Passport utiliza um padrão semelhante para todas as respetivas estratégias (Twitter, Facebook e assim sucessivamente). Todos os escritores aderem padrão. A estratégia de passar uma `function()` que utiliza um token e `done` como parâmetros. A estratégia é devolvida após faz todos os respetivo trabalho. Armazene o utilizador e guarde o token, por isso não terá de voltar a perguntá-lo.

  > [!IMPORTANT]
  > O código anterior entra qualquer utilizador que pode autenticar-se ao seu servidor. Isto é conhecido como registo automático. Num servidor de produção, não pretender permitir que qualquer pessoa sem primeiro passá-los através de um processo de registo que escolher. Isto é, normalmente, o padrão que veem nas aplicações de consumidor. A aplicação pode permitir-lhe registar com o Facebook, mas, em seguida, pede-lhe para introduzir informações adicionais. Se não utilizar um programa da linha de comandos para este tutorial, foi possível extrair o e-mail do objeto do token que é devolvido. Em seguida, pode pedir ao utilizador para introduzir informações adicionais. Porque se trata de um servidor de teste, adicione o utilizador diretamente para a base de dados em memória.
  >
  >

4.  Adicione os métodos que utiliza para controlar os utilizadores que tem sessão iniciados, conforme requerido pelo Passport. Isto inclui as informações do utilizador de serialização e:

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  Adicione o código que carrega o motor Express. Utilize o /views predefinido e padrão de /routes Express fornece:

  ```JavaScript

  // Set up Express (section 2)

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

6.  Adicionar a publicação de rotas esse mão desativar as pedidos de início de sessão reais para o `passport-azure-ad` motor:

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-add-sign-in-and-sign-out-requests-to-azure-ad"></a>4: adicionar os pedidos de início de sessão e fim de sessão para o Azure AD
A aplicação está agora configurada para comunicar com o ponto final v 2.0, utilizando o protocolo de autenticação OpenID Connect. O `passport-azure-ad` estratégia encarrega-se de que todos os detalhes das mensagens de autenticação de composição, tokens de validação do Azure AD e manter a sessão do utilizador. Tudo o falta fazer é conceder aos seus utilizadores uma forma para iniciar sessão e terminar sessão e para recolher mais informações sobre o utilizador que tenha sessão iniciada.

1.  Adicionar o **predefinido**, **início de sessão**, **conta**, e **terminar** métodos no ficheiro app.js:

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
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  Seguem-se os detalhes:

    * O `/` rota redireciona para a vista de index.ejs. Transfere o utilizador no pedido (se existir).
    * O `/account` encaminhar primeiro *garante que sejam autenticados* (implementar que no seguinte código). Em seguida, transmite o utilizador no pedido. Isto é, pelo que pode obter mais informações sobre o utilizador.
    * O `/login` encaminhar chamadas a `azuread-openidconnect` autenticador de `passport-azuread`. Se que não for bem sucedido, o utilizador será redirecionado novamente para `/login`.
    * O `/logout` rota chama a vista de logout.ejs (e rota). Isto limpa cookies e, em seguida, devolve o utilizador novamente para index.ejs.

2.  Adicionar o **EnsureAuthenticated** método que utilizou anteriormente no `/account`:

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  App.js, crie o servidor:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express"></a>5: criar as vistas e rotas no Express
Adicione as rotas e vistas que apresentam informações ao utilizador. As rotas e vistas também processam as `/logout` e `/login` rotas que criou.

1. No diretório de raiz, crie o `/routes/index.js` rota.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  No diretório de raiz, crie o `/routes/user.js` rota.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js` e `/routes/user.js` são rotas simples transferem o pedido para as vistas, incluindo o utilizador, se estiver presente.

3.  No diretório de raiz, crie o `/views/index.ejs` vista. Esta página chama o **início de sessão** e **terminar** métodos. Também é utilizar o `/views/index.ejs` vista para capturar as informações da conta. Pode utilizar política de acesso `if (!user)` como o utilizador que está a ser transferido no pedido. É uma prova que tem um utilizador com sessão iniciado.

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  No diretório de raiz, crie o `/views/account.ejs` vista. O `/views/account.ejs` vista permite-lhe ver informações adicionais que `passport-azuread` coloca o pedido do utilizador.

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  Adicione um esquema. No diretório de raiz, crie o `/views/layout.ejs` vista.

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
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```


  ## <a name="6-build-and-run-your-app"></a>6: criar e executar a aplicação
  1. Para compilar e executar a aplicação, execute o seguinte comando do diretório de raiz de aplicações.

  `node app.js`

   Em seguida, aceda a `http://localhost:3000` no seu browser.

  2. Inicie sessão com uma conta Microsoft pessoal ou uma conta escolar ou profissional. Tenha em atenção que a identidade do utilizador é refletida no `/account` rota.

Tem agora uma aplicação web que está protegida pela utilização de protocolos padrão da indústria. Pode autenticar utilizadores na sua aplicação através das respetivas contas pessoais, profissionais ou escolares.

## <a name="next-steps"></a>Passos Seguintes
Para referência, o exemplo concluído (sem os valores de configuração) é fornecido tal como [um ficheiro. zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). Também pode cloná-lo a partir do GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Em seguida, pode passar a tópicos mais avançados. Pode querer tente:

[Proteger uma API web do Node.js utilizando o ponto final v 2.0](active-directory-v2-devquickstarts-node-api.md)

Seguem-se alguns recursos adicionais:

* [Guia para programadores do Azure AD v 2.0](active-directory-appmodel-v2-overview.md)
* [Etiqueta de "azure-active-directory" capacidade excedida da pilha](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Obter atualizações de segurança dos nossos produtos
Aconselhamo-lo para se inscrever ser notificado quando ocorrem incidentes de segurança. No [as notificações de segurança técnica Microsoft](https://technet.microsoft.com/security/dd252948) página, Subscrever alertas Advisories de segurança.
