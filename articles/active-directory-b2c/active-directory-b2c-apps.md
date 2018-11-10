---
title: Tipos de aplicativos que podem ser utilizados no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre os tipos de aplicativos que pode utilizar no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7671a0a99e12463fcce5ff33fbcba7e8677dde05
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006199"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Tipos de aplicativos que podem ser utilizados no Active Directory B2C

O Azure Active Directory (Azure AD) B2C suporta a autenticação para uma variedade de arquiteturas de aplicativos modernos. Todos elas baseiam-se nos protocolos padrão da indústria [OAuth 2.0](active-directory-b2c-reference-protocols.md) ou [OpenID Connect](active-directory-b2c-reference-protocols.md). Este documento descreve os tipos de aplicativos que pode criar, independente do idioma ou plataforma que preferir. Ele também ajuda a compreender os cenários de alto nível, antes de iniciar a criação de aplicativos.

Cada aplicativo que utiliza o Azure AD B2C tem de estar registado no seu [inquilino do Azure AD B2C](active-directory-b2c-get-started.md) utilizando o [Portal do Azure](https://portal.azure.com/). O processo de registo de aplicação recolhe e atribui valores, tais como:

* Uma **ID da aplicação** que identifica exclusivamente a sua aplicação.
* R **URL de resposta** que podem ser utilizadas para direcionar as respostas de volta à sua aplicação.

Cada pedido enviado para o Azure AD B2C especifica uma **política**. Uma política controla o comportamento do Azure AD. Também pode utilizar estes pontos finais para criar um conjunto altamente personalizável de experiências de utilizador. Políticas comuns incluem inscrição, início de sessão e edição de perfil. Se não estiver familiarizado com as políticas, deverá ler sobre a [estrutura de política extensível](active-directory-b2c-reference-policies.md) do Azure AD B2C antes de continuar.

A interação de cada aplicação segue um padrão de alto nível semelhante:

1. A aplicação direciona o utilizador para o ponto final v2.0 para executar uma [política](active-directory-b2c-reference-policies.md).
2. O utilizador conclui a política de acordo com a sua definição.
3. O aplicativo recebe um token de segurança a partir do ponto final v2.0.
4. O aplicativo usa o token de segurança para aceder a informações protegidas ou um recurso protegido.
5. O servidor de recurso valida o token de segurança para verificar se o acesso pode ser concedido.
6. A aplicação atualiza periodicamente o token de segurança.

Estes passos podem diferir ligeiramente com base no tipo de aplicativo que está criando.

## <a name="web-applications"></a>Aplicações Web

Para aplicativos da web (incluindo .NET, PHP, Java, Ruby, Python e node. js) que estão alojados num servidor e acedidos através de um browser, o Azure AD B2C suporta [OpenID Connect](active-directory-b2c-reference-protocols.md) para o utilizador de todas as experiências. Isto inclui a gestão de início de sessão, inscrição e perfil. Na implementação do Azure AD B2C do OpenID Connect, seu aplicativo web inicia estes experiências do utilizador através da emissão de pedidos de autenticação para o Azure AD. O resultado do pedido é um `id_token`. Este token de segurança representa a identidade do utilizador. Também fornece informações sobre o utilizador na forma de afirmações:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Saiba mais sobre os tipos de tokens e afirmações disponíveis para uma aplicação no [referência de token do Azure AD B2C](active-directory-b2c-reference-tokens.md).

Num aplicativo web, cada execução de um [política](active-directory-b2c-reference-policies.md) utiliza estes passos de alto nível:

1. O usuário navegar até o aplicativo web.
2. O aplicativo web redireciona o usuário para o Azure AD B2C que indicar que a política para executar.
3. O utilizador conclui a política.
4. O Azure AD B2C devolve um `id_token` ao navegador.
5. O `id_token` é publicado para o URI de redirecionamento.
6. O `id_token` é validada e um cookie de sessão está definido.
7. Uma página segura é devolvida ao usuário.

A validação do `id_token` utilizando uma chave pública de assinatura que é recebida do Azure AD é suficiente para verificar a identidade do utilizador. Isto também define um cookie de sessão que pode ser utilizado para identificar o utilizador nos pedidos subsequentes da página.

Para ver este cenário em ação, experimente um dos exemplos de código de início de sessão de aplicativo web na nossa [secção Introdução](active-directory-b2c-overview.md).

Além de facilitar o início de sessão simple, uma aplicação de servidor web também poderá ter de aceder a um serviço web de back-end. Neste caso, o aplicativo web pode realizar um pouco diferente [fluxo de OpenID Connect](active-directory-b2c-reference-oidc.md) e adquirir tokens utilizando códigos de autorização e tokens de atualização. Este cenário é descrito na seguinte [secção APIs da Web](#web-apis).

## <a name="web-apis"></a>APIs da Web

Pode utilizar o Azure AD B2C para proteger serviços da web, como a API web RESTful da sua aplicação. As APIs Web podem utilizar o OAuth 2.0 para proteger os seus dados, ao autenticar pedidos HTTP recebidos utilizando tokens. O autor da chamada de uma API web acrescenta um token no cabeçalho de autorização de um pedido HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

A API web pode utilizar o token para verificar a identidade do autor da chamada da API e extrair informações sobre o autor da chamada com base nas afirmações codificadas no token. Saiba mais sobre os tipos de tokens e afirmações disponíveis para uma aplicação na [referência de token do Azure AD B2C](active-directory-b2c-reference-tokens.md).

Uma API web pode receber tokens de muitos tipos de clientes, incluindo aplicações web, área de trabalho e aplicativos móveis, aplicações de página única, daemons do lado do servidor e outras APIs web. Eis um exemplo de fluxo completo de uma aplicação web que chama uma API web:

1. A aplicação web executa uma política e o utilizador conclui a experiência do usuário.
2. O Azure AD B2C devolve um `access_token` e um código de autorização para o navegador.
3. As postagens de navegador a `access_token` e código de autorização para o URI de redirecionamento.
4. O servidor web valida o `access token` e define um cookie de sessão.
5. O `access_token` é fornecido para o Azure AD B2C com o código de autorização, o ID de cliente da aplicação e as credenciais.
6. O `access_token` e `refresh_token` são devolvidos para o servidor web.
7. A API web é chamada com o `access_token` num cabeçalho de autorização.
8. A API web valida o token.
9. Proteger os dados são retornados para o servidor web.

Para obter mais informações sobre códigos de autorização, tokens de atualização e os passos para obter os tokens, leia sobre o [protocolo OAuth 2.0](active-directory-b2c-reference-oauth-code.md).

Para saber como proteger uma API web utilizando o Azure AD B2C, consulte os tutoriais sobre web API na nossa [secção Introdução](active-directory-b2c-overview.md).

## <a name="mobile-and-native-applications"></a>Aplicações nativas e móveis

Aplicações instaladas em dispositivos, tais como aplicações de ambiente de trabalho e móveis, muitas vezes precisam de aceder a serviços de back-end ou APIs web em nome dos utilizadores. Pode adicionar experiências de gestão de identidade personalizada às suas aplicações nativas e chamar com segurança os serviços de back-end através da utilização do Azure AD B2C e o [fluxo de código de autorização de OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

Neste fluxo, a execução do aplicativo [políticas](active-directory-b2c-reference-policies.md) e recebe um `authorization_code` do Azure AD depois do utilizador conclui a política. O `authorization_code` representa a permissão da aplicação para chamar serviços de back-end em nome de utilizador que tem atualmente sessão iniciada. O aplicativo, em seguida, pode trocar o `authorization_code` em segundo plano para um `id_token` e um `refresh_token`.  O aplicativo pode usar o `id_token` para autenticar a uma API de web de back-end em pedidos de HTTP. Também pode utilizar o `refresh_token` para obter um novo `id_token` quando o antigo expira.

## <a name="current-limitations"></a>Limitações atuais

### <a name="application-not-supported"></a>Aplicação não suportada 

#### <a name="daemonsserver-side-applications"></a>Aplicações daemons/servidor

Aplicativos que contêm processos de longa execução ou que não funcionam sem a presença de um utilizador tem também de uma forma de aceder a recursos protegidos, como as APIs web. Esses aplicativos podem autenticar e obter os tokens utilizando a identidade da aplicação (em vez de identidade delegada de um utilizador) e com o cliente de OAuth 2.0 fluxo de credenciais. Fluxo de credenciais de cliente não é o mesmo como em-nome-fluxo e em-nome-fluxo não devem ser utilizados para autenticação de servidor a servidor.

Embora o fluxo de credencial de cliente não é atualmente suportado pelo Azure AD B2C, pode configurar o fluxo de credenciais de cliente com o Azure AD. Um inquilino do Azure AD B2C compartilha algumas funcionalidades com o enterprise do Azure AD inquilinos.  O fluxo de credenciais de cliente é suportado com a funcionalidade do Azure AD do inquilino do Azure AD B2C. 

Para configurar o fluxo de credenciais de cliente, consulte [v2.0 do Azure Active Directory e o cliente de OAuth 2.0 credenciais fluxo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Uma autenticação com êxito resulta no recebimento de um token formatado para que possa ser utilizado pelo Azure AD, conforme descrito em [referência de token do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

#### <a name="web-api-chains-on-behalf-of-flow"></a>Cadeias de API Web (fluxo em-nome-de)

Muitas arquiteturas incluem uma API web que precisa chamar outra API web a jusante, estando ambas protegidas pelo Azure AD B2C. Este cenário é comum em clientes nativos que têm uma back-end de API Web. Isto chama então um serviço online da Microsoft, como o Azure AD Graph API.

Este cenário de API web em cadeia pode ser suportado utilizando a concessão de credencial de portador do OAuth 2.0 JWT, também conhecido como fluxo em-nome-de.  No entanto, o fluxo em nome de não está atualmente implementado no Azure AD B2C.

### <a name="reply-url-values"></a>Valores de URL de resposta

Atualmente, as aplicações registadas com o Azure AD B2C estão restritas a um conjunto limitado de valores de URL de resposta. O URL de resposta para aplicações e serviços Web tem de começar com o esquema `https`, e todos os valores de URL de resposta têm de partilhar um único domínio de DNS. Por exemplo, não pode registar uma aplicação Web que tenha um destes URLs de resposta:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

O sistema de registo compara o nome DNS completo do URL de resposta existente ao nome DNS do URL de resposta que está a adicionar. O pedido para adicionar o nome DNS falha se uma das seguintes condições for verdadeira:

- O nome DNS completo do novo URL de resposta não corresponde ao nome DNS do URL de resposta existente.
- O nome DNS completo do novo URL de resposta não é um subdomínio do URL de resposta existente.

Por exemplo, se a aplicação tiver este URL de resposta:

`https://login.contoso.com`

Pode adicioná-lo, da seguinte forma:

`https://login.contoso.com/new`

Neste caso, o nome DNS corresponde exatamente. Ou pode fazer o seguinte:

`https://new.login.contoso.com`

Neste caso, está a referir-se a um subdomínio de DNS de login.contoso.com. Se quiser ter uma aplicação com login-east.contoso.com e login-west.contoso.com como URLs de resposta, tem de adicionar esses URLs de resposta pela seguinte ordem:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Pode adicionar os dois últimos porque são subdomínios do primeiro URL de resposta, contoso.com. 

Quando criar aplicativos de móvel/nativa, define um **URI de redirecionamento** em vez de um **URL de reprodução**. Existem duas considerações importantes ao escolher um URI de redirecionamento:

- **Exclusivo**: o esquema do URI de redirecionamento deve ser exclusivo para cada aplicação. No exemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` é o esquema. Este padrão deve ser seguido. Se duas aplicações partilharem o mesmo esquema, o utilizador vê uma **escolher aplicação** caixa de diálogo. Se o utilizador fizer uma seleção incorreta, o início de sessão falha.
- **Completo**: o URI de redirecionamento tem de ter um esquema e um caminho. O caminho tem de conter, pelo menos, uma barra depois do domínio. Por exemplo, `//contoso/` funciona e `//contoso` falhar. Certifique-se de que existem sem carateres especiais, como o URI de redirecionamento de carateres de sublinhado.

### <a name="faulted-apps"></a>Aplicações com falha

As aplicações do Azure AD B2C não devem ser editadas:

- Noutros portais de gestão de aplicações, tais como o [Portal de registo de aplicação](https://apps.dev.microsoft.com/).
- Com a Graph API ou o PowerShell.

Se editar a aplicação do Azure AD B2C fora do portal do Azure, ele se torna um aplicativo com falha e já não pode ser utilizado com o Azure AD B2C. Deve eliminar a aplicação e criá-la novamente.

Para eliminar a aplicação, vá para o [Portal de registo de aplicação](https://apps.dev.microsoft.com/) e elimine a aplicação. Para que a aplicação fique visível, tem de ser o proprietário da aplicação (e não apenas um administrador do inquilino).

