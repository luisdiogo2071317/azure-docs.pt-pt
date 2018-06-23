---
title: Personalizar a autenticação e autorização no serviço de aplicações do Azure | Microsoft Docs
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
ms.openlocfilehash: 688ea090384755b9a6d60a4968d958678edc27ad
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337857"
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Personalizar a autenticação e autorização no serviço de aplicações do Azure

Este artigo mostra como personalizar [autenticação e autorização no serviço de aplicações](app-service-authentication-overview.md)e gerir a identidade da sua aplicação. 

Para começar a trabalhar rapidamente, consulte um dos seguintes tutoriais:

* [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no App Service do Azure (Windows)](app-service-web-tutorial-auth-aad.md)
* [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no App Service do Azure para Linux](containers/tutorial-auth-aad.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão da conta Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Como configurar a sua aplicação para utilizar o início de sessão do Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Configurar várias opções de início de sessão

A configuração do portal não oferece uma forma de ativar a chave para apresentar as opções de início de sessão várias aos seus utilizadores (tais como o Facebook e Twitter). No entanto, não se encontra difícil adicionar a funcionalidade à sua aplicação web. Os passos estão delineados do seguinte modo:

Primeiro, no **autenticação / autorização** página no portal do Azure, configure cada fornecedor de identidade que pretende ativar.

No **ação a tomar quando o pedido não é autenticado**, selecione **pedidos Permitir anónimo (nenhuma ação)**.

Na página de início de sessão, ou a barra de navegação ou qualquer outra localização da sua aplicação web, adicionar uma ligação de início de sessão para cada um dos fornecedores é ativado (`/.auth/login/<provider>`). Por exemplo:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Quando o utilizador clica em uma das ligações, a respetiva página de início de sessão é aberto a sessão do utilizador.

## <a name="access-user-claims"></a>Afirmações de utilizador de acesso

Serviço de aplicações transmite afirmações de utilizador à sua aplicação através da utilização de cabeçalhos especiais. Não são permitidos pedidos externos ao definir estes cabeçalhos, para que estejam presentes apenas se for definido pelo serviço de aplicações. Alguns cabeçalhos de exemplo incluem:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Código que é escrito em qualquer linguagem ou arquitetura pode obter as informações que necessita destes cabeçalhos. Para aplicações ASP.NET 4.6, o **ClaimsPrincipal** é automaticamente definido com os valores adequados.

A aplicação também poderá obter detalhes adicionais sobre o utilizador autenticado ao chamar `/.auth/me`. O servidor de aplicações móveis SDKs fornecem métodos de programa auxiliar para trabalhar com estes dados. Para obter mais informações, consulte [como utilizar o Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), e [trabalhar com o servidor de back-end .NET SDK de Mobile Apps do Azure](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Obter tokens no código da aplicação

A partir do seu código de servidor, os tokens específica do fornecedor são inseridos no cabeçalho do pedido, para que possa aceder-lhes facilmente. A tabela seguinte mostra os nomes de cabeçalho de token possíveis:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Token do Facebook | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Conta Microsoft | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

A partir do seu código de cliente (por exemplo, uma aplicação móvel ou JavaScript no browser), enviar um HTTP `GET` pedido para `/.auth/me`. O JSON devolvido tem os tokens específica do fornecedor.

> [!NOTE]
> Os tokens de acesso são para aceder a recursos de fornecedor, para que estejam presentes apenas se configurar o seu fornecedor com um segredo do cliente. Para ver como obter os tokens de atualização, consulte [atualização de tokens de acesso](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Atualize os tokens de acesso

Quando o token de acesso do fornecedor expira, terá de reautenticação do utilizador. Pode evitar a expiração do token ao tornar um `GET` a chamada para o `/.auth/refresh` ponto final da sua aplicação. Quando chamado, o serviço de aplicações atualiza automaticamente os tokens de acesso no arquivo de token para o utilizador autenticado. Os pedidos subsequentes para os tokens pelo seu código de aplicação obter os tokens de atualizados. No entanto, para atualização do token funcione, o arquivo de tokens tem de conter [tokens de atualização](https://auth0.com/learn/refresh-tokens/) para o seu fornecedor. A forma como para obter os tokens de atualização estão documentados pelo cada fornecedor, mas a lista seguinte é um breve resumo:

- **Google**: acrescentar um `access_type=offline` consultar o parâmetro de cadeia para o `/.auth/login/google` chamada à API. Se utilizar o SDK de aplicações móveis, pode adicionar o parâmetro para um do `LogicAsync` sobrecargas (consulte [Google atualizar Tokens](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: não fornecerem tokens de atualização. Tokens de longa duração expirarem em 60 dias (consulte [expiração Facebook e extensão de Tokens de acesso de](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: tokens de acesso não expirarem (consulte [Twitter FAQ de OAuth](https://developer.twitter.com/en/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft Account**: quando [configurar definições de autenticação de conta Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md), selecione o `wl.offline_access` âmbito.
- **Azure Active Directory**: no [ https://resources.azure.com ](https://resources.azure.com), efetue os seguintes passos:
    1. Na parte superior da página, selecione **leitura/escrita**.
    1. In the left browser, navigate to **subscriptions** > **_\<subscription\_name_** > **resourceGroups** > _**\<resource\_group\_name>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<app\_name>**_ > **config** > **authsettings**. 
    1. Clique em **Editar**.
    1. Modifique a propriedade seguinte. Substitua  _\<aplicação\_id >_ com o ID de aplicação do Azure Active Directory do serviço pretende aceder.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Clique em **colocar**. 

Depois do fornecedor está configurado, pode [encontrar o token de atualização e a hora de expiração para o token de acesso](#retrieve-tokens-in-app-code) no arquivo de token. 

Para atualizar o seu token de acesso em qualquer altura, chame somente `/.auth/refresh` em qualquer idioma. O fragmento seguinte utiliza jQuery para atualizar os tokens de acesso de um cliente de JavaScript.

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

Se um utilizador revoga as permissões concedidas à sua aplicação, a chamada para `/.auth/me` poderá falhar com um `403 Forbidden` resposta. Para diagnosticar erros, verifique os registos de aplicação para obter mais detalhes.

## <a name="extend-session-expiration-grace-period"></a>Expandir o período de tolerância de expiração de sessão

Depois de uma sessão autenticada expira, há um período de tolerância de 72 horas por predefinição. Durante este período de tolerância, que está autorizado a atualizar o cookie de sessão ou um token de sessão com o serviço de aplicações sem reautenticar o utilizador. Apenas pode chamar `/.auth/refresh` quando o cookie de sessão ou token da sessão fica inválido e não precisa de controlar a expiração do token por si. Após o período de tolerância de 72 horas encerraram, o utilizador deve iniciar sessão no novamente para obter um token de sessão ou o cookie de sessão válido.

Se 72 horas não tempo suficiente para si, pode expandir este período de expiração. Expandir a expiração durante um longo período pode ter implicações de segurança significativos (tais como quando um token de autenticação é divulgado ou for roubado). Por isso, deve mantiver a predefinição 72 horas ou defina o período de extensão para o menor valor.

Para expandir o período de expiração predefinido, execute o seguinte comando [nuvem Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> O período de tolerância só se aplica à sessão autenticado do serviço de aplicações, não os tokens de fornecedores de identidade. Não há nenhum período de tolerância para os tokens de fornecedor expirado. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Limitar o domínio de contas de início de sessão

Azure Active Directory e Account Microsoft permite-lhe iniciar sessão a partir de vários domínios. Por exemplo, permite Account Microsoft _outlook.com_, _live.com_, e _hotmail.com_ contas. Azure Active Directory permite que qualquer número de domínios personalizados para as contas de início de sessão. Este comportamento poderá ser indesejável para uma aplicação interna, o que não pretende que qualquer pessoa com uma _outlook.com_ conta para o acesso. Para limitar o nome de domínio das contas de início de sessão, siga estes passos.

In [https://resources.azure.com](https://resources.azure.com), navigate to **subscriptions** > **_\<subscription\_name_** > **resourceGroups** > _**\<resource\_group\_name>**_ > **providers** > **Microsoft.Web** > **sites** > _**\<app\_name>**_ > **config** > **authsettings**. 

Clique em **editar**, modifique a propriedade seguinte e, em seguida, clique em **colocar**. Não se esqueça de substituir  _\<domínio\_nome >_ com o domínio que pretende.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto (Windows)](app-service-web-tutorial-auth-aad.md)
> [Tutorial: autenticar e autorizar utilizadores ponto-a-ponto (Linux)](containers/tutorial-auth-aad.md)