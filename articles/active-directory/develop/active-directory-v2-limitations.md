---
title: Restrições e limitações de ponto final de v2.0 do Azure Active Directory | Documentos da Microsoft
description: Uma lista de limitações e restrições para o ponto de final de v2.0 do Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: edf0b52e5889fe8fa875de65fcaa8c2a22df1a7f
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590754"
---
# <a name="should-i-use-the-v20-endpoint"></a>Deve utilizar o ponto final v2.0?

Quando criar aplicativos que se integram com o Azure Active Directory (Azure AD), precisa decidir se os protocolos de autenticação e de ponto final de v2.0 atender às suas necessidades. Ponto de extremidade original do Azure AD é ainda totalmente suportado e, em alguns aspetos, é mais rico do que a versão 2.0. No entanto, o ponto final v2.0 [apresenta vantagens consideráveis](azure-ad-endpoint-comparison.md) para desenvolvedores.

Aqui está uma recomendação simplificada para desenvolvedores no momento:

* Caso seja necessário suportar contas pessoais da Microsoft na sua aplicação, utilize o ponto final v2.0. Mas antes de o fazer, certifique-se de que compreende as limitações discutidas neste artigo.
* Se seu aplicativo só precisa de suporte profissional da Microsoft contas escolares ou profissionais, não utilize o ponto final v2.0. Em vez disso, consulte a [Guia do programador do Azure AD](azure-ad-developers-guide.md).

O ponto final v2.0 irá evoluir para eliminar as restrições listadas aqui, para que apenas terá de utilizar o ponto final v2.0. Entretanto, utilize este artigo para determinar se o ponto final v2.0 é adequado para si. Vamos continuar a atualizar este artigo para refletir o estado atual do ponto final v2.0. Verifique novamente para reavaliar os requisitos de recursos da versão 2.0.

Se tiver uma aplicação do Azure AD existente que não utiliza o ponto final v2.0, não é necessário começar do zero. No futuro, forneceremos uma forma para que possa utilizar seus aplicativos existentes do Azure AD com o ponto final v2.0.

## <a name="restrictions-on-app-types"></a>Restrições de tipos de aplicações

Atualmente, os seguintes tipos de aplicações não são suportados pelo ponto final v2.0. Para obter uma descrição dos tipos de aplicação suportados, consulte [tipos de aplicações para o ponto de final de v2.0 do Azure Active Directory](v2-app-types.md).

### <a name="standalone-web-apis"></a>APIs da Web autónoma

Pode utilizar o ponto final v2.0 para [criar uma API Web que está protegida com OAuth 2.0](v2-app-types.md#web-apis). No entanto, o que a API Web pode receber tokens apenas a partir de uma aplicação que tenha o ID da mesma aplicação. Não é possível aceder uma API Web a partir de um cliente que tenha um ID da aplicação diferentes. O cliente não será capaz de solicitar ou obter permissões para a API Web.

Para ver como criar uma API Web que aceita tokens de um cliente que tem o mesmo ID de aplicação, veja os exemplos de Web API do ponto final de v2.0 no [introdução ao](active-directory-appmodel-v2-overview.md#getting-started) secção.

## <a name="restrictions-on-app-registrations"></a>Restrições em registos de aplicações

Atualmente, para cada aplicação que pretende integrar com o ponto final v2.0, tem de criar um registo de aplicações no novo [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Do Azure AD existente ou aplicações da conta Microsoft não são compatíveis com o ponto final v2.0. Aplicações que estão registadas no qualquer portal que não seja o Portal de registo de aplicação não são compatíveis com o ponto final v2.0. No futuro, planeamos fornecer uma forma de usar um aplicativo existente como uma aplicação v2.0. Atualmente, não existe nenhum caminho de migração para uma aplicação existente para trabalhar com o ponto final v2.0.

Além disso, os registos de aplicações que criar no [Portal de registo de aplicação](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tem os seguintes avisos:

* Apenas dois segredos de aplicação são permitidos por ID da aplicação.
* Um registo de aplicações, registado por um utilizador com uma conta Microsoft pessoal, pode ser visualizado e gerenciado apenas por uma conta de programador único. Não pode ser partilhado entre vários desenvolvedores. Se gostaria de partilhar o seu registo de aplicações entre vários desenvolvedores, pode criar a aplicação ao iniciar sessão no portal de registo com uma conta do Azure AD.
* Existem várias restrições sobre o formato do URI de redirecionamento que é permitido. Para obter mais informações sobre URIs de redirecionamento, veja a secção seguinte.

## <a name="restrictions-on-redirect-uris"></a>Restrições sobre URIs de redirecionamento

Aplicações que estão registadas no Portal de registo de aplicação estão limitadas a um conjunto limitado de valores URI de redirecionamento. O URI para aplicações e serviços web tem de começar com o esquema de redirecionamento `https`, e todos os valores URI de redirecionamento têm de partilhar um único domínio DNS. Por exemplo, não é possível registar uma aplicação web que tem um dos URIs de redirecionamento:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

O sistema de registo compara o nome DNS completo do URI de redirecionamento existente para o nome DNS do URI de redirecionamento que estiver a adicionar. O pedido para adicionar o nome DNS irá falhar, se uma das seguintes condições for verdadeira:  

* O nome DNS completo do URI de redirecionamento novo não corresponde ao nome DNS do URI de redirecionamento existente.
* O nome DNS completo do URI de redirecionamento novo não é um subdomínio do URI de redirecionamento existente.

Por exemplo, se a aplicação tiver este URI de redirecionamento:

`https://login.contoso.com`

Pode adicioná-lo, da seguinte forma:

`https://login.contoso.com/new`

Neste caso, o nome DNS corresponde exatamente. Ou pode fazer o seguinte:

`https://new.login.contoso.com`

Neste caso, está a referir-se a um subdomínio de DNS de login.contoso.com. Se quiser ter uma aplicação com login-East.contoso.com e login-West.contoso.com como URIs de redirecionamento, tem de adicionar que os URIs de redirecionamento nesta ordem:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Pode adicionar os dois últimos porque são subdomínios de redirecionamento primeiro URI, contoso.com. Esta limitação será removida numa versão futura.

Observe também, pode ter apenas 20 URLs de resposta para uma aplicação específica.

Para saber como registar uma aplicação no Portal de registo de aplicação, veja [como registar uma aplicação com o ponto final v2.0](quickstart-v2-register-an-app.md).

## <a name="restrictions-on-libraries-and-sdks"></a>Restrições de bibliotecas e SDKs

Atualmente, o suporte de biblioteca para o ponto final v2.0 é limitado. Se quiser usar o ponto final v2.0 num aplicativo de produção, tem estas opções:

* Se estiver criando um aplicativo web, pode utilizar com segurança Microsoft em disponibilidade geral do servidor middleware para executar a validação do início de sessão e o token. Estes incluem o middleware da OWIN abrir ID Connect do ASP.NET e o plug-in de passaporte de node. js. Para exemplos de código que utilizam o middleware da Microsoft, consulte a [introdução ao](active-directory-appmodel-v2-overview.md#getting-started) secção.
* Se estiver criando um aplicativo de desktop ou mobile, pode utilizar um da bibliotecas de autenticação da Microsoft (MSAL) de pré-visualização. Essas bibliotecas estão em pré-visualização suportado de produção, portanto, é seguro para usá-los em aplicações de produção. Pode ler mais sobre os termos de pré-visualização e as bibliotecas disponíveis no [referência de bibliotecas de autenticação](reference-v2-libraries.md).
* Para plataformas não abrangidas por bibliotecas da Microsoft, pode integrar com o ponto final v2.0 diretamente enviar e receber mensagens de protocolo no código da aplicação. Os protocolos de OpenID Connect e OAuth v2.0 [estão documentados explicitamente](active-directory-v2-protocols.md) para ajudar a efetuar uma integração desse tipo.
* Por fim, pode utilizar bibliotecas de código-fonte aberto abrir ID Connect e OAuth para integrar com o ponto final v2.0. O protocolo de v2.0 deve ser compatível com muitas bibliotecas de protocolo aberto sem grandes alterações. A disponibilidade desses tipos de bibliotecas varia consoante o idioma e plataforma. O [abrir ID Connect](http://openid.net/connect/) e [OAuth 2.0](http://oauth.net/2/) Web sites manter uma lista de implementações populares. Para obter mais informações, consulte [bibliotecas de autenticação e a versão 2.0 do Azure Active Directory](reference-v2-libraries.md)e a lista de bibliotecas de cliente de código-fonte aberto e exemplos que foram testados com o ponto final v2.0.

## <a name="restrictions-on-protocols"></a>Restrições em protocolos

O ponto final v2.0 não suporta SAML ou WS-Federation; ela oferece suporte apenas abrir ID Connect e OAuth 2.0. Nem todas as funcionalidades e capacidades de protocolos OAuth foram incorporadas ao ponto final v2.0.

As seguintes funcionalidades de protocolo e capacidades estão atualmente *não está disponível* o ponto final de v2.0:

* Atualmente, o `email` afirmação é devolvida apenas se uma afirmação de opcional é configurada e escopo é o âmbito = correio eletrónico foi especificado no pedido. No entanto, este comportamento será alterado à medida que o ponto final v2.0 é atualizado para ainda mais estar em conformidade com as normas de abrir ID Connect e OAuth2.0.
* O ponto de extremidade UserInfo de ligação OpenID não está implementado no ponto final v2.0. No entanto, todos os dados de perfil de utilizador que potencialmente seria recebida neste ponto final está disponível no Microsoft Graph `/me` ponto final.
* O ponto final v2.0 não suporta a emissoras declarações de função ou grupo nos tokens de ID.
* O [concessão de credenciais de palavra-passe do OAuth 2.0 recursos proprietário](https://tools.ietf.org/html/rfc6749#section-4.3) não é suportado pelo ponto final v2.0.

Além disso, o ponto final v2.0 não suporta qualquer outra forma dos protocolos SAML ou WS-Federation.

Para compreender melhor o âmbito da funcionalidade de protocolo suportado o ponto final de v2.0, leia nossa [referência de protocolo do OpenID Connect e OAuth 2.0](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Restrições para contas profissionais e escolares

Se já usou o Active Directory Authentication Library (ADAL) em aplicativos do Windows, que pode tomar partido da autenticação integrada do Windows, que usa a concessão de asserção de Security Assertion Markup Language (SAML). Com esta concessão, os utilizadores de federado do Azure AD inquilinos podem autenticar silenciosamente com sua instância do Active Directory no local sem introduzir as credenciais. Atualmente, a concessão de asserção de SAML não é suportada no ponto final v2.0.
