---
title: Fluxo de código de autorização de OAuth de v 2.0 do Azure AD | Microsoft Docs
description: Criar aplicações web utilizando a implementação do Azure AD do protocolo de autenticação OAuth 2.0.
services: active-directory
documentationcenter: ''
author: hpsin
manager: mtillman
editor: ''
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: hirsin
ms.custom: aaddev
ms.openlocfilehash: 070f5c0f159c3a3fd5148655acf33982288f5353
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Protocolos de v 2.0 - fluxo de código de autorização do OAuth 2.0
A concessão de código de autorização do OAuth 2.0 pode ser utilizada nas aplicações que são instaladas num dispositivo para obter acesso a recursos protegidos, como as APIs web.  Utilizar a implementação de aplicação modelo v 2.0 OAuth 2.0, pode adicionar a iniciar sessão e API aceder às suas aplicações de ambiente de trabalho e móveis.  Este guia é independente de idioma e descreve como enviar e receber mensagens HTTP sem utilizar qualquer um do [bibliotecas de autenticação de open source do Azure](active-directory-authentication-libraries.md).

> [!NOTE]
> Nem todos os cenários do Azure Active Directory e funcionalidades são suportadas pelo ponto final v 2.0.  Para determinar se deve utilizar o ponto final v 2.0, leia sobre [limitações de v 2.0](active-directory-v2-limitations.md).
> 
> 

O fluxo de código de autorização do OAuth 2.0 está descrito em [secção 4.1 da especificação de OAuth 2.0](http://tools.ietf.org/html/rfc6749).  É utilizado para efetuar a autenticação e autorização na maioria dos tipos de aplicação, incluindo [aplicações web](active-directory-v2-flows.md#web-apps) e [instalado nativamente aplicações](active-directory-v2-flows.md#mobile-and-native-apps).  O fluxo permite que aplicações de forma segura adquirir access_tokens que podem ser utilizadas para aceder a recursos protegidos pelo ponto final v 2.0.  

## <a name="protocol-diagram"></a>Diagrama de protocolo
Um nível elevado, o fluxo de autenticação completa para uma aplicação nativa/mobile um pouco este aspeto:

![Fluxo de código de autenticação do OAuth](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Pedir um código de autorização
O fluxo de código de autorização começa com o cliente instruir o utilizador a `/authorize` ponto final.  Este pedido, o cliente indica as permissões que necessita de adquirir do utilizador:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Clique na hiperligação abaixo para executar este pedido! Depois de iniciarem sessão, o browser deve ser redirecionado para `https://localhost/myapp/` com um `code` na barra de endereço.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parâmetro             |             | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inquilino                | Necessário    | O `{tenant}` valor no caminho do pedido pode ser utilizado para controlar quem pode iniciar sessão na aplicação.  Os valores permitidos são `common`, `organizations`, `consumers`e inquilinos identificadores.  Para obter mais detalhes, consulte [Noções básicas de protocolo](active-directory-v2-protocols.md#endpoints).                                                                                                                                                                                                                                                                                                                      |
| client_id             | Necessário    | ID de aplicação que o portal de registo ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) atribuído a sua aplicação.    |
| response_type         | Necessário    | Tem de incluir `code` para que o fluxo de código de autorização.         |
| redirect_uri          | recomendado | Redirect_uri da sua aplicação, onde as respostas de autenticação podem ser enviadas e recebidas pela sua aplicação.  Este deve corresponder exatamente um dos redirect_uris registado no portal, exceto tem de ser codificados de url.  Para aplicações móveis & nativas, deve utilizar o valor predefinido de `https://login.microsoftonline.com/common/oauth2/nativeclient`.     |
| scope                 | Necessário    | Uma lista separada por espaço de [âmbitos](active-directory-v2-scopes.md) que pretende que o utilizador para autorizar.             |
| response_mode         | recomendado | Especifica o método que deve ser utilizado para enviar a cópia de segurança de token resultante para a sua aplicação.  Pode ser `query` ou `form_post`.     |
| state                 | recomendado | Um valor incluído no pedido de que também vai ser devolvido na resposta token.  Pode ser uma cadeia de todos os conteúdos que pretende.  Um valor exclusivo gerado aleatoriamente é normalmente utilizado para [impedir ataques de falsificação de pedidos entre sites](http://tools.ietf.org/html/rfc6749#section-10.12).  O valor pode também codificar informações sobre o estado do utilizador na aplicação, antes de ocorrer o pedido de autenticação, tais como a página ou a vista estivessem nas suas.  |
| linha de comandos                | opcional    | Indica o tipo de interação do utilizador que é necessário.  Os únicos valores válidos neste momento, são 'início de sessão', 'none' e 'consentimento'.  `prompt=login` irá forçar o utilizador introduza as suas credenciais nesse pedido, a negação início de sessão único.  `prompt=none` é o oposto --irá garantir que o utilizador não é apresentado com qualquer linha de comandos interativa contratutal.  Se o pedido não é possível concluir automaticamente através de início de sessão único, o ponto final v 2.0 irá devolver um `interaction_required` erro.  `prompt=consent` aciona a caixa de diálogo de consentimento do OAuth depois do utilizador inicia sessão, solicitando ao utilizador para conceder permissões para a aplicação. |
| login_hint            | opcional    | Pode ser utilizado para a pré-preencher o campo de endereço de e-mail/nome de utilizador da página de início de sessão do utilizador, se souber o nome de utilizador antecedência.  Muitas vezes, as aplicações irão utilizar este parâmetro durante a reautenticação, já ter extraiu o nome de utilizador de uma anterior início de sessão utilizando o `preferred_username` de afirmação.                                                                                                                                                                                                                                                                                                      |
| domain_hint           | opcional    | Pode ser um dos `consumers` ou `organizations`.  Caso, irá ignorar o processo de deteção baseada em e-mail esse utilizador passa através de v 2.0-na página sessão, originando uma experiência de utilizador ligeiramente mais simplificada.  Aplicações, muitas vezes, irão utilizar este parâmetro durante a reautenticação, extraindo o `tid` de um anterior início de sessão.  Se o `tid` é o valor de afirmação `9188040d-6c67-4c5b-b112-36a304b66dad`, deve utilizar `domain_hint=consumers`.  Caso contrário, utilize `domain_hint=organizations`.                                                                                                                |
| code_challenge_method | opcional    | O método utilizado para codificar o `code_verifier` para o `code_challenge` parâmetro. Pode ser um dos `plain` ou `S256`.  Se excluído, `code_challenge` é prestada texto simples se `code_challenge` está incluído.  Azure AAD v 2.0 suporta `plain` e `S256`. Para obter mais informações, consulte o [PKCE RFC](https://tools.ietf.org/html/rfc7636). |
| code_challenge        | opcional    | Utilizado para proteger o código de autorização concede através de chave de prova de código Exchange (PKCE) de um cliente nativo. Se necessário `code_challenge_method` está incluído.  Para obter mais informações, consulte o [PKCE RFC](https://tools.ietf.org/html/rfc7636).                                                                                                                                                                                                                                                                                                                                                                      |

Neste momento, o utilizador será ser-lhe pedido para introduzir as suas credenciais e concluir a autenticação.  O ponto final v 2.0 também vai assegurar que o utilizador consentiu as permissões indicadas a `scope` parâmetro de consulta.  Se o utilizador não se consentiu para qualquer um dessas permissões, pedirá ao utilizador consentimento para as permissões necessárias.  Detalhes de [permissões, consentimento e aplicações de multi-inquilinos são fornecidas aqui](active-directory-v2-scopes.md).

Depois do utilizador efetua a autenticação e atribui o consentimento, o ponto final v 2.0 irá devolver uma resposta a sua aplicação no indicados `redirect_uri`, utilizando o método especificado no `response_mode` parâmetro.

#### <a name="successful-response"></a>Resposta com êxito
Uma resposta com êxito utilizando `response_mode=query` parece ser:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parâmetro | Descrição                                                                                                                                                                                                                        |
|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| código      | Authorization_code que a aplicação pedida. A aplicação pode utilizar o código de autorização para pedir um token de acesso para o recurso de destino.  Authorization_codes são muito suma lived, normalmente, estas expiram após cerca de 10 minutos. |
| state     | Se um parâmetro de estado está incluído no pedido, o mesmo valor deve aparecer na resposta. A aplicação deverá certificar-se de que os valores de estado no pedido e resposta são idênticos.                                              |

#### <a name="error-response"></a>Resposta de erro
As respostas de erro também podem ser enviadas para o `redirect_uri` para a aplicação pode processar corretamente:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parâmetro         | Descrição                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| erro             | Uma cadeia de código de erro que pode ser utilizada para classificar os tipos de erros ocorridos e pode ser utilizada para reagir a erros. |
| error_description | Uma mensagem de erro específicas que pode ajudar um programador identificar a causa de raiz de um erro de autenticação.            |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Códigos de erro para erros de ponto final de autorização
A tabela seguinte descreve os vários códigos de erro que podem ser devolvidos no `error` parâmetro da resposta de erro.

| Código de Erro                | Descrição                                                                                                           | Ação de cliente                                                                                                                                                                                                                               |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| invalid_request           | Erro de protocolo, como um parâmetro necessário em falta.                                                                 | Corrija e submeta novamente o pedido. Este é um erro de desenvolvimento, normalmente, detetado durante o teste inicial.                                                                                                                                       |
| unauthorized_client       | A aplicação de cliente não é permitida para pedir um código de autorização.                                             | Normalmente, este erro ocorre quando a aplicação de cliente não está registada no Azure AD ou não é adicionada ao inquilino do Azure AD do utilizador. A aplicação pode pedir ao utilizador com instruções para instalar a aplicação e adicioná-lo para o Azure AD. |
| access_denied             | Proprietário do recurso negado consentimento                                                                                         | A aplicação cliente pode notificar o utilizador que não é possível continuar a menos que o utilizador permitir.                                                                                                                                                 |
| unsupported_response_type | O servidor de autorização não suporta o tipo de resposta no pedido.                                           | Corrija e submeta novamente o pedido. Este é um desenvolvimento erro normalmente é detetado durante o teste inicial.                                                                                                                                       |
| server_error              | O servidor encontrou um erro inesperado.                                                                           | Repita o pedido. Estes erros poderá resultar de condições temporárias. A aplicação cliente pode explicar ao utilizador que a respetiva resposta está atrasada a um erro temporário.                                                                  |
| temporarily_unavailable   | O servidor estiver temporariamente demasiado ocupado para processar o pedido.                                                             | Repita o pedido. A aplicação cliente pode explicar ao utilizador que a respetiva resposta está atrasada devido a uma condição temporária.                                                                                                                 |
| invalid_resource          | O recurso de destino é inválido porque não existe, do Azure AD não é possível encontrá-lo ou não está corretamente configurado. | Este erro indica que o recurso, se existir, não foi configurado no inquilino. A aplicação pode pedir ao utilizador com instruções para instalar a aplicação e adicioná-lo para o Azure AD.                                            |
|login_required             | Demasiados ou não foram localizados utilizadores | O cliente pediu autenticação automática (`prompt=none`), mas não foi encontrado um único utilizador.  Isto pode significar que existem vários utilizadores Active Directory a sessão ou nenhum utilizador.  Isto leva em consideração o inquilino escolhido (por exemplo, se existirem 2 contas do AAD Active Directory e uma MSA, e `consumers` é escolhido, autenticação automática será funciona). |
|interaction_required       | O pedido requer interação do utilizador.  | É necessário um passo de autenticação adicional ou consentimento. Repita o pedido sem `prompt=none`.  |

## <a name="request-an-access-token"></a>Pedir um token de acesso
Agora que já tiver adquirido uma authorization_code e tenha sido concedida permissão pelo utilizador, pode resgatar a `code` para um `access_token` para o recurso pretendido. Fazê-lo através do envio de um `POST` pedido para o `/token` ponto final:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Tente executar este pedido na Postman! (Não se esqueça de substituir o `code`) [ ![executar no Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parâmetro     |                       | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inquilino        | Necessário              | O `{tenant}` valor no caminho do pedido pode ser utilizado para controlar quem pode iniciar sessão na aplicação.  Os valores permitidos são `common`, `organizations`, `consumers`e inquilinos identificadores.  Para obter mais detalhes, consulte [Noções básicas de protocolo](active-directory-v2-protocols.md#endpoints).                                                                                                                                                     |
| client_id     | Necessário              | ID de aplicação que o portal de registo ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) atribuído a sua aplicação.                                                                                                                                                                                                                               |
| grant_type    | Necessário              | Tem de ser `authorization_code` para que o fluxo de código de autorização.                                                                                                                                                                                                                                                                                                                                                                              |
| scope         | Necessário              | Uma lista separada por espaço de âmbitos.  Os âmbitos de pedido nesta parte tem de ser equivalente ao ou um subconjunto dos âmbitos solicitada na primeira fase.  Se os âmbitos especificados neste pedido span vários servidores de recursos, o ponto final v 2.0 irá devolver um token para o recurso especificado no âmbito do primeiro.  Para obter uma explicação mais detalhada dos âmbitos, consulte [permissões, consentimento e dos âmbitos](active-directory-v2-scopes.md). |
| código          | Necessário              | Authorization_code que obteve na primeira parte do fluxo.                                                                                                                                                                                                                                                                                                                                                                     |
| redirect_uri  | Necessário              | O valor de redirect_uri mesmo que utilizou para adquirir o authorization_code.                                                                                                                                                                                                                                                                                                                                                               |
| client_secret | necessário para aplicações web | O segredo de aplicação que criou no portal de registo de aplicação para a sua aplicação.  Não deve ser utilizado numa aplicação nativa, porque client_secrets não pode ser fiável armazenado nos dispositivos.  É necessário para as aplicações web e APIs, que têm a capacidade de armazenar o client_secret de forma segura no lado do servidor web.                                                                                                                        |
| code_verifier | opcional              | O mesmo code_verifier que foi utilizado para obter o authorization_code.  Necessário se PKCE foi utilizada no pedido de concessão do código de autorização.  Para obter mais informações, consulte o [PKCE RFC](https://tools.ietf.org/html/rfc7636)                                                                                                                                                                                                                                                                                             |
#### <a name="successful-response"></a>Resposta com êxito
Uma resposta de token com êxito irá ter o seguinte aspeto:

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
| access_token  | O token de pedido de acesso. A aplicação pode utilizar este token para autenticar para o recurso seguro, tais como uma API web.                                                                                                                                                                                                                                                                                                                                      |
| token_type    | Indica o valor de tipo de token. O único tipo que suporte do Azure AD é portador                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Quanto o token de acesso é válido (em segundos).                                                                                                                                                                                                                                                                                                                                                                                                         |
| scope         | Os âmbitos que o access_token é válido para.                                                                                                                                                                                                                                                                                                                                                                                                           |
| refresh_token | Um token de atualização de OAuth 2.0. A aplicação pode utilizar este token adquirir tokens de acesso adicionais depois do token de acesso atual expira.  Refresh_tokens são longa duração e pode ser utilizado para manter o acesso a recursos para períodos de tempo prolongados.  Para obter mais detalhes, consulte o [referência de token de v 2.0](active-directory-v2-tokens.md). <br> **Nota:** apenas fornecido se `offline_access` âmbito foi pedido.                                                 |
| id_token      | Um não assinados JSON Web tokens (JWT). Base64Url de pode aplicação descodificar os segmentos do token pedido informações sobre o utilizador que tem sessão iniciada. A aplicação pode colocar em cache os valores e apresentá-los, mas não deverá confiar nos mesmos para qualquer autorização ou limites de segurança.  Para mais informações sobre id_tokens, consulte o [referência de token de ponto final v 2.0](active-directory-v2-tokens.md). <br> **Nota:** apenas fornecido se `openid` âmbito foi pedido. |
#### <a name="error-response"></a>Resposta de erro
As respostas de erro terá um aspeto semelhante:

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parâmetro         | Descrição                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| erro             | Uma cadeia de código de erro que pode ser utilizada para classificar os tipos de erros ocorridos e pode ser utilizada para reagir a erros. |
| error_description | Uma mensagem de erro específicas que pode ajudar um programador identificar a causa de raiz de um erro de autenticação.            |
| error_codes       | Uma lista de códigos de erro de STS específicas que podem ajudar no diagnóstico.                                                  |
| carimbo de data/hora         | A hora em que ocorreu o erro.                                                                             |
| trace_id          | Um identificador exclusivo para o pedido que pode ajudar no diagnóstico.                                                 |
| correlation_id    | Um identificador exclusivo para o pedido que pode ajudar no diagnóstico componentes.                               |

#### <a name="error-codes-for-token-endpoint-errors"></a>Códigos de erro para erros de ponto final de tokens
| Código de Erro              | Descrição                                                                                                           | Ação de cliente                                                                                                                                                                                                                               |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| invalid_request         | Erro de protocolo, como um parâmetro necessário em falta.                                                                 | Corrija e submeta novamente o pedido                                                                                                                                                                                                                |
| invalid_grant           | O código de autorização ou o Verificador de código PKCE é inválido ou expirou.                                               | Tente um novo pedido para o `/authorize` endpoint e certifique-se de que o parâmetro code_verifier estava correto.                                                                                                                                     |
| unauthorized_client     | O cliente autenticado não está autorizado a utilizar este tipo de conceder de autorização.                                      | Normalmente, isto ocorre quando a aplicação de cliente não está registada no Azure AD ou não é adicionada ao inquilino do Azure AD do utilizador. A aplicação pode pedir ao utilizador com instruções para instalar a aplicação e adicioná-lo para o Azure AD. |
| invalid_client          | Falha na autenticação de cliente.                                                                                         | As credenciais do cliente não são válidas. Para corrigir, o administrador da aplicação atualiza as credenciais.                                                                                                                                        |
| unsupported_grant_type  | O servidor de autorização não suporta o tipo de conceder de autorização.                                               | Altere o tipo de concessão no pedido. Este tipo de erro deve ocorrem apenas durante o desenvolvimento e ser detetado durante o teste inicial.                                                                                                       |
| invalid_resource        | O recurso de destino é inválido porque não existe, do Azure AD não é possível encontrá-lo ou não está corretamente configurado. | Isto indica que o recurso, se existir, não foi configurado no inquilino. A aplicação pode pedir ao utilizador com instruções para instalar a aplicação e adicioná-lo para o Azure AD.                                            |
| interaction_required    | O pedido requer interação do utilizador. Por exemplo, um passo de autenticação adicional é necessário.                    | Repita o pedido com o mesmo recurso.                                                                                                                                                                                                   |
| temporarily_unavailable | O servidor estiver temporariamente demasiado ocupado para processar o pedido.                                                             | Repita o pedido. A aplicação cliente pode explicar ao utilizador que a respetiva resposta está atrasada devido a uma condição temporária.                                                                                                                 |

## <a name="use-the-access-token"></a>Utilizar o token de acesso
Agora que já obteve com êxito um `access_token`, pode utilizar o token de pedidos a APIs da Web, incluindo-na `Authorization` cabeçalho:

> [!TIP]
> Execute este pedido na Postman! (Substituir a `Authorization` cabeçalho primeiro) [ ![executar no Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Atualizar o token de acesso
São curtos Access_tokens lived e deve atualizá-las após expirarem para continuar a aceder a recursos.  Pode fazê-lo ao submeter outro `POST` pedido para o `/token` ponto final, desta vez desde que o `refresh_token` em vez do `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Tente executar este pedido na Postman! (Não se esqueça de substituir o `refresh_token`) [ ![executar no Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parâmetro     |                       | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inquilino        | Necessário              | O `{tenant}` valor no caminho do pedido pode ser utilizado para controlar quem pode iniciar sessão na aplicação.  Os valores permitidos são `common`, `organizations`, `consumers`e inquilinos identificadores.  Para obter mais detalhes, consulte [Noções básicas de protocolo](active-directory-v2-protocols.md#endpoints).                                                                                                                                                                                   |
| client_id     | Necessário              | ID de aplicação que o portal de registo ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) atribuído a sua aplicação.                                                                                                                                                                                                                                                             |
| grant_type    | Necessário              | Tem de ser `refresh_token` para esta parte do fluxo de código de autorização.                                                                                                                                                                                                                                                                                                                                                                                                     |
| scope         | Necessário              | Uma lista separada por espaço de âmbitos.  Os âmbitos de pedido nesta parte tem de ser equivalente ao ou um subconjunto dos âmbitos solicitada na fase de pedido de authorization_code original.  Se os âmbitos especificados neste pedido span vários servidores de recursos, o ponto final v 2.0 irá devolver um token para o recurso especificado no âmbito do primeiro.  Para obter uma explicação mais detalhada dos âmbitos, consulte [permissões, consentimento e dos âmbitos](active-directory-v2-scopes.md). |
| refresh_token | Necessário              | Refresh_token que obteve na segunda parte do fluxo.                                                                                                                                                                                                                                                                                                                                                                                                       |
| redirect_uri  | Necessário              | O valor de redirect_uri mesmo que utilizou para adquirir o authorization_code.                                                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | necessário para aplicações web | O segredo de aplicação que criou no portal de registo de aplicação para a sua aplicação.  Não deve ser utilizado numa aplicação nativa, porque client_secrets não pode ser fiável armazenado nos dispositivos.  É necessário para as aplicações web e APIs, que têm a capacidade de armazenar o client_secret de forma segura no lado do servidor web.                                                                                                                                                     |

#### <a name="successful-response"></a>Resposta com êxito
Uma resposta de token com êxito irá ter o seguinte aspeto:

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
| access_token  | O token de pedido de acesso. A aplicação pode utilizar este token para autenticar para o recurso seguro, tais como uma API web.                                                                                                                                                                                                                                                                                                                                      |
| token_type    | Indica o valor de tipo de token. O único tipo que suporte do Azure AD é portador                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Quanto o token de acesso é válido (em segundos).                                                                                                                                                                                                                                                                                                                                                                                                         |
| scope         | Os âmbitos que o access_token é válido para.                                                                                                                                                                                                                                                                                                                                                                                                           |
| refresh_token | Um novo token de atualização de OAuth 2.0. Deve substituir o token de atualização antigo por este token de atualização recentemente adquiridas para garantir que os tokens de atualização mantêm-se válidas para, desde que possível. <br> **Nota:** apenas fornecido se `offline_access` âmbito foi pedido.                                                                                                                                                                                                 |
| id_token      | Um não assinados JSON Web tokens (JWT). Base64Url de pode aplicação descodificar os segmentos do token pedido informações sobre o utilizador que tem sessão iniciada. A aplicação pode colocar em cache os valores e apresentá-los, mas não deverá confiar nos mesmos para qualquer autorização ou limites de segurança.  Para mais informações sobre id_tokens, consulte o [referência de token de ponto final v 2.0](active-directory-v2-tokens.md). <br> **Nota:** apenas fornecido se `openid` âmbito foi pedido. |

#### <a name="error-response"></a>Resposta de erro

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parâmetro         | Descrição                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| erro             | Uma cadeia de código de erro que pode ser utilizada para classificar os tipos de erros ocorridos e pode ser utilizada para reagir a erros. |
| error_description | Uma mensagem de erro específicas que pode ajudar um programador identificar a causa de raiz de um erro de autenticação.            |
| error_codes |Uma lista de códigos de erro de STS específicas que podem ajudar no diagnóstico. |
| carimbo de data/hora |A hora em que ocorreu o erro. |
| trace_id |Um identificador exclusivo para o pedido que pode ajudar no diagnóstico. |
| correlation_id |Um identificador exclusivo para o pedido que pode ajudar no diagnóstico componentes. |

Para obter uma descrição dos códigos de erro e a ação de cliente recomendada, consulte [códigos de erro para erros de ponto final de tokens](#error-codes-for-token-endpoint-errors).
