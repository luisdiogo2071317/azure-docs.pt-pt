---
title: Autenticação e autorização no serviço de aplicações do Azure | Microsoft Docs
description: Referência conceptual e descrição geral sobre a autenticação / autorização funcionalidades do App Service do Azure
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: 9220eb3800cd1b7b46e276bbda5bb88fc06d5e54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337829"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Autenticação e autorização no Serviço de Aplicações do Azure

App Service do Azure fornece suportem incorporada autenticação e autorização, para que possa aos utilizadores iniciar sessão e aceder a dados por escrever mínima ou nenhum código na sua aplicação web, API e móvel de back-end e também [das funções do Azure](../azure-functions/functions-overview.md). Este artigo descreve como o serviço de aplicações ajuda a simplificar a autenticação e autorização para a sua aplicação. 

Proteger a autenticação e autorização requer uma compreensão profunda de segurança, incluindo a Federação, encriptação, [tokens de web JSON (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) gestão, [conceder tipos](https://oauth.net/2/grant-types/), e assim sucessivamente. O App Service fornece estes utilitários, de modo a que pode demora mais tempo e energia fornecer valor empresarial para o seu cliente.

> [!NOTE]
> Não está necessárias para utilizar o serviço de aplicações para autenticação e autorização. Muitas estruturas de web estão incluídas com funcionalidades de segurança e pode utilizá-los se assim o desejar. Se precisar de mais flexibilidade do que o App Service fornece, também pode escrever o seus próprio utilitários.  
>

Para obter informações específicas para as aplicações móveis nativas, consulte [autenticação de utilizador e a autorização para aplicações móveis com o App Service do Azure](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Como funciona

O módulo de autenticação e autorização é executado na mesma sandbox como o código da aplicação. Quando estiver ativada, todos os pedidos HTTP recebidos passam através do mesmo antes de ser processado por código da aplicação.

![](media/app-service-authentication-overview/architecture.png)

Este módulo gere vários aspetos da sua aplicação:

- Autentica os utilizadores com o fornecedor especificado
- Valida, armazena e atualiza tokens
- Gere a sessão autenticada
- Injects informações de identidade para cabeçalhos de pedido

O módulo é executado separadamente do código da aplicação e está configurado com as definições de aplicação. Não existem SDKs, idiomas específicos ou alterações ao código da aplicação são necessárias. 

### <a name="user-claims"></a>Afirmações de utilizador

Para todas as estruturas de idioma, serviço de aplicações disponibiliza afirmações do utilizador para o seu código ao inserirem-las para os cabeçalhos do pedido. Povoa o serviço de aplicações para aplicações ASP.NET 4.6, [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) com afirmações do utilizador autenticado, pelo que pode seguir o padrão de código .NET padrão, incluindo o `[Authorize]` atributo. Da mesma forma, para aplicações PHP, serviço de aplicações preenche a `_SERVER['REMOTE_USER']` variável.

Para [das funções do Azure](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` não é hydrated para o código de .NET, mas ainda pode encontrar as afirmações de utilizador nos cabeçalhos do pedido.

Para obter mais informações, consulte [afirmações de utilizador de acesso](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Arquivo de tokens

Serviço de aplicações fornece um arquivo de tokens incorporado, que é um repositório de tokens que estão associados os utilizadores das suas aplicações web, APIs ou as aplicações móveis nativas. Quando ativar a autenticação com qualquer fornecedor, este arquivo de tokens é imediatamente disponibilizado para a sua aplicação. Se precisar de aceder a dados a partir destes fornecedores em nome do utilizador, como o código da aplicação: 

- publicar linha cronológica do Facebook do utilizador autenticado
- ler os dados empresariais do utilizador a partir do Active Directory Graph API do Azure ou mesmo o Microsoft Graph

Os tokens de id, tokens de acesso e atualização de tokens em cache para a sessão autenticada e estão acessível apenas ao utilizador associado.  

Normalmente, tem de escrever código para recolher, armazenar e atualizar estes tokens na sua aplicação. Com o arquivo de tokens, acabou [obter os tokens](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) quando precisar delas e [dizer ao App Service para atualizar os](app-service-authentication-how-to.md#refresh-access-tokens) quando ficam inválidas. 

Se não precisa de trabalhar com tokens na sua aplicação, pode desativar o arquivo de tokens.

### <a name="logging-and-tracing"></a>Registo e rastreio

Se lhe [ativar o registo de aplicação](web-sites-enable-diagnostic-log.md), verá os rastreios de autenticação e autorização diretamente nos seus ficheiros de registo. Se vir um erro de autenticação previmos, pode encontrar todos os detalhes comodamente procurando nos seus registos de aplicação existente. Se ativar [de pedidos falhados](web-sites-enable-diagnostic-log.md), pode ver exatamente que função a autenticação e o módulo de autorização pode ter reproduzido num pedido de falha. Procure nos registos de rastreio, referências a um módulo com o nome `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Fornecedores de identidade

Serviço de aplicações utiliza [identidade federada](https://en.wikipedia.org/wiki/Federated_identity), em que um fornecedor de identidade de terceiros gere as identidades de utilizador e o fluxo de autenticação para si. Fornecedores de identidade cinco estão disponíveis por predefinição: 

| Fornecedor | Ponto final de início de sessão |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Conta Microsoft](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoft` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |

Quando ativa a autenticação e autorização com um destes fornecedores, o ponto final início de sessão está disponível para autenticação de utilizador e para a validação de tokens de autenticação do fornecedor. Pode fornecer os seus utilizadores com qualquer número destas opções de início de sessão com facilidade. Também pode integrar outro fornecedor de identidade ou [sua própria solução de identidade personalizada][custom-auth].

## <a name="authentication-flow"></a>Fluxo de autenticação

O fluxo de autenticação é o mesmo para todos os fornecedores, mas difere dependendo se pretende iniciar sessão com SDK o fornecedor:

- Sem o fornecedor de SDK: A aplicação delega federado início de sessão para o App Service. Este é normalmente o caso de aplicações de browser, o que pode apresentar a página de início de sessão do fornecedor ao utilizador. O código de servidor gere o processo de início de sessão, também é chamado _direcionadas para o servidor de fluxo_ ou _fluxo servidor_. Este caso aplica-se a aplicações web. Também se aplica a aplicações nativas que inscrevem os utilizadores utilizando o SDK do cliente de Mobile Apps, porque o SDK abre uma vista web para a sessão dos utilizadores com a autenticação do serviço de aplicações. 
- Com o fornecedor de SDK: A aplicação iniciar manualmente o utilizador sessão e, em seguida, envia o token de autenticação no App Service para validação. Este é normalmente o caso de aplicações de browser menor, o que não é possível apresentar a página de início de sessão do fornecedor para o utilizador. O código de aplicação gere o processo de início de sessão, também é chamado _direcionadas para o cliente fluxo_ ou _fluxo cliente_. Este caso aplica-se às REST APIs, [das funções do Azure](../azure-functions/functions-overview.md)e os clientes de browser de JavaScript, bem como aplicações web que tem mais flexibilidade no processo de início de sessão. Também se aplica a aplicações móveis nativas que inscrevem os utilizadores utilizando SDK o fornecedor.

> [!NOTE]
> As chamadas a partir de uma aplicação de browser fidedigna no App Service chama outro API de REST no serviço de aplicações ou [das funções do Azure](../azure-functions/functions-overview.md) possa ser autenticado utilizando o fluxo direcionadas para o servidor. Para obter mais informações, consulte [autenticar os utilizadores com o App Service do Azure]().
>

A tabela abaixo mostra os passos para o fluxo de autenticação.

| Passo | Sem o fornecedor de SDK | Com o fornecedor de SDK |
| - | - | - |
| 1. Utilizador de início de sessão no | Redireciona o cliente `/.auth/login/<provider>`. | Código de cliente inicia utilizador sessão diretamente com o SDK do fornecedor e recebe um token de autenticação. Para informações, consulte a documentação do fornecedor. |
| 2. Pós-autenticação | Fornecedor redireciona o cliente `/.auth/login/<provider>/callback`. | Código de cliente mensagens do token do fornecedor para `/.auth/login/<provider>` para validação. |
| 3. Estabelecer uma sessão autenticada | Serviço de aplicações adiciona cookie autenticado a resposta. | Serviço de aplicações devolve os seus próprios tokens de autenticação para o código de cliente. |
| 4. Servir conteúdo autenticado | Cliente inclui o cookie de autenticação em pedidos subsequentes (automaticamente processados pelo browser). | Código de cliente apresenta o token de autenticação no `X-ZUMO-AUTH` cabeçalho (automaticamente processado pelo cliente de Mobile Apps SDKs). |

Para browsers de cliente do serviço de aplicações pode encaminhar automaticamente todos os utilizadores não autenticados `/.auth/login/<provider>`. Também pode apresentar os utilizadores com uma ou mais `/.auth/login/<provider>` ligações para iniciar sessão na sua aplicação utilizando o respetivo fornecedor escolhidas.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Comportamento de autorização

No [portal do Azure](https://portal.azure.com), pode configurar a autorização de serviço de aplicações com um número de comportamentos.

![](media/app-service-authentication-overview/authorization-flow.png)

Os cabeçalhos seguintes descrevem as opções.

### <a name="allow-all-requests-default"></a>Permitir que todos os pedidos (predefinição)

Autenticação e autorização não é gerido pelo serviço de aplicações (desativada). 

Escolha esta opção se não necessitar de autenticação e autorização, ou se pretender escrever o seu próprio código de autenticação e autorização.

### <a name="allow-only-authenticated-requests"></a>Permitir apenas pedidos autenticados

A opção é **iniciar sessão com \<fornecedor >**. Serviço de aplicações redireciona todos os pedidos anónimos `/.auth/login/<provider>` para o fornecedor que escolher. Se o pedido anónimo for proveniente de uma aplicação móvel nativa, a resposta devolvida é um `HTTP 401 Unauthorized`.

Com esta opção, não precisa de escrever qualquer código de autenticação na sua aplicação. Melhorar autorização, tais como autorização específicos da função, pode ser processada através da inspeção afirmações do utilizador (consulte [afirmações de utilizador de acesso](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Permitir que todos os pedidos, mas validar pedidos autenticados

A opção é **pedidos anónimos permitir**. Esta opção ativa a autenticação e autorização no serviço de aplicações, mas defers decisões de autorização para o código da aplicação. Para pedidos autenticados, serviço de aplicações também passa ao longo de informações de autenticação nos cabeçalhos de HTTP. 

Esta opção fornece mais flexibilidade no processamento de pedidos anónimos. Por exemplo, permite-lhe [apresentar as opções de início de sessão várias](app-service-authentication-how-to.md#configure-multiple-sign-in-options) aos seus utilizadores. No entanto, tem de escrever código. 

## <a name="more-resources"></a>Mais recursos

[Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no App Service do Azure (Windows)](app-service-web-tutorial-auth-aad.md)  
[Tutorial: Autenticar e autorizar utilizadores ponto-a-ponto no App Service do Azure para Linux](containers/tutorial-auth-aad.md)  
[Personalizar a autenticação e autorização no serviço de aplicações](app-service-authentication-how-to.md)

Guias de procedimentos específica do fornecedor:

* [Como configurar a sua aplicação para utilizar o início de sessão do Azure Active Directory][AAD]
* [Como configurar a sua aplicação para utilizar o início de sessão do Facebook][Facebook]
* [Como configurar a sua aplicação para utilizar o início de sessão do Google][Google]
* [Como configurar a sua aplicação para utilizar o início de sessão do Microsoft Account][MSA]
* [Como configurar a sua aplicação para utilizar o início de sessão do Twitter][Twitter]
* [Como: utilizar a autenticação personalizada para a sua aplicação][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
