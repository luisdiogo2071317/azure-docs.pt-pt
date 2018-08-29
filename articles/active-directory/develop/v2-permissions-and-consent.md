---
title: Âmbitos de v2.0 do Active Directory, permissões e consentimento do Azure | Documentos da Microsoft
description: Uma descrição de autorização, o Azure AD v2.0 ponto final, incluindo âmbitos, permissões e consentimento.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: celested
ms.reviewer: hirsin, jesakowi, justhu
ms.custom: aaddev
ms.openlocfilehash: f83ca06843b94aecf44a4e4a58959d35f00532c2
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125121"
---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Âmbitos, permissões e consentimento no ponto de final de v2.0 do Azure Active Directory

Aplicações que se integram com o Azure Active Directory (Azure AD), siga um modelo de autorização que proporciona aos utilizadores controlo sobre como uma aplicação pode aceder aos respetivos dados. A implementação da versão 2.0 do modelo de autorização foi atualizada e ele altera a forma como uma aplicação tem de interagir com o Azure AD. Este artigo abrange os conceitos básicos deste modelo de autorização, incluindo âmbitos, permissões e consentimento.

> [!NOTE]
> O ponto final v2.0 não suporta todos os cenários do Azure Active Directory e funcionalidades. Para determinar se deve utilizar o ponto final v2.0, leia sobre [v2.0 limitações](active-directory-v2-limitations.md).

## <a name="scopes-and-permissions"></a>Âmbitos e permissões

Azure AD implementa a [OAuth 2.0](active-directory-v2-protocols.md) protocolo de autorização. OAuth 2.0 é um método por meio do qual uma aplicação de terceiros pode acessar recursos hospedados na web em nome de um utilizador. Qualquer recurso hospedado na web que se integra com o Azure AD tem um identificador de recurso, ou *URI de ID de aplicação*. Por exemplo, alguns dos recursos de hospedado na web da Microsoft incluem:

* O Office 365 Unified correio API: `https://outlook.office.com`
* A API do Graph do Azure AD: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

O mesmo acontece com todos os recursos de terceiros que tem integrado com o Azure AD. Qualquer um desses recursos também pode definir um conjunto de permissões que pode ser utilizado para dividir a funcionalidade desse recurso em segmentos mais pequenos. Por exemplo, [Microsoft Graph](https://graph.microsoft.io) definiu permissões para efetuar as seguintes tarefas, entre outras:

* Leia o calendário de um utilizador
* Escrever para o calendário de um utilizador
* Enviar e-mails como se fosse um utilizador

Ao definir esses tipos de permissões, o recurso tem um controle refinado sobre os seus dados e como os dados são expostos. Uma aplicação de terceiros pode pedir estas permissões de um utilizador de aplicação. O utilizador de aplicação têm de aprovar as permissões para a aplicação pode agir em nome do usuário. Por segmentar a funcionalidade do recurso em conjuntos menores de permissão, aplicações de terceiros podem ser criadas para solicitar apenas as permissões específicas que precisam para executar a respetiva função. Os utilizadores da aplicação podem saber exatamente como uma aplicação utilizará os seus dados e pode ser mais seguro de que a aplicação não está se comportando com más intenções.

No Azure AD e são chamados de OAuth, esses tipos de permissões *âmbitos*. Eles também, por vezes, são denominados *oAuth2Permissions*. Um âmbito é representado no Azure AD como um valor de cadeia de caracteres. Continuando com o exemplo do Microsoft Graph, o valor de âmbito para cada permissão é:

* Leia o calendário de um utilizador através da utilização `Calendars.Read`
* Escrever para o calendário de um utilizador ao utilizar `Calendars.ReadWrite`
* Envie e-mails como um utilizador a utilizar por `Mail.Send`

Uma aplicação pode pedir estas permissões, especificando os âmbitos em pedidos para o ponto final v2.0.

## <a name="openid-connect-scopes"></a>Âmbitos de OpenID Connect

A implementação de v2.0 do OpenID Connect tem alguns âmbitos bem definidos que não se aplicam a um recurso específico: `openid`, `email`, `profile`, e `offline_access`.

### <a name="openid"></a>openid

Se uma aplicação realiza início de sessão usando [OpenID Connect](active-directory-v2-protocols.md), ele deve solicitar o `openid` âmbito. O `openid` âmbito mostra a página de consentimento da conta de trabalho como a permissão "Iniciar sessão" e a página de consentimento de conta pessoal Microsoft como a permissão "Visualizar seu perfil e ligar a aplicações e serviços com a sua conta Microsoft". Com esta permissão, uma aplicação pode receber um identificador exclusivo para o usuário na forma do `sub` de afirmação. Isso também permite o acesso da aplicação para o ponto final de UserInfo. O `openid` âmbito pode ser utilizado no ponto de final do token de v2.0 para adquirir os tokens de ID, que podem ser utilizados para proteger as chamadas HTTP entre os diferentes componentes de uma aplicação.

### <a name="email"></a>e-mail

O `email` âmbito pode ser utilizado com o `openid` escopo e dos outros. Fornece o acesso da aplicação para o endereço de e-mail primário do utilizador na forma do `email` de afirmação. O `email` afirmação está incluída num token apenas se um endereço de e-mail está associado com a conta de utilizador, que nem sempre é o caso. Se utiliza a `email` âmbito, a sua aplicação deve estar preparada para processar um caso em que o `email` afirmação não existe no token.

### <a name="profile"></a>perfil

O `profile` âmbito pode ser utilizado com o `openid` escopo e dos outros. Possibilita o acesso de aplicação a uma quantidade substancial de informações sobre o utilizador. As informações que ele pode acessar incluem, mas não está limitadas a, o nome de utilizador nome próprio, apelido, preferencial e ID de objeto. Para obter uma lista completa das afirmações de perfil disponíveis no parâmetro id_tokens para um utilizador específico, consulte a [v2.0 tokens referência](v2-id-and-access-tokens.md).

### <a name="offlineaccess"></a>offline_access

O [ `offline_access` âmbito](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) oferece o acesso a aplicações para recursos em nome do utilizador para um período de tempo alargado. Na página de consentimento de conta de trabalho, este âmbito é apresentado como a permissão "Aceder aos seus dados em qualquer altura". A página consentimento de conta da Microsoft pessoa, ele aparece como a permissão "Aceder às suas informações em qualquer altura". Quando um utilizador aprova o `offline_access` âmbito, a aplicação pode receber tokens de atualização do ponto final do token da versão 2.0. Tokens de atualização são vida longa. A aplicação pode obter novos tokens de acesso, como o antigo expirar.

Se a sua aplicação não solicita a `offline_access` escopo, ele não irão receber tokens de atualização. Isso significa que, quando resgatar um código de autorização no [fluxo de código de autorização de OAuth 2.0](active-directory-v2-protocols.md), irá receber um token de acesso do `/token` ponto final. O token de acesso é válido para um curto período de tempo. O token de acesso, normalmente, expira dentro de uma hora. AT que ponto, o aplicativo precisa redirecionar o usuário de volta para o `/authorize` ponto final para obter um novo código de autorização. Durante este redirecionamento, dependendo do tipo de aplicação, o utilizador poderá ter de introduzir as respetivas credenciais novamente ou novamente o consentimento a permissões.

Para obter mais informações sobre como obter e utilizar tokens de atualização, consulte a [referência do protocolo v2.0](active-directory-v2-protocols.md).

## <a name="accessing-v10-resources"></a>Aceder a recursos da versão 1.0
aplicativos v2.0 podem pedir tokens e consentimento para aplicativos de v1.0 (por exemplo, a API do Power BI `https://analysis.windows.net/powerbi/api` ou a API do Sharepoint `https://{tenant}.sharepoint.com`).  Para fazer isso, pode fazer referência a cadeia URI e o âmbito de aplicação no `scope` parâmetro.  Por exemplo, `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All` poderia solicitar o Power BI `View all Datasets` permissão para a sua aplicação. 

Para solicitar permissões vários, acrescente o URI completo com um espaço ou `+`, por exemplo, `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All+https://analysis.windows.net/powerbi/api/Report.Read.All`.  Isso solicita ambos os `View all Datasets` e `View all Reports` permissões.  Tenha em atenção que tal como acontece com todos os âmbitos de acesso do Azure AD e as permissões, aplicativos podem apenas fazer um pedido a um recurso ao mesmo tempo - pelo que a solicitação `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All+https://api.skypeforbusiness.com/Conversations.Initiate`, que solicita que tanto o Power BI `View all Datasets` permissão e o Skype para empresas `Initiate conversations` permissão, será rejeitada devido a pedidas permissões em dois recursos diferentes.  

### <a name="v10-resources-and-tenancy"></a>recursos de versões 1.0 e inquilinos
O v1.0 e v2.0 protocolos do Azure AD utilizam um `{tenant}` parâmetro incorporadas no URI (`https://login.microsoftonline.com/{tenant}/oauth2/`).  Ao utilizar o ponto final v2.0 para aceder a um recurso de v1.0 organizacional, o `common` e `consumers` inquilinos não podem ser utilizados, como esses recursos são apenas acessíveis com organizacional (Azure AD) contas.  Portanto, ao aceder a estes recursos, apenas o GUID de inquilino ou `organizations` pode ser utilizado como o `{tenant}` parâmetro.  

Se um aplicativo tentar acessar um recurso de v1.0 organizacional com um inquilino incorreto, será devolvido um erro semelhante ao seguinte. 

`AADSTS90124: Resource 'https://analysis.windows.net/powerbi/api' (Microsoft.Azure.AnalysisServices) is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.`


## <a name="requesting-individual-user-consent"></a>Pedir consentimento do utilizador individuais

Num [OpenID Connect ou do OAuth 2.0](active-directory-v2-protocols.md) pedido de autorização, uma aplicação pode pedir as permissões necessárias ao utilizar o `scope` parâmetro de consulta. Por exemplo, quando um utilizador inicia sessão a uma aplicação, a aplicação envia um pedido, como o exemplo seguinte (com quebras de linha adicionadas para melhorar a legibilidade):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

O `scope` parâmetro é uma lista separada por espaço dos âmbitos que a aplicação está a pedir. Cada âmbito é indicado ao acrescentar o valor de âmbito para o identificador do recurso (o URI de ID de aplicação). O exemplo de pedido, a aplicação tem permissão para ler o calendário do usuário e enviar correio como o utilizador.

Depois do utilizador introduz as respetivas credenciais, o ponto final v2.0 verifica a existência de um registo correspondente *consentimento do utilizador*. Se o utilizador não consentiu para qualquer um das permissões pedidas no passado, o ponto final v2.0 pede ao utilizador para conceder as permissões pedidas.

![Consentimento de conta de trabalho](./media/v2-permissions-and-consent/work_account_consent.png)

A permissão aprovada pelo usuário, o consentimento é registado para que o utilizador não tem consentimento novamente em inícios de sessão subsequentes de conta.

## <a name="requesting-consent-for-an-entire-tenant"></a>Pedir consentimento para um inquilino completo

Muitas vezes, quando uma organização compra uma subscrição para uma aplicação ou de licença, a organização pretende aprovisionar completamente o aplicativo para seus funcionários. Como parte deste processo, um administrador pode conceder o consentimento para a aplicação para agir em nome de qualquer funcionário. Se o administrador conceder consentimento para todo o inquilino, os funcionários da organização não verão uma página de consentimento para a aplicação.

Para pedir consentimento para todos os utilizadores num inquilino, a sua aplicação pode utilizar o ponto final de consentimento de administrador.

## <a name="admin-restricted-scopes"></a>Âmbitos de administração restrito

Algumas permissões de privilégio elevado no ecossistema da Microsoft podem ser definidos como *restritas por administrador*. Exemplos desses tipos de âmbitos incluem as seguintes permissões:

* Ler dados do diretório da organização ao utilizar `Directory.Read`
* Escrever dados para o diretório da organização através da utilização `Directory.ReadWrite`
* Grupos de segurança de leitura no diretório da organização através da utilização `Groups.Read.All`

Embora um utilizador de consumidor pode conceder uma aplicação de acesso para este tipo de dados, os utilizadores organizacionais são impedidos de conceder acesso para o mesmo conjunto de dados confidenciais da empresa. Se a aplicação solicite o acesso a uma destas permissões de um utilizador organizacional, o utilizador recebe uma mensagem de erro que diz que não tem autorização para dar consentimento a permissões da sua aplicação.

Se a sua aplicação necessita de acesso a âmbitos de administração restrito de mensagens em fila para as organizações, deve pedi-los também diretamente a partir de um administrador de empresa, utilizando o consentimento do ponto final de administração, descrito a seguir.

Quando um administrador concede estas permissões por meio do ponto final de consentimento de administrador, o consentimento é concedido para todos os utilizadores no inquilino.

## <a name="using-the-admin-consent-endpoint"></a>Utilizar o ponto final de consentimento de administrador

Se seguir estes passos, a aplicação pode recolher as permissões para todos os utilizadores num inquilino, incluindo os âmbitos de administração restrito. Para ver um exemplo de código que implementa os passos, consulte a [exemplo de âmbitos de administração restrito](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Solicitar as permissões no portal de registo de aplicação

1. Aceda ao seu aplicativo no [Portal de registo de aplicação](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), ou [criar uma aplicação](quickstart-v2-register-an-app.md) se ainda não o fez.
2. Localize a **permissões do Microsoft Graph** secção e, em seguida, adicione as permissões que a sua aplicação necessita.
3. **Guardar** o registo de aplicações.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Recomendado: Sessão do utilizador para a sua aplicação

Normalmente, quando cria uma aplicação que utiliza o ponto final de consentimento de administrador, a aplicação tem uma vista em que o administrador pode aprovar as permissões da aplicação ou página. Esta página pode ser parte do fluxo de inscrição da aplicação, parte integrante das definições da aplicação, ou pode ser um fluxo de "ligar" dedicada. Em muitos casos, faz sentido para a aplicação mostre isso "ligar" modo de exibição apenas depois de um utilizador ter iniciado sessão com uma empresa ou escola conta Microsoft.

Quando o utilizador inicia sessão à sua aplicação, pode identificar a organização à qual o administrador pertence antes de solicitar a aprovação as permissões necessárias. Embora não seja estritamente necessário, ele pode ajudá-lo a criar uma experiência mais intuitiva para os seus utilizadores organizacionais. Para iniciar a sessão do utilizador no, siga nosso [tutoriais de protocolo de v2.0](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Solicitar as permissões de um administrador do diretório

Quando estiver pronto para solicitar permissões de administrador da sua organização, pode redirecionar o utilizador para a versão 2.0 *ponto final de consentimento de administração*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parâmetro | Condição | Descrição |
| --- | --- | --- |
| `tenant` | Necessário | O inquilino do diretório que pretende pedir permissão do. Podem ser fornecidos no formato de nome amigável ou de GUID ou genericamente referenciado com "comum", conforme mostrado no exemplo. |
| `client_id` | Necessário | ID de aplicação que o [Portal de registo de aplicação](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) atribuído à sua aplicação. |
| `redirect_uri` | Necessário |O URI de redirecionamento do qual pretende que a resposta a serem enviados para a sua aplicação processar. Ele deve corresponder exatamente um dos URIs que registou no portal de registo de aplicação de redirecionamento. |
| `state` | Recomendado | Um valor incluído no pedido que também vai ser devolvido na resposta de token. Pode ser uma cadeia de caracteres de qualquer conteúdo que quer. Utilize o estado para codificar as informações sobre o estado do utilizador na aplicação antes do pedido de autenticação ocorreu, como a página ou a vista estivessem na. |

Neste momento, o Azure AD requer um administrador de inquilino iniciar sessão concluir o pedido. O administrador é-lhe pedido para aprovar todas as permissões que solicitou para a sua aplicação no portal de registo de aplicação.

#### <a name="successful-response"></a>Resposta com êxito

Se o administrador aprova as permissões para a sua aplicação, a resposta com êxito esta aparência:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parâmetro | Descrição |
| --- | --- | --- |
| `tenant` | O inquilino do diretório que concedidas as permissões solicitada, no formato GUID de seu aplicativo. |
| `state` | Um valor incluído no pedido que também vai ser devolvido na resposta de token. Pode ser uma cadeia de caracteres de qualquer conteúdo que quer. O estado é usado para codificar as informações sobre o estado do utilizador na aplicação antes do pedido de autenticação ocorreu, como a página ou a vista estivessem na. |
| `admin_consent` | Será definido como **true**. |

#### <a name="error-response"></a>Resposta de erro

Se o administrador não aprova as permissões para a sua aplicação, a resposta com falhas é semelhante a isto:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parâmetro | Descrição |
| --- | --- | --- |
| `error` |Uma cadeia de código de erro que pode ser utilizada para classificar tipos de erros que ocorrem e pode ser utilizada para reagir a erros. |
| `error_description` |Uma mensagem de erro específicas que pode ajudar a um desenvolvedor de identificar a causa de raiz de um erro. |

Depois que recebeu uma resposta com êxito a partir do ponto final de consentimento de administrador, a aplicação obteve as permissões que solicitou. Em seguida, pode solicitar um token para o recurso que pretende.

## <a name="using-permissions"></a>Utilizando as permissões

Depois do utilizador dá consentimento a permissões para a sua aplicação, a sua aplicação pode adquirir os tokens de acesso que representam a permissão da sua aplicação para aceder a um recurso em alguma capacidade. Um token de acesso pode ser utilizado apenas para um único recurso, mas codificado dentro do token de acesso é cada permissão que tenha sido concedida a sua aplicação para esse recurso. Para adquirir um token de acesso, a aplicação pode fazer um pedido para o token ponto final v2.0, como este:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Pode usar o token de acesso resultante em pedidos de HTTP para o recurso. Ele fiável indica ao recurso que a aplicação tem as permissões adequadas para executar uma tarefa específica. 

Para obter mais informações sobre o protocolo OAuth 2.0 e como obter os tokens de acesso, consulte a [referência de protocolo do ponto final v2.0](active-directory-v2-protocols.md).
