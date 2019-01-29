---
title: Comparação entre o ponto final da v2.0 do Azure AD com o ponto final de v1.0 | Documentos da Microsoft
description: Saber as diferenças entre o ponto final de v2.0 do Azure AD e o ponto de extremidade v1.0
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2018
ms.author: celested
ms.reviewer: hirsin, andret, jmprieur, sureshja, jesakowi, lenalepa, kkrishna, dadobali, negoe
ms.custom: aaddev
ms.openlocfilehash: 82984687e1d51598b87c85564ebda74bf4b2daa2
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098052"
---
# <a name="comparing-the-azure-ad-v20-endpoint-with-the-v10-endpoint"></a>Comparação entre o ponto final da v2.0 do Azure AD com o ponto final de v1.0

Ao desenvolver um novo aplicativo, é importante saber as diferenças entre os pontos de extremidade do v1.0 e v2.0 do Azure Active Directory (Azure AD). Este artigo aborda as principais diferenças entre os pontos finais e algumas limitações existentes para o ponto final v2.0.

> [!NOTE]
> O ponto final v2.0 não suporta todos os cenários do Azure AD e funcionalidades. Para determinar se deve utilizar o ponto final v2.0, leia sobre [v2.0 limitações](#limitations).

## <a name="who-can-sign-in"></a>Quem pode iniciar sessão

![Quem pode iniciar sessão com pontos finais v1.0 e v2.0](media/azure-ad-endpoint-comparison/who-can-sign-in.png)

* O ponto de extremidade v1.0 permite apenas contas profissionais e escolares iniciar sessão na sua aplicação (Azure AD)
* O ponto final v2.0 permite o trabalho e contas escolares do Azure AD e contas pessoais da Microsoft (MSA), como hotmail.com, outlook.com e msn.com, para iniciar sessão.
* Pontos finais v1.0 e v2.0 também aceitam inícios de sessão do *[os utilizadores convidados](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b)* de um diretório do Azure AD para aplicações configuradas como *[inquilino único](single-and-multi-tenant-apps.md)* ou para *multi-inquilino* aplicações configuradas para apontar para o ponto final específico de inquilino (`https://login.microsoftonline.com/{TenantId_or_Name}`).

O ponto final v2.0 permite-lhe escrever aplicações que aceitam inícios de sessão de contas Microsoft pessoais e contas profissionais e escolares. Isto dá-lhe a capacidade de escrever a sua aplicação completamente independente de conta. Por exemplo, se a aplicação chama o [Microsoft Graph](https://graph.microsoft.io), algumas funcionalidades adicionais e os dados estarão disponíveis para contas, como seus sites do SharePoint ou dados do diretório de trabalho. Mas para muitas ações, como [ler correio de um utilizador](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_messages), o mesmo código pode aceder ao e-mail para tanto pessoais e contas profissionais e escolares.

Para o ponto final v2.0, pode utilizar o Microsoft Authentication Library (MSAL) para obter acesso para o consumidor, instituições de ensino e os mundos da empresa. O ponto de final do Azure AD v1.0 aceita inícios de sessão de contas profissionais e escolares.

## <a name="incremental-and-dynamic-consent"></a>Consentimento de incremental e dinâmico

Aplicações com o ponto de extremidade de versão 1.0 do Azure AD são necessárias para especificar as respetivas necessárias permissões de OAuth 2.0 com antecedência, por exemplo:

![Registo de permissões da interface do Usuário](./media/azure-ad-endpoint-comparison/app_reg_permissions.png)

As permissões definidas diretamente no registo de aplicação são **estático**. Enquanto estáticas permissões da aplicação definida no portal do Azure manter o código interessante e simples, ele apresenta alguns possíveis problemas para os desenvolvedores:

* A aplicação precisa solicitar todas as permissões nunca seriam necessárias após primeiro início de sessão o utilizador. Isso pode levar a uma longa lista de permissões que desestimula os utilizadores finais de aprovação de acesso da aplicação no início de sessão inicial.

* A aplicação precisa de saber todos os recursos acessaria nunca antes do tempo. Era difícil criar aplicações que podem aceder a um número arbitrário de recursos.

Com o ponto final v2.0, pode ignorar as permissões estáticas definidas as informações de registo de aplicação nas permissões do Azure portal e o pedido de forma incremental em vez disso, o que significa solicitando um conjunto mínimo bare de antemão as permissões e a acumular mais ao longo do tempo como o cliente utiliza funcionalidades adicionais de aplicações. Para fazer isso, pode especificar os âmbitos a sua aplicação precisa num dado momento, incluindo novos âmbitos no `scope` parâmetro ao pedir um token de acesso - sem a necessidade de previamente defini-los em informações de registo de aplicação. Se o utilizador ainda não tiver dado consentimento novos âmbitos adicionados ao pedido, serão solicitados para dar consentimento apenas para as novas permissões. Para obter mais informações, consulte [permissões e consentimento e âmbitos](v2-permissions-and-consent.md).

Permitir que uma aplicação para solicitar permissões dinamicamente por meio do `scope` parâmetro dá aos programadores controlo total sobre a experiência dos seus utilizadores. Também pode front carga seu consentimento experiência e peça para todas as permissões numa solicitação de autorização inicial. Se a sua aplicação requer um grande número de permissões, pode recolher essas permissões do usuário incrementalmente à medida que tentam utilizar determinadas funcionalidades da aplicação ao longo do tempo.

Consentimento de administrador realizado em nome de uma organização ainda exige as permissões estáticas registadas para a aplicação, pelo que deve definir essas permissões para aplicações no portal de registo de aplicação se precisar de um administrador para dar consentimento em nome de toda a organização. Isso reduz os ciclos necessários pelo administrador da organização para configurar a aplicação.

## <a name="scopes-not-resources"></a>Âmbitos, não a recursos

Para aplicações que utilizam o ponto de extremidade v1.0, uma aplicação pode se comportar como um **recursos**, ou um destinatário de tokens. Um recurso pode definir um número de **âmbitos** ou **oAuth2Permissions** que ela compreende, permitindo que o cliente aplicações solicitar tokens desse recurso para um determinado conjunto de âmbitos. Considere o Azure AD Graph API como um exemplo de um recurso:

* Identificador de recurso, ou `AppID URI`: `https://graph.windows.net/`
* Âmbitos, ou `oAuth2Permissions`: `Directory.Read`, `Directory.Write`e assim por diante.

Isso se aplica para o ponto final v2.0. Uma aplicação ainda pode se comportar como um recurso, definir escopos e ser identificada por um URI. Aplicações de cliente ainda podem solicitar acesso a esses âmbitos. No entanto, a maneira como um cliente solicita a essas permissões mudou. 

Para autorizar o ponto de extremidade v1.0, um OAuth 2.0 a pedido para o Azure AD poderão ter aparência:

```text
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https://graph.windows.net/
...
```

Aqui, o **recursos** parâmetro indicado qual o recurso que a aplicação de cliente está a solicitar autorização. O Azure AD calculadas as permissões necessárias pela aplicação com base na configuração estática no portal do Azure e emitidos tokens em conformidade. 

Para aplicações utilizando o ponto final v2.0, o mesmo OAuth 2.0 autorizar a solicitação é semelhante:

```text
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https://graph.windows.net/directory.read%20https://graph.windows.net/directory.write
...
```

Aqui, o **âmbito** parâmetro indica quais recursos e as permissões a aplicação está a solicitar autorização. O recurso pretendido ainda está presente no pedido – ele é incluído em cada um dos valores do parâmetro de âmbito. Utilizar o parâmetro de âmbito desta forma permite que o ponto final v2.0 ser mais compatível com a especificação de OAuth 2.0 e se alinha mais de perto com práticas comuns da indústria. Ele também permite que aplicações efetuar [consentimento incremental](#incremental-and-dynamic-consent) – só pedir permissões quando o aplicativo requê-las, em vez de com antecedência.

## <a name="well-known-scopes"></a>Âmbitos conhecidos

### <a name="offline-access"></a>Acesso offline

Aplicações com o ponto final v2.0 podem exigir o uso de uma nova permissão bem conhecido para aplicações - o `offline_access` âmbito. Todas as aplicações terá de pedir esta permissão se precisarem de aceder a recursos em nome de um utilizador durante um período prolongado de tempo, mesmo quando o utilizador poderá não estar ativamente a utilizar a aplicação. O `offline_access` âmbito vai aparecer ao utilizador nas caixas de diálogo de consentimento como **aceder aos seus dados em qualquer altura**, que o utilizador tem de aceitar. Pedir o `offline_access` permissão irá permitir que a aplicação web receber refresh_tokens de OAuth 2.0 a partir do ponto final v2.0. Atualizar tokens são vida longa e podem ser trocadas por longos períodos de acesso para novos tokens de acesso de OAuth 2.0.

Se a sua aplicação não pedir o `offline_access` âmbito, não receberá tokens de atualização. Isso significa que quando resgatar um código de autorização no fluxo de código de autorização de OAuth 2.0, apenas receberá novamente um token de acesso a partir do `/token` ponto final. Que token permanece válido durante um curto período de tempo (normalmente de uma hora) de acesso, mas, por fim, irá expirar. Nessa altura, a aplicação tem de redirecionar o utilizador de volta para o `/authorize` ponto final para obter um novo código de autorização. Durante este redirecionamento, o utilizador pode ou não poderá ter de introduzir as respetivas credenciais novamente ou reconsent de permissões, dependendo do tipo de aplicação.

Para saber mais sobre o OAuth 2.0, `refresh_tokens`, e `access_tokens`, confira o [referência do protocolo v2.0](active-directory-v2-protocols.md).

### <a name="openid-profile-and-email"></a>E-mail, perfil e OpenID

Historicamente, o mais básico início de sessão no fluxo de OpenID Connect com o Azure AD forneceria muitas informações sobre o utilizador no resultante *id_token*. As declarações num id_token podem incluir o utilizador nome, nome de utilizador preferencial, endereço de e-mail, ID de objeto e muito mais.

As informações que o `openid` âmbito propicia a sua aplicação o acesso ao agora é restrito. O `openid` âmbito apenas irá permitir que a sua aplicação iniciar a sessão do utilizador e receber um identificador específico de aplicação para o utilizador. Se pretender obter os dados pessoais sobre o utilizador na sua aplicação, a sua aplicação precisa solicitar permissões adicionais do usuário. Dois novos âmbitos `email` e `profile`, permite-lhe solicitar permissões adicionais.

* O `email` âmbito permite o acesso a aplicações para o endereço de e-mail primário do utilizador a `email` id_token, partindo do princípio de que o utilizador tem um endereço de e-mail endereçável de afirmação. 
* O `profile` âmbito permite o acesso a aplicações para todas as outras informações básicas sobre o utilizador, tais como o respetivo nome, nome de utilizador preferencial, ID de objeto, e assim por diante no id_token.

Nestes âmbitos permitem-lhe a aplicação de uma forma de divulgação mínima de código para que apenas pode pedir ao utilizador para o conjunto de informações que a sua aplicação precisa para fazer seu trabalho. Para obter mais informações sobre estes âmbitos, consulte [a referência de âmbito v2.0](v2-permissions-and-consent.md).

## <a name="token-claims"></a>Afirmações de token

O ponto final v2.0 emite um conjunto menor de declarações em seus tokens por predefinição, para manter cargas pequeno. Se tiver aplicações e serviços que têm uma dependência numa declaração específica num token de v1.0 que já não é fornecido por predefinição num v2.0 token, considere utilizar o [afirmações opcionais](active-directory-optional-claims.md) funcionalidade para incluir essa declaração.

## <a name="limitations"></a>Limitações

Existem algumas restrições estar atento ao utilizar a versão 2.0.

Quando criar aplicativos que se integram com a plataforma de identidade da Microsoft, precisa decidir se os protocolos de autenticação e de ponto final de v2.0 atender às suas necessidades. A plataforma e o ponto final de v1.0 é ainda totalmente suportado e, em alguns aspetos, é mais rico do que a versão 2.0. No entanto, v2.0 [apresenta vantagens consideráveis](azure-ad-endpoint-comparison.md) para desenvolvedores.

Aqui está uma recomendação simplificada para desenvolvedores no momento:

* Se quer ou precisa para oferecer suporte a contas Microsoft pessoais em seu aplicativo, ou se estiver escrevendo uma nova aplicação, utilize a versão 2.0. Mas antes de o fazer, certifique-se de que compreende as limitações discutidas neste artigo.
* Se estiver a migrar ou atualizar um aplicativo que se baseia em SAML, é possível utilizar a versão 2.0. Em vez disso, consulte a [v1.0 guia](v1-overview.md).

O ponto final v2.0 irá evoluir para eliminar as restrições listadas aqui, para que só precisará usar o ponto final v2.0. Entretanto, utilize este artigo para determinar se o ponto final v2.0 é adequado para si. Vamos continuar a atualizar este artigo para refletir o estado atual do ponto final v2.0. Verifique novamente para reavaliar os requisitos de recursos da versão 2.0.

### <a name="restrictions-on-app-registrations"></a>Restrições em registos de aplicações

Para cada aplicação que pretende integrar com o ponto final v2.0, pode criar um registo de aplicações no [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Em alternativa, pode registar uma aplicação com o [ **registos das aplicações (pré-visualização)** experiência](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) no portal do Azure. Aplicações de conta Microsoft existentes não são compatíveis com o portal de pré-visualização, mas são todas as aplicações do AAD, independentemente de onde ou quando eles foram registados. 

Registos de aplicações que suportam o trabalho e contas escolares e pessoais contas têm os seguintes avisos:

* Apenas dois segredos de aplicação são permitidos por ID de aplicação.
* Um aplicativo que não foi registado num inquilino só pode ser gerido pela conta que registou. Não podem ser partilhado com outros desenvolvedores. Esse é o caso para a maioria das aplicações que foram registados com uma conta Microsoft pessoal no Portal de registo de aplicação. Se gostaria de partilhar o seu registo de aplicações com vários desenvolvedores, registar a aplicação num inquilino utilizando o **registos de aplicações (pré-visualização)** seção do portal do Azure.
* Existem várias restrições sobre o formato do URL de redirecionamento que é permitido. Para obter mais informações sobre o URL de redirecionamento, veja a secção seguinte.

### <a name="restrictions-on-redirect-urls"></a>Restrições em URLs de redirecionamento

Aplicações que estão registadas para v2.0 estão limitadas a um conjunto limitado de valores de URL de redirecionamento. O URL para aplicações e serviços web tem de começar com o esquema de redirecionamento `https`, e todos os valores de URL de redirecionamento têm de partilhar um único domínio DNS.  O sistema de registo compara o nome DNS completo do URL de redirecionamento existente para o nome DNS do URL de redirecionamento que estiver a adicionar. `http://localhost` Também é suportado como um URL de redirecionamento.  

O pedido para adicionar o nome DNS irá falhar, se uma das seguintes condições for verdadeira:  

* O nome DNS completo do novo URL de redirecionamento não corresponde ao nome DNS do URL de redirecionamento existente.
* O nome DNS completo do novo URL de redirecionamento não é um subdomínio do URL de redirecionamento existente.

#### <a name="example-1"></a>Exemplo 1

Se a aplicação tiver um URL de redirecionamento de `https://login.contoso.com`, pode adicionar um URL de redirecionamento em que o nome DNS corresponde exatamente, conforme mostrado no exemplo a seguir:

`https://login.contoso.com/new`

Em alternativa, pode consultar um subdomínio DNS de login.contoso.com, conforme mostrado no exemplo a seguir:

`https://new.login.contoso.com`

#### <a name="example-2"></a>Exemplo 2

Se quiser ter uma aplicação que tenha `login-east.contoso.com` e `login-west.contoso.com` como URLs de redirecionamento, tem de adicionar os URLs de redirecionamento na seguinte ordem:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Pode adicionar os dois últimos porque são subdomínios do primeiro URL de redirecionamento, contoso.com. Esta limitação será removida numa versão futura.

Observe também, pode ter apenas 20 URLs de resposta para uma aplicação específica – este limite aplica em todos os tipos de aplicação que suporta o registo (SPA, cliente nativo, aplicação web e serviço).  

Para saber como registar uma aplicação para utilização com a versão 2.0, veja estes guias de introdução:

* [Registar uma aplicação com o Portal de registo de aplicação](quickstart-v2-register-an-app.md)
* [Registar uma aplicação com a experiência de registos (pré-visualização) da aplicação](quickstart-register-app.md)

### <a name="restrictions-on-libraries-and-sdks"></a>Restrições de bibliotecas e SDKs

Atualmente, o suporte de biblioteca para o ponto final v2.0 é limitado. Se quiser usar o ponto final v2.0 num aplicativo de produção, tem estas opções:

* Se estiver criando um aplicativo web, pode utilizar o middleware de servidor em disponibilidade geral em segurança para executar a validação do início de sessão e o token. Estes incluem o middleware da OWIN OpenID Connect para ASP.NET e o plug-in de passaporte de node. js. Para exemplos de código que utilizam o middleware da Microsoft, consulte a [v2.0 Introdução](v2-overview.md#getting-started) secção.
* Se estiver criando um aplicativo de desktop ou mobile, pode utilizar um da bibliotecas de autenticação da Microsoft (MSAL) de pré-visualização. Essas bibliotecas estão em pré-visualização suportado de produção, portanto, é seguro para usá-los em aplicações de produção. Pode ler mais sobre os termos de pré-visualização e as bibliotecas disponíveis no [referência de bibliotecas de autenticação](reference-v2-libraries.md).
* Para plataformas não abrangidas por bibliotecas da Microsoft, pode integrar com o ponto final v2.0 diretamente enviar e receber mensagens de protocolo no código da aplicação. Os protocolos de OpenID Connect e OAuth v2.0 [estão documentados explicitamente](active-directory-v2-protocols.md) para ajudar a efetuar uma integração desse tipo.
* Por fim, pode utilizar bibliotecas de código-fonte aberto OpenID Connect e OAuth para integrar com o ponto final v2.0. O ponto final v2.0 deve ser compatível com muitas bibliotecas de protocolo aberto sem alterações. A disponibilidade desses tipos de bibliotecas varia consoante o idioma e plataforma. O [OpenID Connect](https://openid.net/connect/) e [OAuth 2.0](https://oauth.net/2/) Web sites manter uma lista de implementações populares. Para obter mais informações, consulte [bibliotecas de autenticação e a versão 2.0 do Azure Active Directory](reference-v2-libraries.md)e a lista de bibliotecas de cliente de código-fonte aberto e exemplos que foram testados com o ponto final v2.0.
* Para referência, o `.well-known` ponto final para o ponto de extremidade comum v2.0 é `https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration`. Substitua `common` com o seu ID de inquilino para obter os dados específicos ao seu inquilino.  

### <a name="protocol-changes"></a>Alterações de protocolo

O ponto final v2.0 não suporta SAML ou WS-Federation; ela oferece suporte apenas OpenID Connect e OAuth 2.0.  Alterações importantes para os protocolos de OAuth 2.0, do ponto de extremidade de v1.0 são: 

* O `email` afirmação é devolvida se estiver configurada uma afirmação opcional **ou** âmbito = correio eletrónico foi especificado no pedido. 
* O `scope` parâmetro é agora suportado em vez do `resource` parâmetro.  
* Respostas muitos foram modificadas para torná-los mais compatível com a especificação de OAuth 2.0, por exemplo, retornando corretamente `expires_in` como um inteiro em vez de uma cadeia de caracteres.  

Para compreender melhor o âmbito da funcionalidade de protocolo suportado o ponto final de v2.0, consulte [referência de protocolo do OpenID Connect e OAuth 2.0](active-directory-v2-protocols.md).

#### <a name="saml-restrictions"></a>Restrições de SAML

Se já usou o Active Directory Authentication Library (ADAL) em aplicativos do Windows, que pode tomar partido da autenticação integrada do Windows, que usa a concessão de asserção de Security Assertion Markup Language (SAML). Com esta concessão, os utilizadores de federado do Azure AD inquilinos podem autenticar silenciosamente com sua instância do Active Directory no local sem introduzir as credenciais. A concessão de asserção de SAML não é suportada no ponto final v2.0.
