---
title: Personalizar autenticação e autorização no serviço de aplicações do Azure | Documentos da Microsoft
description: Mostra como personalizar a autenticação e autorização no serviço de aplicações e obter afirmações de utilizador e tokens diferentes.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2018
ms.author: cephalin
ms.openlocfilehash: 191d42f43e500c7f8041a02aeba2fbcb7dfd5379
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226531"
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Personalizar autenticação e autorização no serviço de aplicações do Azure

Este artigo mostra-lhe como personalizar [autenticação e autorização no serviço de aplicações](app-service-authentication-overview.md)e para gerir a identidade da sua aplicação. 

Para começar rapidamente a utilizar, consulte um dos seguintes tutoriais:

* [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no serviço de aplicações do Azure (Windows)](app-service-web-tutorial-auth-aad.md)
* [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no serviço de aplicações do Azure para Linux](containers/tutorial-auth-aad.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão da conta Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Configurar várias opções de início de sessão

A configuração do portal não oferece uma forma chave na mão para apresentar as várias opções de início de sessão a seus usuários (por exemplo, Facebook e Twitter). No entanto, não é difícil adicionar a funcionalidade à sua aplicação web. Os passos estão delineados do seguinte modo:

Primeiro, na **autenticação / autorização** página no portal do Azure, configure cada fornecedor de identidade que pretende ativar.

Na **ação a tomar quando o pedido não é autenticado**, selecione **pedidos permitir anónimos (sem ação)**.

Na página de início de sessão, ou a barra de navegação ou qualquer outro local da sua aplicação web, adicione uma ligação de início de sessão para cada um dos fornecedores ativou (`/.auth/login/<provider>`). Por exemplo:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Quando o usuário clica em um dos links, é aberta a respectiva página de início de sessão para iniciar a sessão do utilizador.

Para redirecionar o utilizador pós-sessão-para um URL personalizado, utilize o `post_login_redirect_url` (não deve ser confundido com o URI de redirecionamento na sua configuração de fornecedor de identidade) de parâmetro de cadeia de caracteres de consulta. Por exemplo, para direcionar o utilizador para `/Home/Index` após o início de sessão, utilize o seguinte código HTML:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="access-user-claims"></a>Afirmações de utilizador de acesso

Serviço de aplicações passa afirmações de utilizador à sua aplicação ao utilizar cabeçalhos especiais. Solicitações externas não são permitidas para definir esses cabeçalhos, para que estejam presentes apenas se for definido pelo serviço de aplicações. Alguns cabeçalhos de exemplo incluem:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

O código escrito em qualquer linguagem ou arquitetura pode obter as informações que necessita desses cabeçalhos. Para aplicações ASP.NET 4.6, o **ClaimsPrincipal** é definida automaticamente com os valores apropriados.

Seu aplicativo também pode obter detalhes adicionais sobre o utilizador autenticado ao chamar `/.auth/me`. O servidor de aplicações móveis SDKs fornecem métodos auxiliares para trabalhar com estes dados. Para obter mais informações, consulte [como utilizar o Azure Mobile Apps node. js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), e [trabalhar com o SDK do servidor de back-end de .NET para aplicações móveis do Azure](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Obter tokens no código da aplicação

A partir do código de servidor, os tokens de específica do fornecedor serão inseridos o cabeçalho do pedido, para que pode acessá-los facilmente. A tabela seguinte mostra os nomes de cabeçalho de token possíveis:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Token do Facebook | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Conta Microsoft | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

A partir do código de cliente (por exemplo, uma aplicação móvel ou JavaScript no browser), enviar um HTTP `GET` pedido para `/.auth/me`. O JSON devolvido tem os tokens de específica do fornecedor.

> [!NOTE]
> São tokens de acesso para aceder a recursos de fornecedor, para que estejam presentes apenas se configurar o seu fornecedor com um segredo do cliente. Para ver como obter tokens de atualização, consulte [tokens de acesso de atualização](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Tokens de acesso de atualização

Quando o token de acesso do seu fornecedor expira, terá de autenticar o utilizador. Pode evitar a expiração do token, fazendo uma `GET` chamar para o `/.auth/refresh` ponto final do seu aplicativo. Quando chamado, o serviço de aplicações atualiza automaticamente os tokens de acesso no arquivo de tokens para o usuário autenticado. Pedidos subsequentes para tokens pelo seu código de aplicação obtém os tokens de atualização. No entanto, para a atualização de token funcionar, o arquivo de tokens tem de conter [tokens de atualização](https://auth0.com/learn/refresh-tokens/) para o seu fornecedor. A forma de obter tokens de atualização estão documentados pelo cada fornecedor, mas a lista seguinte é um breve resumo:

- **Google**: acrescentar uma `access_type=offline` consultar o parâmetro de cadeia de caracteres para seu `/.auth/login/google` chamada à API. Se utilizar o SDK de aplicações móveis, pode adicionar o parâmetro para um da `LogicAsync` sobrecargas (consulte [Tokens de atualização do Google](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: não fornece os tokens de atualização. Tokens de longa duração expirarem em 60 dias (consulte [expiração do Facebook e extensão de Tokens de acesso](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: os tokens de acesso não expiram (consulte [Twitter FAQ de OAuth](https://developer.twitter.com/en/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft Account**: quando [configurar as definições de autenticação de conta Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md), selecione o `wl.offline_access` âmbito.
- **O Azure Active Directory**: na [ https://resources.azure.com ](https://resources.azure.com), siga os passos abaixo:
    1. Na parte superior da página, selecione **leitura/escrita**.
    1. No navegador à esquerda, navegue até **subscrições** > **_\<subscrição\_nome_**   >  **resourceGroups** > _**\<recursos\_grupo\_nome >**_   >  **provedores** > **Microsoft. Web** > **sites** > _**\<aplicação \_name >**_ > **config** > **authsettings**. 
    1. Clique em **Editar**.
    1. Modifique a seguinte propriedade. Substitua  _\<aplicação\_id >_ com o ID da aplicação do Azure Active Directory do serviço pretende aceder.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Clique em **colocar**. 

Assim que o fornecedor está configurado, pode [encontrar o token de atualização e a hora de expiração para o token de acesso](#retrieve-tokens-in-app-code) no arquivo de tokens. 

Para atualizar o token de acesso a qualquer altura, basta chamar `/.auth/refresh` em qualquer idioma. O fragmento seguinte utiliza o jQuery para atualizar seus tokens de acesso de um cliente JavaScript.

```JavaScript
function refreshTokens() {
  var refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Se um utilizador revoga as permissões concedidas à sua aplicação, a chamada para `/.auth/me` poderá falhar com um `403 Forbidden` resposta. Para diagnosticar erros, verifique os registos da aplicação para obter detalhes.

## <a name="extend-session-expiration-grace-period"></a>Estender o período de tolerância de expiração de sessão

Depois de uma sessão autenticada expira, existe um período de tolerância de 72 horas por predefinição. Durante este período de tolerância, tem permissão para atualizar o cookie de sessão ou o token de sessão com o serviço de aplicações sem reautenticar o utilizador. Pode simplesmente chamar `/.auth/refresh` quando o cookie de sessão ou o token de sessão se torna inválido e não precisa de controlar a expiração do token por conta própria. Após o período de tolerância de 72 horas encerraram, o utilizador deve iniciar sessão novamente para obter um token de sessão ou o cookie de sessão válido.

Se 72 horas não estiver tempo suficiente para, pode estender esta janela de expiração. Estendendo a expiração durante um longo período pode ter implicações de segurança significativos (por exemplo, quando um token de autenticação for perdido ou roubado). Por isso, deve deixá-lo com a predefinição 72 horas ou definir o período de extensão para o valor mais baixo.

Para expandir a janela de expiração predefinida, execute o seguinte comando [Cloud Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> O período de tolerância só se aplica à sessão do serviço de aplicações que authenticated, não os tokens de fornecedores de identidade. Não há nenhum período de cortesia para os tokens expirados fornecedor. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Limite do domínio de contas de início de sessão

Account da Microsoft e do Azure Active Directory permite-lhe iniciar sessão a partir de vários domínios. Por exemplo, permite que a Microsoft Account _outlook.com_, _live.com_, e _hotmail.com_ contas. O Azure Active Directory permite que qualquer número de domínios personalizados para as contas de início de sessão. Esse comportamento pode ser indesejável para uma aplicação interna, que não pretende qualquer pessoa com uma _outlook.com_ para acesso da conta. Para limitar o nome de domínio das contas de início de sessão, siga estes passos.

Na [ https://resources.azure.com ](https://resources.azure.com), navegue até à **subscrições** > **_\<subscrição\_nome_**   >  **resourceGroups** > _**\<recursos\_grupo\_nome >**_   >  **fornecedores** > **Microsoft. Web** > **sites**  >    _**\<app\_nome >**_ > **config** > **authsettings**. 

Clique em **edite**, modifique a seguinte propriedade e, em seguida, clique em **colocar**. Não se esqueça de substituir  _\<domínio\_name >_ com o domínio que pretende.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto (Windows)](app-service-web-tutorial-auth-aad.md)
> [Tutorial: autenticar e autorizar utilizadores ponto-a-ponto (Linux)](containers/tutorial-auth-aad.md)