---
title: Pedir tokens de acesso no Azure Active Directory B2C | Documentos da Microsoft
description: Este artigo irá mostrar como configurar uma aplicação de cliente e adquirir um token de acesso.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/09/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f3db56c7ce61960fca0e5347b2385bcc65a88354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845151"
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Pedir tokens de acesso

Um token de acesso (denotado como **acesso\_token** nas respostas do Azure AD B2C) é uma forma de token de segurança que um cliente pode utilizar para aceder a recursos que está protegido por um [o servidor de autorização](active-directory-b2c-reference-protocols.md), por exemplo, uma API web. Tokens de acesso são representados como [JWTs](active-directory-b2c-reference-tokens.md) e contêm informações sobre o servidor de recurso pretendido e as permissões concedidas para o servidor. Ao chamar o servidor de recursos, o token de acesso tem de estar presente na solicitação HTTP.

Este artigo aborda como configurar uma aplicação cliente e a API web para obter um **acesso\_token**.

> [!NOTE]
> **Cadeias de API da Web (em-nome-de) não é suportado pelo Azure AD B2C.**
>
> Muitas arquiteturas incluem uma API web que precisa chamar outra API web a jusante, ambas protegidas pelo Azure AD B2C. Este cenário é comum em clientes nativos que têm um web API back-end, que por sua vez chama um serviço online da Microsoft, como o Azure AD Graph API.
>
> Este cenário de API web em cadeia pode ser suportado utilizando a concessão de credencial de portador do OAuth 2.0 JWT, também conhecida como o fluxo em-nome-de. No entanto, o fluxo em-nome-de atualmente não é implementado no Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Registar uma API web e publicar as permissões

Antes de solicitar um token de acesso, tem primeiro de registar uma API web e publicar as permissões (âmbitos) que podem ser concedidas à aplicação cliente.

### <a name="register-a-web-api"></a>Registar uma API Web

1. No menu de recursos do Azure AD B2C no portal do Azure, clique em **aplicativos**.
2. Clique em **+ adicionar** na parte superior do menu.
3. Introduza um **Nome** para a aplicação que irá descrever a aplicação para os consumidores. Por exemplo, pode introduzir "Contoso API".
4. Coloque o comutador **Incluir aplicação Web/API Web** em **Sim**.
5. Introduza um valor arbitrário para o **URLs de resposta**. Por exemplo, introduza `https://localhost:44316/`. O valor não é relevante, uma vez que uma API deve não receber o token diretamente a partir do Azure AD B2C.
6. Introduza um **URI de ID de Aplicação**. Este é o identificador utilizado para a API Web. Por exemplo, introduza "notas" na caixa. O **URI de ID de aplicação** seria `https://{tenantName}.onmicrosoft.com/notes`.
7. Clique em **Criar** para registar a aplicação.
8. Clique na aplicação que acabou de criar e copie a **ID de Aplicação de Cliente** exclusiva global que utilizará posteriormente no seu código.

### <a name="publishing-permissions"></a>Permissões de publicação

Os escopos, que são análogos às permissões, são necessários quando a sua aplicação está a chamar uma API. Alguns exemplos de âmbitos são de "leitura" ou "escrita". Suponha que deseja que a web ou aplicação nativa "ler" a partir de uma API. A aplicação deverá chamar o Azure AD B2C e solicitar um token de acesso que dá acesso ao âmbito de "leitura". Para o Azure AD B2C emitir um token de acesso, a aplicação precisa ser concedida a permissão "ler" a partir da API específica. Para fazer isso, sua API tem primeiro de publicar o âmbito "ler".

1. No Azure AD B2C **aplicativos** menu, abra a API web de aplicação ("Contoso API").
2. Clique em **Âmbitos publicados**. Esta é a secção onde pode definir as permissões (âmbitos) que podem ser concedidas a outras aplicações.
3. Adicione **valores de âmbito** conforme necessário (por exemplo, "ler"). Por predefinição, será definido o âmbito "user_impersonation". Pode ignorar esta se desejar. Introduza uma descrição do âmbito na **nome do âmbito** coluna.
4. Clique em **Guardar**.

> [!IMPORTANT]
> O **nome do âmbito** é a descrição dos **o valor de âmbito**. Ao usar o escopo, certifique-se de utilizar o **valor de âmbito**.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Conceder um nativo ou web permissões de aplicações para uma API web

Depois de uma API estiver configurada para publicar âmbitos, a aplicação de cliente tem de ser concedido esses âmbitos através do portal do Azure.

1. Navegue para o **aplicativos** menu no menu de recursos do Azure AD B2C.
2. Registar uma aplicação de cliente ([aplicação web](active-directory-b2c-app-registration.md) ou [cliente nativo](active-directory-b2c-app-registration.md)) se ainda não tiver um. Se estiver a seguir este guia como ponto de partida, terá de registar uma aplicação de cliente.
3. Clique em **acesso à API**.
4. Clique em **Adicionar**.
5. Selecione a API web e os âmbitos (permissões) que pretende conceder.
6. Clique em **OK**.

> [!NOTE]
> O Azure AD B2C não pedir o seu cliente utilizadores da aplicação para o seu consentimento. Em vez disso, todos os consentimento é fornecido pelo administrador, com base nas permissões configuradas entre as aplicações descritas acima. Se uma concessão de permissões para uma aplicação tiver sido revogada, todos os utilizadores que eram anteriormente possível adquirir essa permissão já não será capazes de fazer isso.

## <a name="requesting-a-token"></a>Pedir um token

Quando pedir um token de acesso, a aplicação cliente tem de especificar as permissões pretendidas no **âmbito** parâmetro do pedido. Por exemplo, para especificar a **valor do âmbito** "leitura" para a API que tenha o **URI de ID de aplicação** de `https://contoso.onmicrosoft.com/notes`, o escopo seria `https://contoso.onmicrosoft.com/notes/read`. Segue-se um exemplo de um pedido de código de autorização para o `/authorize` ponto final.

> [!NOTE]
> Atualmente, os domínios personalizados não são suportados, juntamente com os tokens de acesso. Tem de utilizar o seu domínio tenantName.onmicrosoft.com no URL do pedido.

```
https://<tenantName>.b2clogin.com/tfp/<tenantName>.onmicrosoft.com/<yourPolicyId>/oauth2/v2.0/authorize?client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Para adquirir várias permissões no mesmo pedido, pode adicionar várias entradas no único **âmbito** parâmetro, separado por espaços. Por exemplo:

URL descodificado:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

Com codificação URL:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

Pode solicitar mais âmbitos (permissões) para um recurso do que as concedidas para a sua aplicação de cliente. Quando for este o caso, a chamada terá êxito se pelo menos uma permissão for concedida. Resultante **acesso\_token** terá sua afirmação de "scp" preenchida com apenas as permissões que foram concedido com êxito.

> [!NOTE] 
> Não suportamos a pedidas permissões em relação a dois recursos da web diferentes no mesmo pedido. Esse tipo de pedido irá falhar.

### <a name="special-cases"></a>Casos especiais

O padrão de OpenID Connect especifica vários valores de "escopo" especial. Os seguintes âmbitos especiais representam a permissão "aceder o perfil do usuário":

* **openid**: Isso pede um token de ID
* **offline\_access**: Isso pede um token de atualização (usando [fluxos de código de autenticação](active-directory-b2c-reference-oauth-code.md)).

Se o `response_type` parâmetro numa `/authorize` pedido inclui `token`, o `scope` parâmetro tem de incluir o âmbito de pelo menos um recurso (diferente de `openid` e `offline_access`) que vai ser concedida. Caso contrário, o `/authorize` pedido irá terminar com uma falha.

## <a name="the-returned-token"></a>O token devolvido

Com êxito minted **acesso\_token** (em qualquer um a `/authorize` ou `/token` ponto final), as seguintes declarações estará presentes:

| Nome | Afirmação | Descrição |
| --- | --- | --- |
|Audiência |`aud` |O **ID da aplicação** do recurso único que o token concede acesso para. |
|Âmbito |`scp` |As permissões concedidas ao recurso. Várias permissões concedidas ficarão separadas por espaço. |
|Terceiros autorizados |`azp` |O **ID da aplicação** da aplicação cliente que a solicitação. |

Quando a API recebe a **acesso\_token**, ela tem [validar o token](active-directory-b2c-reference-tokens.md) para provar que o token é autêntico e tem as afirmações corretas.

Estamos sempre abertas para comentários e sugestões! Se tiver quaisquer problemas com este tópico, publique no Stack Overflow usando a marca ["azure-ad-b2c"](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Para pedidos de funcionalidades, adicioná-los ao [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Passos Seguintes

* Criar uma API web com [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Criar uma API web com [node. js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
