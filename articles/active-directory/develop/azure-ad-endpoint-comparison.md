---
title: Quais são as diferenças no ponto de final de v2.0 do Azure AD? | Microsoft Docs
description: Uma comparação entre o Azure AD original e os pontos de extremidade v2.0.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: elisol, jmprieur, hirsin
ms.custom: aaddev
ms.openlocfilehash: 0e344f6e9dfee3793320dc9cb79e3231c2eeda87
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581923"
---
# <a name="whats-different-about-the-v20-endpoint"></a>O que é diferente sobre o ponto final v2.0?

Se estiver familiarizado com o Azure Active Directory (Azure AD) ou se tiver integrado a aplicações com o Azure AD no passado, existem algumas diferenças no ponto final v2.0, que não esperava. Este artigo invocar as diferenças pela sua compreensão.

> [!NOTE]
> Nem todos os cenários do Azure AD e funcionalidades são compatíveis com o ponto final v2.0. Para determinar se deve utilizar o ponto final v2.0, leia sobre [v2.0 limitações](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>As contas Microsoft e contas do Azure AD

O ponto final v2.0 permite aos desenvolvedores escrever aplicações que aceitam inícios de sessão de contas Microsoft Accounts e o Azure AD, com um ponto de final de autenticação único. Isto dá-lhe a capacidade de escrever a sua aplicação completamente independente conta, que significa que é possível saber do que o tipo de conta que o utilizador inicia sessão com a aplicação. Pode tornar a sua aplicação com suporte para o tipo de conta que está a ser usada numa sessão em particular, mas não precisa.

Por exemplo, se a aplicação chama o [Microsoft Graph](https://graph.microsoft.io), algumas funcionalidades adicionais e os dados estarão disponíveis para os utilizadores empresariais, como seus sites do SharePoint ou dados do diretório. Mas para muitas ações, como [ler correio de um utilizador](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), o código pode ser escrito exatamente o mesmo para contas Microsoft Accounts e o Azure AD. 

Agora a integração de seu aplicativo com Accounts da Microsoft e contas do Azure AD é um processo simple. Pode utilizar um único conjunto de pontos de extremidade, uma única biblioteca e um registo de aplicação única para obter acesso a mundos de consumidor e empresariais. Para saber mais sobre o ponto final v2.0, confira [visão geral do](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Novo portal de registo de aplicação

Para registar uma aplicação que funciona com o ponto final v2.0, tem de utilizar o portal de registo de aplicação do Microsoft: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Este é o portal onde pode obter um ID de aplicação, personalize o aspeto da página de início de sessão da sua aplicação e muito mais. Tudo o que precisa para aceder ao portal é uma conta Microsoft com tecnologia - conta pessoal ou escolares /.

## <a name="one-app-id-for-all-platforms"></a>ID de uma aplicação para todas as plataformas

Se utilizou o Azure AD, provavelmente já registou a várias aplicações diferentes para um único projeto. Por exemplo, se criou um Web site e uma aplicação iOS, era preciso registrá-los em separado, com dois Ids de aplicação diferente. O portal de registo de aplicação do Azure AD forçado a fazer essa distinção durante o registo:

![Registo de aplicação antigo da interface do Usuário](./media/azure-ad-endpoint-comparison/old_app_registration.PNG)

Da mesma forma, se tivesse um Web site e um web api de back-end, poderá se registrou cada um como uma aplicação separada no Azure AD. Ou, se tivesse uma aplicação iOS e uma aplicação Android, também poderá registou duas aplicações diferentes. Registar cada componente de um aplicativo levou a alguns comportamentos inesperados para programadores e aos clientes:

* Cada componente apareceu como uma aplicação separada no inquilino do Azure AD de cada cliente.
* Quando um administrador inquilino tentou aplicar a política para gerir o acesso aos ou eliminar uma aplicação, terá de fazê-lo para cada componente da aplicação.
* Quando os clientes que deu consentimento a uma aplicação, cada componente apareceria no ecrã de consentimento como um aplicativo distinto.

Com o ponto final v2.0, pode agora registar todos os componentes do seu projeto como um registo de aplicação única e utilizar um Id de aplicação única para seu projeto inteiro. Pode adicionar vários "plataformas" para cada projeto e fornecer os dados apropriados para cada plataforma que adicionar. É claro, pode criar aplicações, conforme gosta dependendo das suas necessidades, mas para a maioria dos casos, apenas um Id da aplicação deve ser necessário.

Nosso objetivo é que isso vai levar a uma experiência de desenvolvimento e gestão de aplicações mais simplificado e criar uma vista consolidada de mais de um único projeto pode estar a trabalhar no.

## <a name="scopes-not-resources"></a>Âmbitos, não a recursos

No Azure AD, uma aplicação pode se comportar como um **recursos**, ou um destinatário de tokens. Um recurso pode definir um número de **âmbitos** ou **oAuth2Permissions** que ela compreende, permitindo que o cliente aplicações pedir tokens para esse recurso para um determinado conjunto de âmbitos. Considere o Azure AD Graph API como um exemplo de um recurso:

* Identificador de recurso, ou `AppID URI`: `https://graph.windows.net/`
* Âmbitos, ou `OAuth2Permissions`: `Directory.Read`, `Directory.Write`, etc. 

Tudo isso se aplica para o ponto final v2.0. Uma aplicação ainda pode se comportar como recurso, definir escopos e ser identificada por um URI. Aplicações de cliente ainda podem solicitar acesso a esses âmbitos. No entanto, a maneira como um cliente solicita a essas permissões mudou. No passado, autorizar um OAuth 2.0 pedido para o Azure AD poderão ter a aparência:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

em que o **recursos** parâmetro indicado qual o recurso que a aplicação de cliente está a solicitar autorização para. O Azure AD calculadas as permissões necessárias pela aplicação com base na configuração estática no portal do Azure e emitidos tokens em conformidade. Agora, o OAuth 2.0 mesmo autorizar a solicitação é semelhante:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

em que o **âmbito** parâmetro indica qual o recurso e permissões a aplicação está a solicitar autorização para. O recurso pretendido ainda está presente no pedido – ele simplesmente é incluído em cada um dos valores do parâmetro de âmbito. Utilizar o parâmetro de âmbito desta forma permite que o ponto final v2.0 ser mais compatível com a especificação de OAuth 2.0 e se alinha mais de perto com práticas comuns da indústria. Ele também permite que aplicações efetuar [consentimento incremental](#incremental-and-dynamic-consent), que é descrito na secção seguinte.

## <a name="incremental-and-dynamic-consent"></a>Consentimento de incremental e dinâmico

Aplicações registadas no Azure AD anteriormente necessário para especificar as suas permissões de OAuth 2.0 necessárias no portal do Azure, no momento da criação de aplicações:

![Registo de permissões da interface do Usuário](./media/azure-ad-endpoint-comparison/app_reg_permissions.PNG)

As permissões foram configuradas uma aplicação obrigatória **estaticamente**. Embora isso permitido a configuração da aplicação para existe no portal do Azure e mantidos o código interessante e simples, apresenta alguns problemas para os desenvolvedores:

* Um aplicativo tinha a conhecer todas as permissões que ele nunca seria necessário no momento da criação de aplicações. Adicionar permissões ao longo do tempo era um processo difícil.
* Um aplicativo tinha saber todos os recursos acessaria nunca antes do tempo. Era difícil criar aplicações que podem aceder a um número arbitrário de recursos.
* Tinha uma aplicação solicitar todas as permissões nunca seriam necessárias após primeiro início de sessão o utilizador. Em alguns casos que isso levou a uma longa lista de permissões, que não recomendada os utilizadores finais de aprovação de acesso da aplicação no início de sessão inicial.

Com o ponto final v2.0, pode especificar as permissões de suas necessidades de aplicação **dinamicamente**, no tempo de execução, durante a utilização normal da sua aplicação. Para fazer isso, pode especificar os âmbitos que a sua aplicação precisa em qualquer determinado ponto no tempo ao incluí-los no `scope` parâmetro de um pedido de autorização:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

A permissão de pedidos acima para a aplicação leia dados do diretório de um utilizador do Azure AD, bem como escrever dados no seu diretório. Se o utilizador consentiu essas permissões no passado para esta aplicação específico, eles serão introduzam as credenciais e ser iniciados a sessão. Se o utilizador não consentiu a qualquer uma destas permissões, o ponto final v2.0 pedirá ao utilizador consentimento para essas permissões. Para saber mais, pode ler sobre [permissões e consentimento e âmbitos](v2-permissions-and-consent.md).

Permitir que uma aplicação para solicitar permissões dinamicamente por meio do `scope` parâmetro dá-lhe controlo total sobre a experiência dos seus utilizadores. Se desejar, pode optar por frontload seu consentimento experiência e peça para todas as permissões numa solicitação de autorização inicial. Ou, se a sua aplicação requer um grande número de permissões, pode optar por coletar essas permissões do usuário de forma incremental, conforme eles tentam usar determinados recursos da sua aplicação ao longo do tempo.

## <a name="well-known-scopes"></a>Âmbitos conhecidos

### <a name="offline-access"></a>Acesso offline

Aplicações com o ponto final v2.0 podem exigir o uso de uma nova permissão bem conhecido para aplicações - o `offline_access` âmbito. Todas as aplicações terá de pedir esta permissão se precisarem de aceder a recursos em nome de um utilizador durante um período prolongado de tempo, mesmo quando o utilizador poderá não estar ativamente a utilizar a aplicação. O `offline_access` âmbito vai aparecer ao utilizador nas caixas de diálogo de consentimento como "Aceder a dados offline", que o utilizador tem de aceitar. Pedir o `offline_access` permissão irá permitir que a aplicação web receber refresh_tokens de OAuth 2.0 a partir do ponto final v2.0. Refresh_tokens são vida longa e podem ser trocadas para novo access_tokens de OAuth 2.0 por longos períodos de acesso. 

Se a sua aplicação não solicita a `offline_access` âmbito, não receberá refresh_tokens. Isso significa que resgata um authorization_code no fluxo de código de autorização de OAuth 2.0, receberá apenas novamente uma access_token do `/token` ponto final. Esse access_token permanecerá válido durante um curto período de tempo (normalmente de uma hora), mas, por fim, irá expirar. Nessa altura, a aplicação tem de redirecionar o utilizador de volta para o `/authorize` ponto final para obter um novo authorization_code. Durante este redirecionamento, o utilizador pode ou não poderá ter de introduzir as respetivas credenciais novamente ou voltar a autorizar permissões, dependendo do tipo de aplicação.

Para saber mais sobre o OAuth 2.0, refresh_tokens e access_tokens, consulte a [referência do protocolo v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>E-mail, perfil e OpenID

Historicamente, o mais básico início de sessão no fluxo de OpenID Connect com o Azure AD forneceria muitas informações sobre o utilizador no id_token resultante. As declarações num id_token podem incluir o utilizador nome, nome de utilizador preferencial, endereço de e-mail, ID de objeto e muito mais.

As informações que o `openid` âmbito propicia a sua aplicação o acesso ao agora é restrito. O `openid` âmbito apenas irá permitir que a sua aplicação iniciar a sessão do utilizador e receber um identificador específico de aplicação para o utilizador. Se pretender obter os dados pessoais sobre o utilizador na sua aplicação, a aplicação terá de solicitar permissões adicionais do usuário. Dois novos âmbitos – a `email` e `profile` âmbitos – permite-lhe solicitar permissões adicionais.

O `email` âmbito permite o acesso a aplicações para o endereço de e-mail primário do utilizador a `email` o id_token de afirmação. 

O `profile` âmbito permite o acesso a aplicações para todas as outras informações básicas sobre o usuário – seu nome, nome de utilizador preferencial, ID de objeto e assim por diante.

Isto permite-lhe a aplicação de uma forma de divulgação mínima de código – só pode pedir ao utilizador para o conjunto de informações que a sua aplicação necessita para fazer seu trabalho. Para obter mais informações sobre estes âmbitos, consulte [a referência de âmbito v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Afirmações de Token

As afirmações nos tokens emitidos pelo ponto final v2.0 não será idênticas à tokens emitidos pelo disponível em geral os pontos finais do Azure AD. Migrar para o novo serviço de aplicações não devem assumir que uma afirmação específica existirá no id_tokens ou access_tokens. Para saber mais sobre as declarações específicas emitidas na versão 2.0 tokens, consulte a [referência de token de v2.0](v2-id-and-access-tokens.md).

## <a name="limitations"></a>Limitações

Existem algumas restrições estar atento ao utilizar o ponto de v2.0. Para saber se qualquer uma destas restrições aplicam-se ao seu cenário específico, veja a [doc de limitações de v2.0](active-directory-v2-limitations.md).
