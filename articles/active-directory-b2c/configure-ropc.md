---
title: Configurar o fluxo de credenciais de palavra-passe de proprietário do recurso no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como configurar o fluxo de credenciais de palavra-passe de proprietário do recurso no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5d68f8fe28b7f029d19a0ed0c03e5324c32f29c0
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446814"
---
# <a name="configure-the-resource-owner-password-credentials-flow-in-azure-ad-b2c"></a>Configurar o fluxo de credenciais de palavra-passe de proprietário do recurso no Azure AD B2C

O fluxo de credenciais (ROPC) de palavra-passe de proprietário do recurso é um fluxo de autenticação padrão OAuth onde o aplicativo, também conhecido como a entidade confiadora, troca de credenciais válidas, como o ID de utilizador e palavra-passe para um token de ID, o token de acesso e um token de atualização. 

> [!NOTE]
> Esta funcionalidade está em pré-visualização.

No Azure Active Directory (Azure AD) B2C, são suportadas as seguintes opções:

- **Cliente nativo**: interação do usuário durante a autenticação ocorre quando o código é executado num dispositivo do lado do usuário. O dispositivo pode ser um aplicativo móvel que está em execução num sistema operativo nativo, tal como Android, ou em execução num browser, como JavaScript.
- **Fluxo de cliente público**: apenas as credenciais de utilizador, coletadas por uma aplicação, são enviadas na chamada de API. As credenciais do aplicativo não são enviadas.
- **Adicionar nova afirmações**: conteúdo o ID de token pode ser alterado para adicionar nova afirmações. 

Não são suportados os seguintes fluxos:

- **Para-servidor**: O sistema de proteção de identidade tem um endereço IP fiável recolhido do chamador (o cliente nativo) como parte da interação. Numa chamada de API do lado do servidor, é utilizado apenas o endereço IP do servidor. Se um limiar dinâmico de autenticação falhadas é excedido, o sistema de proteção de identidade pode identificar um endereço IP repetido como um atacante.
- **Fluxo de cliente confidencial**: O ID de cliente da aplicação é validado, mas o segredo de aplicação não é validado.

##  <a name="create-a-resource-owner-policy"></a>Criar uma política de proprietário do recurso

1. Inicie sessão no portal do Azure como administrador global do inquilino do Azure AD B2C.
2. Para mudar para o inquilino do Azure AD B2C, selecione o diretório de B2C no canto superior direito do portal.
3. Sob **políticas**, selecione **políticas de proprietário do recurso**.
4. Forneça um nome para a política, como *ROPC_Auth*e, em seguida, selecione **afirmações de aplicação**.
5. Selecione as afirmações de aplicação que necessita para a sua aplicação, tal como *nome a apresentar*, *endereço de E-Mail*, e *fornecedor de identidade*.
6. Selecione **OK** e, em seguida, selecione **Criar**.

   Em seguida, verá um ponto de extremidade, como neste exemplo:

   `https://login.microsoftonline.com/yourtenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1A_ROPC_Auth`


## <a name="register-an-application"></a>Registar uma aplicação

1. Nas definições do B2C, selecione **aplicativos**e, em seguida, selecione **Add**.
2. Introduza um nome para a aplicação, tal como *ROPC_Auth_app*.
3. Selecione **não** para **API da Web/aplicação Web**e, em seguida, selecione **Sim** para **cliente nativo**.
4. Deixe todos os outros valores que são e, em seguida, selecionam **criar**.
5. Selecione a nova aplicação e tome nota do ID da aplicação para utilização posterior.

## <a name="test-the-policy"></a>Testar a política

Utilize a sua aplicação de desenvolvimento de API favorita para gerar uma chamada à API e rever a resposta para depurar a sua política. Construa uma chamada como esta com as informações na tabela seguinte como o corpo da solicitação POST:
- Substitua  *\<yourtenant.onmicrosoft.com >* com o nome do inquilino do B2C.
- Substitua  *\<B2C_1A_ROPC_Auth >* com o nome completo da sua política de credenciais de palavra-passe de proprietário do recurso.
- Substitua  *\<bef2222d56-552f-4a5b-b90a-1988a7d634c3 >* com o ID de aplicação do seu registo.

`https://login.microsoftonline.com/<yourtenant.onmicrosoft.com>/<B2C_1A_ROPC_Auth>/oauth2/v2.0/token`

| Chave | Valor |
| --- | ----- |
| o nome de utilizador | leadiocl@outlook.com |
| palavra-passe | Passxword1 |
| grant_type | palavra-passe |
| scope | openid \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > offline_access |
| client_id | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| response_type | token id_token |

*Client_id* é o valor que anotou anteriormente como a ID da aplicação. *Offline_access* é opcional, se pretender receber um token de atualização. 

O pedido POST real é semelhante ao seguinte:

```
POST /yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

username=leadiocl%40trashmail.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```


Uma resposta com êxito com acesso offline terá um aspeto semelhante ao seguinte exemplo:

```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJpc3MiOiJodHRwczovL3RlLmNwaW0ud2luZG93cy5uZXQvZjA2YzJmZTgtNzA5Zi00MDMwLTg1ZGMtMzhhNGJmZDllODJkL3YyLjAvIiwiZXhwIjoxNTEzMTMwMDc4LCJuYmYiOjE1MTMxMjY0NzgsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsIm9pZCI6IjNjM2I5NjljLThjNDktNGUxMS1hNGVmLWZkYjJmMzkyZjA0OSIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF6cCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsInZlciI6IjEuMCIsImlhdCI6MTUxMzEyNjQ3OH0.MSEThYZxCS4SevBw3-3ecnVLUkucFkehH-gH-P7SFcJ-MhsBeQEpMF1Rzu_R9kUqV3qEWKAPYCNdZ3_P4Dd3a63iG6m9TnO1Vt5SKTETuhVx3Xl5LYeA1i3Slt9Y7LIicn59hGKRZ8ddrQzkqj69j723ooy01amrXvF6zNOudh0acseszt7fbzzofyagKPerxaeTH0NgyOinLwXu0eNj_6RtF9gBfgwVidRy9OzXUJnqm1GdrS61XUqiIUtv4H04jYxDem7ek6E4jsH809uSXT0iD5_4C5bDHrpO1N6pXSasmVR9GM1XgfXA_IRLFU4Nd26CzGl1NjbhLnvli2qY4A", 
    "token_type": "Bearer", 
    "expires_in": "3600", 
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6IjEuMCJ9.aJ_2UW14dh4saWTQ0jLJ7ByQs5JzIeW_AU9Q_RVFgrrnYiPhikEc68ilvWWo8B20KTRB_s7oy_Eoh5LACsqU6Oz0Mjnh0-DxgrMblUOTAQ9dbfAT5WoLZiCBJIz4YT5OUA_RAGjhBUkqGwdWEumDExQnXIjRSeaUBmWCQHPPguV1_5wSj8aW2zIzYIMbofvpjwIATlbIZwJ7ufnLypRuq_MDbZhJkegDw10KI4MHJlJ40Ip8mCOe0XeJIDpfefiJ6WQpUq4zl06NO7j8kvDoVq9WALJIao7LYk_x9UIT-3d0W0eDBHGSRcNgtMYpymaN9ltx6djcEesXNn4CFnWG3g.y6KKeA9EcsW9zW-g.TrTSgn4WBt18gezegxihBla9SLSTC3YfDROQsL9K4yX4400FKlTlf-2l9CnpGTEdWXVi7sIMHCl8S4oUiXd-rvY2mn_NfDrbbVJfgKp1j7Nnq9FFyeJEFcP_FtUXgsNTG9iwfzWox04B1d845qNRWiS9N8BhAAAIdz5N0ChHuOxsVOC0Y_Ly3DNe-JQyXcq964M6-jp3cgi4UqMxT837L6pLY5Ih_iPsSfyHzstsFeqyUIktnzt1MpTlyW-_GDyFK1S-SyV8PPQ7phgFouw2jho1iboHX70RlDGYyVmP1CfQzKE_zWxj3rgaCZvYMWN8fUenoiatzhvWkUM7dhqKGjofPeL8rOMkhl6afLLjObzhUg3PZFcMR6guLjQdEwQFufWxGjfpvaHycZSKeWu6-7dF8Hy_nyMLLdBpUkdrXPob_5gRiaH72KvncSIFvJLqhY3NgXO05Fy87PORjggXwYkhWh4FgQZBIYD6h0CSk2nfFjR9uD9EKiBBWSBZj814S_Jdw6HESFtn91thpvU3hi3qNOi1m41gg1vt5Kh35A5AyDY1J7a9i_lN4B7e_pknXlVX6Z-Z2BYZvwAU7KLKsy5a99p9FX0lg6QweDzhukXrB4wgfKvVRTo.mjk92wMk-zUSrzuuuXPVeg", 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJleHAiOjE1MTMxMzAwNzgsIm5iZiI6MTUxMzEyNjQ3OCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly90ZS5jcGltLndpbmRvd3MubmV0L2YwNmMyZmU4LTcwOWYtNDAzMC04NWRjLTM4YTRiZmQ5ZTgyZC92Mi4wLyIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsImFjciI6ImIyY18xYV9yZXNvdXJjZW93bmVydjIiLCJpYXQiOjE1MTMxMjY0NzgsImF1dGhfdGltZSI6MTUxMzEyNjQ3OCwib2lkIjoiM2MzYjk2OWMtOGM0OS00ZTExLWE0ZWYtZmRiMmYzOTJmMDQ5IiwiYXRfaGFzaCI6Ikd6QUNCTVJtcklwYm9OdkFtNHhMWEEifQ.iAJg13cgySsdH3cmoEPGZB_g-4O8KWvGr6W5VzRXtkrlLoKB1pl4hL6f_0xOrxnQwj2sUgW-wjsCVzMc_dkHSwd9QFZ4EYJEJbi1LMGk2lW-PgjsbwHPDU1mz-SR1PeqqJgvOqrzXo0YHXr-e07M4v4Tko-i_OYcrdJzj4Bkv7ZZilsSj62lNig4HkxTIWi5Ec2gD79bPKzgCtIww1KRnwmrlnCOrMFYNj-0T3lTDcXAQog63MOacf7OuRVUC5k_IdseigeMSscrYrNrH28s3r0JoqDhNUTewuw1jx0X6gdqQWZKOLJ7OF_EJMP-BkRTixBGK5eW2YeUUEVQxsFlUg" 
} 
```

## <a name="redeem-a-refresh-token"></a>Utilizar um token de atualização

Construa uma chamada POST, como o mostrado aqui com as informações na tabela seguinte como o corpo do pedido:

`https://login.microsoftonline.com/<yourtenant.onmicrosoft.com>/<B2C_1A_ROPC_Auth>/oauth2/v2.0/token`

| Chave | Valor |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| Recurso | \<bef2222d56-552f-4a5b-b90a-1988a7d634c3 > |
| refresh_token | eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3... |

*Client_id* e *recurso* são os valores que apontou anteriormente como a ID da aplicação. *Refresh_token* é o token que recebeu na chamada de autenticação mencionada anteriormente.

## <a name="implement-with-your-preferred-native-sdk-or-use-app-auth"></a>Implementar com o SDK nativo preferencial ou utilizar a autenticação da aplicação

A implementação do Azure AD B2C atende aos padrões de OAuth 2.0 para credenciais de palavra-passe do proprietário de recursos do cliente público e deve ser compatível com a maioria dos SDKs de cliente. Nós o Testamos este fluxo extensivamente, na produção, com AppAuth para iOS e AppAuth para Android. Para obter as informações mais recentes, consulte [SDK da aplicação nativa para OAuth 2.0 e OpenID Connect implementando práticas recomendadas modernos](https://appauth.io/).

Baixe amostras de trabalho que tenham sido configuradas para utilização com o Azure AD B2C a partir do GitHub, [para Android](https://aka.ms/aadb2cappauthropc) e [para iOS](https://aka.ms/aadb2ciosappauthropc).




