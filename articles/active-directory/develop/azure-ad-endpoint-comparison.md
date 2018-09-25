---
title: Comparação entre o ponto final da v2.0 do Azure AD com o ponto final de v1.0 | Documentos da Microsoft
description: Saber as diferenças entre o ponto final de v2.0 do Azure AD e o ponto de extremidade v1.0
services: active-directory
documentationcenter: ''
author: andretms
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: andret
ms.reviewer: hirsin, celested
ms.custom: aaddev
ms.openlocfilehash: 02c7edc84d2ac3a91c33d8f266d022db5cd5cb40
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948964"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-v10-endpoint"></a>Comparação entre o ponto final da v2.0 do Azure AD com o ponto final de v1.0

Ao desenvolver um novo aplicativo, é importante saber as diferenças entre os pontos finais v1.0 e v2.0. Seguem-se as principais diferenças, bem como algumas limitações existentes para o ponto final v2.0.

> [!NOTE]
> Nem todos os cenários do Azure AD e funcionalidades são compatíveis com o ponto final v2.0. Para determinar se deve utilizar o ponto final v2.0, leia sobre [v2.0 limitações](#limitations).

## <a name="who-can-sign-in"></a>Quem pode iniciar sessão

![Quem pode iniciar sessão com pontos finais v1.0 e v2.0](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* O ponto de extremidade v1.0 permite apenas contas profissionais e escolares iniciar sessão na sua aplicação (Azure AD)

* O ponto final v2.0 permite que o trabalho e contas escolares do Azure Active Directory e as contas pessoais (MSA) (hotmail.com, outlook.com, msn.com) para iniciar sessão.

* Pontos finais v1.0 e v2.0 também aceitam inícios de sessão do *[os utilizadores convidados](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* de um diretório do Azure AD para aplicações configuradas como *[inquilino único](single-and-multi-tenant-apps.md)* ou para *multi-inquilino* aplicações configuradas para apontar para o ponto final específico de inquilino (`https://login.microsoftonline.com/{TenantId_or_Name}`).

O ponto final v2.0 permite-lhe escrever aplicações que aceitam inícios de sessão de contas pessoais e profissionais e escolares, dando-lhe a capacidade de escrever a sua aplicação completamente independente de conta. Por exemplo, se a aplicação chama o [Microsoft Graph](https://graph.microsoft.io), algumas funcionalidades adicionais e os dados estarão disponíveis para contas, como seus sites do SharePoint ou dados do diretório de trabalho. Mas para muitas ações, como [ler correio de um utilizador](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), o mesmo código pode aceder ao e-mail para tanto pessoais e contas profissionais e escolares.

Para o ponto final v2.0, pode utilizar uma única biblioteca (MSAL) para obter acesso para o consumidor, ensino e mundos da empresa.

 O ponto de final do Azure AD v1.0 aceita inícios de sessão de contas profissionais e escolares.

## <a name="incremental-and-dynamic-consent"></a>Consentimento de incremental e dinâmico

Aplicações com o ponto de extremidade de versão 1.0 do Azure AD são necessárias para especificar as respetivas necessárias permissões de OAuth 2.0 com antecedência, por exemplo:

![Registo de permissões da interface do Usuário](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

As permissões definidas diretamente no registo de aplicação são **estático**. Embora estáticas permissões da aplicação definida no portal do Azure e mantidos o código interessante e simples, ele pode apresentar alguns problemas para os desenvolvedores:

* A aplicação precisa de saber todas as permissões que ele nunca seria necessário no momento da criação de aplicações. Adicionar permissões ao longo do tempo era um processo difícil.

* A aplicação precisa de saber todos os recursos acessaria nunca antes do tempo. Era difícil criar aplicações que podem aceder a um número arbitrário de recursos.

* A necessidade de uma aplicação para solicitar todas as permissões nunca seriam necessárias após primeiro início de sessão o utilizador. Em alguns casos que isso levou a uma longa lista de permissões, que não recomendada os utilizadores finais de aprovação de acesso da aplicação no início de sessão inicial.

Com o ponto final v2.0, pode ignorar as permissões definidas estaticamente definidas nas informações de registo de aplicação no portal do Azure e especificar as permissões de suas necessidades de aplicação **dinamicamente** em tempo de execução, durante a utilização regular de seu aplicação, independentemente das permissões definidas estaticamente as informações de registo de aplicação.

Para fazer isso, pode especificar os âmbitos que a sua aplicação precisa num determinado período em seu tempo de aplicação, incluindo novos âmbitos no `scope` parâmetro ao pedir um token de acesso - sem a necessidade de previamente defini-los em informações de registo de aplicação .

Se o utilizador ainda não permitiu novos âmbitos adicionados ao pedido, serão solicitados para dar consentimento apenas para as novas permissões. Para saber mais, pode ler sobre [permissões e consentimento e âmbitos](v2-permissions-and-consent.md).

Permitir que uma aplicação para solicitar permissões dinamicamente por meio do `scope` parâmetro dá aos programadores controlo total sobre a experiência dos seus utilizadores. Se desejar, também pode optar por carga de front-seu consentimento experiência e peça para todas as permissões no pedido de autorização de inicial de um. Ou, se a sua aplicação requer um grande número de permissões, pode optar por coletar essas permissões do usuário de forma incremental, conforme eles tentam usar determinados recursos da sua aplicação ao longo do tempo.

Observe que o consentimento de administrador realizado em nome de uma organização usa ainda as permissões estáticas registadas para a aplicação, pelo que é recomendado que defina essas permissões para aplicações que utilizam o ponto final v2.0, se precisar de um administrador para dar consentimento em nome de toda a organização. Isso reduz os ciclos necessários pelo administrador da organização para o aplicativo de configuração

## <a name="scopes-not-resources"></a>Âmbitos, não a recursos

Para aplicações que utilizam o ponto de extremidade v1.0, uma aplicação pode se comportar como um **recursos**, ou um destinatário de tokens. Um recurso pode definir um número de **âmbitos** ou **oAuth2Permissions** que ela compreende, permitindo que o cliente aplicações pedir tokens para esse recurso para um determinado conjunto de âmbitos. Considere o Azure AD Graph API como um exemplo de um recurso:

* Identificador de recurso, ou `AppID URI`: `https://graph.windows.net/`

* Âmbitos, ou `OAuth2Permissions`: `Directory.Read`, `Directory.Write`e assim por diante.

Tudo isso se aplica para o ponto final v2.0. Uma aplicação ainda pode se comportar como recurso, definir escopos e ser identificada por um URI. Aplicações de cliente ainda podem solicitar acesso a esses âmbitos. No entanto, a maneira como um cliente solicita a essas permissões mudou. Para autorizar o ponto de extremidade v1.0, um OAuth 2.0 a pedido para o Azure AD poderão ter aparência:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

em que o **recursos** parâmetro indicado qual o recurso que a aplicação de cliente está a solicitar autorização para. O Azure AD calculadas as permissões necessárias pela aplicação com base na configuração estática no portal do Azure e emitidos tokens em conformidade. Para aplicações utilizando o ponto final v2.0, o mesmo OAuth 2.0 autorizar a solicitação é semelhante:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

em que o **âmbito** parâmetro indica qual o recurso e permissões a aplicação está a solicitar autorização para. O recurso pretendido ainda está presente no pedido – ele simplesmente é incluído em cada um dos valores do parâmetro de âmbito. Utilizar o parâmetro de âmbito desta forma permite que o ponto final v2.0 ser mais compatível com a especificação de OAuth 2.0 e se alinha mais de perto com práticas comuns da indústria. Ele também permite que aplicações efetuar [consentimento incremental](#incremental-and-dynamic-consent), que é descrito anteriormente.

## <a name="well-known-scopes"></a>Âmbitos conhecidos

### <a name="offline-access"></a>Acesso offline

Aplicações com o ponto final v2.0 podem exigir o uso de uma nova permissão bem conhecido para aplicações - o `offline_access` âmbito. Todas as aplicações terá de pedir esta permissão se precisarem de aceder a recursos em nome de um utilizador durante um período prolongado de tempo, mesmo quando o utilizador poderá não estar ativamente a utilizar a aplicação. O `offline_access` âmbito vai aparecer ao utilizador nas caixas de diálogo de consentimento como **aceder aos seus dados em qualquer altura**, que o utilizador tem de aceitar. Pedir o `offline_access` permissão irá permitir que a aplicação web receber refresh_tokens de OAuth 2.0 a partir do ponto final v2.0. Atualizar tokens são vida longa e podem ser trocadas por longos períodos de acesso para novos tokens de acesso de OAuth 2.0.

Se a sua aplicação não solicita a `offline_access` âmbito, não receberá tokens de atualização. Isso significa que quando resgatar um código de autorização no fluxo de código de autorização de OAuth 2.0, receberá apenas novamente um token de acesso a partir do `/token` ponto final. Esse token de acesso permanecerá válido durante um curto período de tempo (normalmente de uma hora), mas, por fim, irá expirar. Nessa altura, a aplicação tem de redirecionar o utilizador de volta para o `/authorize` ponto final para obter um novo código de autorização. Durante este redirecionamento, o utilizador pode ou não poderá ter de introduzir as respetivas credenciais novamente ou voltar a autorizar permissões, dependendo do tipo de aplicação.

Para saber mais sobre o OAuth 2.0, `refresh_tokens`, e `access_tokens`, confira o [referência do protocolo v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>E-mail, perfil e OpenID

Historicamente, o mais básico início de sessão no fluxo de OpenID Connect com o Azure AD forneceria muitas informações sobre o utilizador no resultante *id_token*. As declarações num *id_token* podem incluir o utilizador nome, nome de utilizador preferencial, endereço de e-mail, ID de objeto e muito mais.

As informações que o `openid` âmbito propicia a sua aplicação o acesso ao agora é restrito. O `openid` âmbito apenas irá permitir que a sua aplicação iniciar a sessão do utilizador e receber um identificador específico de aplicação para o utilizador. Se pretender obter os dados pessoais sobre o utilizador na sua aplicação, a aplicação terá de solicitar permissões adicionais do usuário. Dois novos âmbitos – a `email` e `profile` âmbitos – permite-lhe solicitar permissões adicionais.

O `email` âmbito permite o acesso a aplicações para o endereço de e-mail primário do utilizador a `email` o id_token de afirmação. O `profile` âmbito permite o acesso a aplicações para todas as outras informações básicas sobre o usuário – seu nome, nome de utilizador preferencial, ID de objeto e assim por diante.

Isto permite-lhe a aplicação de uma forma de divulgação mínima de código – só pode pedir ao utilizador para o conjunto de informações que a sua aplicação necessita para fazer seu trabalho. Para obter mais informações sobre estes âmbitos, consulte [a referência de âmbito v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Afirmações de Token

As afirmações nos tokens emitidos pelo ponto final v2.0 não será idênticas à tokens emitidos pelo disponível em geral os pontos finais do Azure AD. Migrar para o novo serviço de aplicações não devem assumir que uma afirmação específica existirá no id_tokens ou access_tokens. Estão disponíveis em detalhes adicionais de diferentes tipos de tokens utilizados no ponto final v2.0 a [token de acesso](access-tokens.md) referência e [ `id_token` referência](id-tokens.md)

## <a name="limitations"></a>Limitações

Existem algumas restrições estar atento ao utilizar a versão 2.0.

Quando criar aplicativos que se integram com a plataforma de identidade da Microsoft, precisa decidir se os protocolos de autenticação e de ponto final de v2.0 atender às suas necessidades. A plataforma e o ponto final de v1.0 é ainda totalmente suportado e, em alguns aspetos, é mais rico do que a versão 2.0. No entanto, v2.0 [apresenta vantagens consideráveis](azure-ad-endpoint-comparison.md) para desenvolvedores.

Aqui está uma recomendação simplificada para desenvolvedores no momento:

* Caso seja necessário suportar contas Microsoft pessoais em seu aplicativo, utilize a versão 2.0. Mas antes de o fazer, certifique-se de que compreende as limitações discutidas neste artigo.

* Se seu aplicativo só precisa de suporte profissional da Microsoft contas escolares ou profissionais, não utilize a versão 2.0. Em vez disso, consulte a [v1.0 guia](azure-ad-developers-guide.md).

O ponto final v2.0 irá evoluir para eliminar as restrições listadas aqui, para que apenas terá de utilizar o ponto final v2.0. Entretanto, utilize este artigo para determinar se o ponto final v2.0 é adequado para si. Vamos continuar a atualizar este artigo para refletir o estado atual do ponto final v2.0. Verifique novamente para reavaliar os requisitos de recursos da versão 2.0.

### <a name="restrictions-on-app-types"></a>Restrições de tipos de aplicações

Atualmente, os seguintes tipos de aplicações não são suportados pelo ponto final v2.0. Para obter uma descrição dos tipos de aplicação suportados, consulte [tipos de aplicações na versão 2.0](v2-app-types.md).

#### <a name="standalone-web-apis"></a>APIs da Web autónoma

Pode utilizar o ponto final v2.0 para [criar uma API Web que está protegida com OAuth 2.0](v2-app-types.md#web-apis). No entanto, o que a API Web pode receber tokens apenas a partir de uma aplicação que tenha o ID da mesma aplicação. Não é possível aceder uma API Web a partir de um cliente que tenha um ID da aplicação diferentes. O cliente não será capaz de solicitar ou obter permissões para a API Web.

Para ver como criar uma API Web que aceita tokens de um cliente que tem o mesmo ID de aplicação, veja os exemplos de Web API do ponto final de v2.0 no [v2.0 Introdução](v2-overview.md#getting-started) secção.

### <a name="restrictions-on-app-registrations"></a>Restrições em registos de aplicações

Atualmente, para cada aplicação que pretende integrar com o ponto final v2.0, tem de criar um registo de aplicações no novo [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Do Azure AD existente ou aplicações da conta Microsoft não são compatíveis com o ponto final v2.0. Aplicações que estão registadas no qualquer portal que não seja o Portal de registo de aplicação não são compatíveis com o ponto final v2.0.

Além disso, os registos de aplicações que criar no [Portal de registo de aplicação](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tem os seguintes avisos:

* Apenas dois segredos de aplicação são permitidos por ID da aplicação.

* Um registo de aplicações, registado por um utilizador com uma conta Microsoft pessoal, pode ser visualizado e gerenciado apenas por uma conta de programador único. Não pode ser partilhado entre vários desenvolvedores. Se gostaria de partilhar o seu registo de aplicações entre vários desenvolvedores, pode criar a aplicação ao iniciar sessão no portal de registo com uma conta do Azure AD.

* Existem várias restrições sobre o formato do URL de redirecionamento que é permitido. Para obter mais informações sobre o URL de redirecionamento, veja a secção seguinte.

### <a name="restrictions-on-redirect-urls"></a>Restrições em URLs de redirecionamento

Aplicações que estão registadas no Portal de registo de aplicação estão limitadas a um conjunto limitado de valores de URL de redirecionamento. O URL para aplicações e serviços web tem de começar com o esquema de redirecionamento `https`, e todos os valores de URL de redirecionamento têm de partilhar um único domínio DNS. Por exemplo, não é possível registar uma aplicação web que tem um dos seguintes URLs de redirecionamento:

* `https://login-east.contoso.com`  
* `https://login-west.contoso.com`

O sistema de registo compara o nome DNS completo do URL de redirecionamento existente para o nome DNS do URL de redirecionamento que estiver a adicionar. O pedido para adicionar o nome DNS irá falhar, se uma das seguintes condições for verdadeira:  

* O nome DNS completo do novo URL de redirecionamento não corresponde ao nome DNS do URL de redirecionamento existente.

* O nome DNS completo do novo URL de redirecionamento não é um subdomínio do URL de redirecionamento existente.

Por exemplo, se a aplicação tiver este URL de redirecionamento:

`https://login.contoso.com`

Pode adicioná-lo, da seguinte forma:

`https://login.contoso.com/new`

Neste caso, o nome DNS corresponde exatamente. Ou pode fazer o seguinte:

`https://new.login.contoso.com`

Neste caso, está a referir-se a um subdomínio de DNS de login.contoso.com. Se quiser ter uma aplicação que tenha `login-east.contoso.com` e `login-west.contoso.com` como URLs de redirecionamento, tem de adicionar os URLs de redirecionamento por esta ordem:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Pode adicionar os dois últimos porque são subdomínios do primeiro URL de redirecionamento, contoso.com. Esta limitação será removida numa versão futura.

Observe também, pode ter apenas 20 URLs de resposta para uma aplicação específica.

Para saber como registar uma aplicação no Portal de registo de aplicação, veja [como registar uma aplicação com o ponto final v2.0](quickstart-v2-register-an-app.md).

### <a name="restrictions-on-libraries-and-sdks"></a>Restrições de bibliotecas e SDKs

Atualmente, o suporte de biblioteca para o ponto final v2.0 é limitado. Se quiser usar o ponto final v2.0 num aplicativo de produção, tem estas opções:

* Se estiver criando um aplicativo web, pode utilizar com segurança Microsoft em disponibilidade geral do servidor middleware para executar a validação do início de sessão e o token. Estes incluem o middleware da OWIN abrir ID Connect do ASP.NET e o plug-in de passaporte de node. js. Para exemplos de código que utilizam o middleware da Microsoft, consulte a [v2.0 Introdução](v2-overview.md#getting-started) secção.

* Se estiver criando um aplicativo de desktop ou mobile, pode utilizar um da bibliotecas de autenticação da Microsoft (MSAL) de pré-visualização. Essas bibliotecas estão em pré-visualização suportado de produção, portanto, é seguro para usá-los em aplicações de produção. Pode ler mais sobre os termos de pré-visualização e as bibliotecas disponíveis no [referência de bibliotecas de autenticação](reference-v2-libraries.md).

* Para plataformas não abrangidas por bibliotecas da Microsoft, pode integrar com o ponto final v2.0 diretamente enviar e receber mensagens de protocolo no código da aplicação. Os protocolos de OpenID Connect e OAuth v2.0 [estão documentados explicitamente](active-directory-v2-protocols.md) para ajudar a efetuar uma integração desse tipo.

* Por fim, pode utilizar bibliotecas de código-fonte aberto abrir ID Connect e OAuth para integrar com o ponto final v2.0. O protocolo de v2.0 deve ser compatível com muitas bibliotecas de protocolo aberto sem grandes alterações. A disponibilidade desses tipos de bibliotecas varia consoante o idioma e plataforma. O [abrir ID Connect](http://openid.net/connect/) e [OAuth 2.0](http://oauth.net/2/) Web sites manter uma lista de implementações populares. Para obter mais informações, consulte [bibliotecas de autenticação e a versão 2.0 do Azure Active Directory](reference-v2-libraries.md)e a lista de bibliotecas de cliente de código-fonte aberto e exemplos que foram testados com o ponto final v2.0.

* Para referência, o `.well-known` ponto final para o ponto de extremidade comum v2.0 é `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration` .  Substitua `common` com o seu ID de inquilino para obter os dados específicos ao seu inquilino.  

### <a name="restrictions-on-protocols"></a>Restrições em protocolos

O ponto final v2.0 não suporta SAML ou WS-Federation; ela oferece suporte apenas abrir ID Connect e OAuth 2.0. Nem todas as funcionalidades e capacidades de protocolos OAuth foram incorporadas ao ponto final v2.0.

As seguintes funcionalidades de protocolo e capacidades estão atualmente *não está disponível* o ponto final de v2.0:

* Atualmente, o `email` afirmação é devolvida apenas se uma afirmação de opcional é configurada e escopo é o âmbito = correio eletrónico foi especificado no pedido. No entanto, este comportamento será alterado à medida que o ponto final v2.0 é atualizado para ainda mais estar em conformidade com as normas de abrir ID Connect e OAuth2.0.

* O ponto final v2.0 não suporta a emissoras declarações de função ou grupo nos tokens de ID.

* O [concessão de credenciais de palavra-passe do OAuth 2.0 recursos proprietário](https://tools.ietf.org/html/rfc6749#section-4.3) não é suportado pelo ponto final v2.0.

Além disso, o ponto final v2.0 não suporta qualquer outra forma dos protocolos SAML ou WS-Federation.

Para compreender melhor o âmbito da funcionalidade de protocolo suportado o ponto final de v2.0, leia nossa [referência de protocolo do OpenID Connect e OAuth 2.0](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>Restrições de SAML

Se já usou o Active Directory Authentication Library (ADAL) em aplicativos do Windows, que pode tomar partido da autenticação integrada do Windows, que usa a concessão de asserção de Security Assertion Markup Language (SAML). Com esta concessão, os utilizadores de federado do Azure AD inquilinos podem autenticar silenciosamente com sua instância do Active Directory no local sem introduzir as credenciais. Atualmente, a concessão de asserção de SAML não é suportada no ponto final v2.0.
