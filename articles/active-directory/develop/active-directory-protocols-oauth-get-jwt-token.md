---
title: Autenticar com o Azure AD e obter um Token JWT com OAuth 2.0
description: Código de exemplo que mostra como autenticar com o Azure Active Directory com o OAuth 2.0 para acessar aplicativos de web seguro e APIs web na sua organização.
services: active-directory
author: rloutlaw
manager: angerobe
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: routlaw
ms.custom: aaddev
ms.openlocfilehash: cbc86eb6d13034fb3154ed8e9d021064f1d15ece
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265791"
---
# <a name="request-an-access-token-using-oauth-20-to-access-web-apis-and-applications-secured-by-azure-active-directory"></a>Pedir um token de acesso com o OAuth 2.0 para APIs da web do acesso e de aplicativos a serem protegidos pelo Azure Active Directory

Este artigo mostra como obter um JSON Web Token (JWT) para aceder aos recursos protegidos pelo Azure AD. Parte do princípio que tem um [token de autorização](/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code) a partir de permissão concedido ao utilizador ou através uma [principal de serviço](/azure/active-directory/develop/active-directory-application-objects).

## <a name="build-the-request"></a>Criar o pedido

Utilize o seguinte `POST` pedido HTTP para obter um JWT para aceder a uma aplicação específica ou a API protegida pelo Azure AD.

```http
POST https://{tenant}/oauth2/v2.0/token?client_id={client-id}
&scope={scope}
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh   
```

### <a name="request-headers"></a>Cabeçalhos do pedido

Os seguintes cabeçalhos são necessários:

|Cabeçalho do pedido|Descrição|  
|--------------------|-----------------|  
| *Anfitrião:* | https://login.microsoftonline.com |
| *Tipo de conteúdo:*| aplicação/x-www-form-urlencoded |
 

### <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro     |                       | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inquilino        | Necessário              | O `{tenant}` valor no caminho do pedido pode ser utilizado para controlar quem pode iniciar sessão na aplicação. Os valores permitidos são `common`, `organizations`, `consumers`e identificadores de inquilinos. Para obter mais detalhes, consulte [Noções básicas de protocolo](active-directory-v2-protocols.md#endpoints).                                                                                                                                                   |
| client_id     | Necessário              | ID de aplicação que o portal de registo ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) atribuído a sua aplicação.                                                                                                                                                                                                                             |
| grant_type    | Necessário              | Tem de ser `authorization_code` para o fluxo de código de autorização.                                                                                                                                                                                                                                                                                                                                                                            |
| scope         | Necessário              | Uma lista de âmbitos separadas por espaços. Os âmbitos solicitados neste leg tem de ser equivalente a ou um subconjunto dos âmbitos na chamada para `/authorize`. Se os âmbitos especificados neste pedido abrangem vários servidores de recursos, o ponto final v2.0 irá devolver um token para o recurso especificado no âmbito da primeira. Para obter uma explicação mais detalhada de âmbitos, consulte [permissões e consentimento e âmbitos](active-directory-v2-scopes.md). |
| Código          | Necessário              | O authorization_code que obteve na chamada para `/authorize`                                                                                                                                                                                                                                                                                                                                                                |
| redirect_uri  | Necessário              | O valor de redirect_uri mesmo que foi utilizado para adquirir o authorization_code.                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | necessária para as aplicações web | O segredo de aplicação que criou no portal de registo de aplicação para a sua aplicação. Não utilize num aplicativo nativo, porque client_secrets não podem ser armazenados com confiança nos dispositivos. É necessário para aplicações web e APIs, que têm a capacidade de armazenar o client_secret em segurança no lado do servidor web.  Segredos do cliente tem de ser codificados de URL antes de serem enviados.                                                                                 |
| code_verifier | opcional              | O code_verifier mesmo que foi utilizado para obter o authorization_code. Necessário se PKCE foi utilizada no pedido de concessão do código de autorização. Para obter mais informações, consulte o [PKCE RFC](https://tools.ietf.org/html/rfc7636)                                                                                                                                                                                                                                                                                             |
## <a name="handle-the-response"></a>Tratar da resposta

Uma resposta de token com êxito irá conter um token JWT e terá a seguinte aparência:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parâmetro     | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| access_token  | O token de acesso solicitado. A aplicação pode utilizar este token para autenticar para o recurso protegido, como uma API web.                                                                                                                                                                                                                                                                                                                                    |
| token_type    | Indica o valor de tipo de token. O único tipo que o Azure AD suporta é portador                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | O tempo que o token de acesso é válido (em segundos).                                                                                                                                                                                                                                                                                                                                                                                                       |
| scope         | Os âmbitos que o access_token é válido para.                                                                                                                                                                                                                                                                                                                                                                                                         |
| refresh_token | Um token de atualização de OAuth 2.0. A aplicação pode utilizar este token adquirir os tokens de acesso adicionais depois do token de acesso atual expira. Refresh_tokens são vida longa e pode ser utilizado para manter o acesso aos recursos por longos períodos de tempo. Para obter mais detalhes, consulte a [referência de token de v2.0](active-directory-v2-tokens.md). <br> **Nota:** só será fornecido se `offline_access` âmbito foi pedido.                                               |
| id_token      | Um não assinados JSON Web Token (JWT). A aplicação pode decodificar os segmentos deste token solicite informações sobre o utilizador que iniciou sessão. A aplicação pode armazenar em cache os valores e exibi-los, mas não deverá confiar nos mesmos para qualquer autorização ou limites de segurança. Para obter mais informações sobre id_tokens, consulte a [referência de token de ponto final de v2.0](active-directory-v2-tokens.md). <br> **Nota:** só será fornecido se `openid` âmbito foi pedido. |



