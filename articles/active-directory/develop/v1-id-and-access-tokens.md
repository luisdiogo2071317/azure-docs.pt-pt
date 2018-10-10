---
title: Saiba mais sobre o token de diferente e afirmação suportados pelo Azure AD | Documentos da Microsoft
description: Um guia para compreender e avaliar as afirmações nos tokens SAML 2.0 e JSON Web Tokens (JWT) emitidos pelo Azure Active Directory (AAD)
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: a8d4f2894a188ce83939180def65c5f5b058d215
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902217"
---
# <a name="azure-ad-token-reference"></a>Referência de token do Azure AD
Azure Active Directory (Azure AD) emite vários tipos de tokens de segurança no processamento de cada fluxo de autenticação. Este documento descreve o formato, características de segurança e conteúdo de cada tipo de token. 

## <a name="types-of-tokens"></a>Tipos de tokens
O Azure AD suporta o [protocolo de autorização de OAuth 2.0](v1-protocols-oauth-code.md), que usa access_tokens e refresh_tokens. Também suporta a autenticação e início de sessão por meio [OpenID Connect](v1-protocols-openid-connect-code.md), que introduz um terceiro tipo de token, o id_token. Cada um destes tokens é representada como um "token de portador".

Um token de portador é um token de segurança simples que concede o acesso de "bearer" a um recurso protegido. Nesse sentido, "bearer" é capaz de apresentar o token de terceiros. Embora a autenticação com o Azure AD é necessário para receber um token de portador, são necessárias etapas para proteger o token, para impedir a interceptação por uma entidade não-intencionais. Uma vez que os tokens de portador não tiver um mecanismo interno para evitar que partes não autorizadas a utilizá-los, devam ser transportadas um canal seguro, como a segurança de camada de transporte (HTTPS). Se um token de portador é transmitido de forma, um man-in a invasão intermediária pode ser utilizado para adquirir o token e obter acesso não autorizado a um recurso protegido. Os mesmos princípios de segurança se aplicam ao armazenar ou de colocação em cache os tokens de portador para utilização posterior. Certifique-se sempre de que a aplicação transmite e armazena os tokens de portador de forma segura. Para obter mais considerações de segurança em tokens de portador, consulte [RFC 6750 secção 5](http://tools.ietf.org/html/rfc6750).

Muitos dos tokens emitidos pelo AD do Azure são implementados como JSON Web Tokens ou JWTs. Um JWT é um meio de compacto e seguras para URL de transferência de informações entre duas partes. As informações contidas no JWTs são conhecidos como "afirmações", ou asserções de informações sobre o portador e o assunto do token. As afirmações no JWTs são objetos JSON codificado e serializado para transmissão. Uma vez que o JWTs emitidos pelo Azure AD são assinadas, mas não encriptados, pode inspecionar facilmente o conteúdo de um JWT para fins de depuração. Existem várias ferramentas disponíveis para fazer isso, como [jwt.ms](https://jwt.ms/). Para obter mais informações sobre JWTs, pode consultar o [especificação do JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens são uma forma de token de segurança de início de sessão que a aplicação recebe quando efetuar a autenticação com [OpenID Connect](v1-protocols-openid-connect-code.md). Eles são representados como [JWTs](#types-of-tokens)e contêm afirmações que pode utilizar para o utilizador a iniciar sessão na sua aplicação. Pode utilizar as declarações num id_token conforme achar – normalmente eles são usados para exibir informações de conta ou a tomada de decisões de controlo de acesso numa aplicação.

Id_tokens são assinadas, mas não encriptados neste momento. Quando a sua aplicação receber uma id_token, deve [validar a assinatura](#validating-tokens) para provar a autenticidade do token e validar algumas declarações no token para provar a sua validade. As declarações validadas por uma aplicação variam consoante os requisitos de cenários, mas existem algumas [validações de afirmação comuns](#validating-tokens) que seu aplicativo deve executar em todos os cenários.

Consulte a secção seguinte para obter informações sobre afirmações id_tokens, bem como um id_token de exemplo. Tenha em atenção que as afirmações no id_tokens não são devolvidas em qualquer ordem específica. Além disso, as novas declarações podem ser introduzidas na id_tokens em qualquer ponto no tempo - a aplicação não deve separar conforme novas declarações são introduzidas. A lista seguinte inclui as afirmações que seu aplicativo confiável pode interpretar no momento da redação deste artigo. Se necessário, ainda mais detalhes podem ser encontradas no [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Id_token de exemplo
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Para a prática, tente inspecionar as afirmações no id_token de exemplo através da colagem-lo para [jwt.ms](https://jwt.ms/).
> 
> 

#### <a name="claims-in-idtokens"></a>Afirmações no id_tokens
> [!div class="mx-codeBreakAll"]
| Afirmação do JWT | Nome | Descrição |
| --- | --- | --- |
| `aud` |Audiência |O destinatário do token. O aplicativo que recebe o token tem de verificar que o valor de público-alvo está correto e rejeitar quaisquer tokens que se destina a um público-alvo diferente. <br><br> **Valor de SAML do exemplo**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Valor de JWT exemplo**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Referência da Classe de Contexto de Autenticação da Aplicação |Indica como o cliente foi autenticado. Para um cliente público, o valor for 0. Se o ID de cliente e segredo do cliente são usados, o valor é 1. Se um certificado de cliente foi utilizado para autenticação, o valor é 2. <br><br> **Valor de JWT exemplo**: <br> `"appidacr": "0"` |
| `acr` |Referência de classe de contexto de autenticação |Indica como o assunto foi autenticado, em vez do cliente na afirmação referência de classe de contexto de autenticação de aplicação. Um valor de "0" indica que a autenticação de utilizador final não cumpria os requisitos da norma ISO/IEC 29115. <br><br> **Valor de JWT exemplo**: <br> `"acr": "0"` |
| Autenticação Instantânea |Regista a data e hora em que ocorreu a autenticação. <br><br> **Valor de SAML do exemplo**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Método de Autenticação |Identifica a forma como o assunto do token foi autenticado. <br><br> **Valor de SAML do exemplo**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Valor de JWT exemplo**: `“amr”: ["pwd"]` |
| `given_name` |Nome Próprio |Fornece a primeira ou "fixados" nome do utilizador, conforme definido no objeto de utilizador do Azure AD. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"given_name": "Frank"` |
| `groups` |Grupos |Fornece os IDs de objeto que representam as associações de grupo do requerente. Estes valores são exclusiva (veja a ID de objeto) e podem ser usados com segurança para gerir o acesso, como a imposição de autorização para aceder a um recurso. Os grupos incluídos na afirmação grupos são configurados numa base por aplicação, por meio da propriedade de "groupMembershipClaims" de manifesto do aplicativo. Um valor nulo será excluir todos os grupos, um valor de "SecurityGroup" irá incluir apenas as associações de grupo de segurança do Active Directory e um valor de "All" irá incluem grupos de segurança e listas de distribuição do Office 365. <br><br> **Notas de**: <br> Consulte a `hasgroups` afirmação abaixo para obter detalhes sobre como utilizar o `groups` a afirmação com a concessão implícita. <br> Para outros fluxos, se o número de grupos que o usuário estiver no é feito por um limite de (150 para SAML, 200 para JWT), em seguida, a utilização excedida de afirmação será adicionada as origens de afirmação apontando para o ponto de extremidade do gráfico que contém a lista de grupos do utilizador. (em. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
|`hasgroups` | Indicador de utilização excedida de grupos do JWT fluxo implícito| Se estiver presente, sempre `true`, que indica o utilizador está em pelo menos um grupo. Utilizado em vez do `groups` afirmação para JWTs nos fluxos de concessão implícita se os grupos de totais de afirmação se estende o fragmento do URI para além dos limites de comprimento de URL (atualmente 6 ou mais grupos). Indica que o cliente deve usar o gráfico para determinar os grupos do utilizador (`https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects`). |
| `groups:src1` <br> `http://schemas.microsoft.com/claims/groups.link` | Indicador de utilização excedida de grupos | Para pedidos de token que não estão limitado de comprimento (consulte `hasgroups` acima), mas ainda demasiado grande para o token, uma ligação para a lista de grupos de completa para o utilizador serão incluída. Para JWTs como uma afirmação distribuída, para SAML como uma nova afirmação em vez do `groups` de afirmação. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=” http://schemas.microsoft.com/claims/groups.link”>`<br>`<AttributeValue>https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"groups":"src1` <br> `_claim_sources`: `"src1" : { "endpoint" : "https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects" }`|
| `idp` |Fornecedor de Identidade |Regista o fornecedor de identidade que autenticou o requerente do token. Este valor é idêntico ao valor de afirmação do emissor, a menos que a conta de utilizador estiver num inquilino diferente do que o emissor. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Armazena a hora em que o token foi emitido. É frequentemente utilizado para medir a atualização de token. <br><br> **Valor de SAML do exemplo**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Valor de JWT exemplo**: <br> `"iat": 1390234181` |
| `iss` |Emissor |Identifica o serviço de token de segurança (STS) que constrói e devolve o token. Os tokens que retorna do Azure AD, o emissor é sts.windows.net. O GUID no valor de afirmação do emissor é o ID de inquilino do diretório do Azure AD. O ID de inquilino é um imutável e fiável o identificador do diretório. <br><br> **Valor de SAML do exemplo**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Valor de JWT exemplo**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Apelido |Fornece o último nome, sobrenome ou nome de família do utilizador, conforme definido no objeto de utilizador do Azure AD. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"family_name": "Miller"` |
| `unique_name` |Nome |Fornece um valor legível por humanos que identifica o requerente do token. Este valor não é garantido de ser exclusivo dentro de um inquilino e foi concebido para ser utilizado apenas para fins de exibição. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |ID de objeto |Contém um identificador exclusivo de um objeto no Azure AD. Este valor é imutável e não pode ser reatribuído ou reutilizado. Utilize o ID de objeto para identificar um objeto nas consultas para o Azure AD. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Funções |Representa todas as funções de aplicação que o assunto tenha sido concedido são feitas direta e indiretamente através da associação de grupo e pode ser usado para impor o controlo de acesso baseado em funções. Funções de aplicação são definidas numa base por aplicação, através do `appRoles` propriedade do manifesto do aplicativo. O `value` propriedade de cada função de aplicação é o valor que é apresentado na afirmação de funções. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Âmbito |Indica as permissões de representação concedidas à aplicação cliente. A permissão predefinida é `user_impersonation`. O proprietário do recurso protegido pode registar os valores adicionais no Azure AD. <br><br> **Valor de JWT exemplo**: <br> `"scp": "user_impersonation"` |
| `sub` |Requerente |Identifica a entidade de segurança sobre o qual o token declara informações, como o utilizador de um aplicativo. Este valor é imutável e não pode ser reatribuído ou reutilizados, por isso, ele pode ser utilizado para realizar verificações de autorização com segurança. Uma vez que o assunto é sempre presente nos tokens de problemas do Azure AD, recomendamos utilizar este valor num sistema de autorização de fins gerais. <br> `SubjectConfirmation` Não é uma afirmação. Ele descreve como o assunto do token é verificado. `Bearer` indica que o assunto é confirmado pela sua posse do token. <br><br> **Valor de SAML do exemplo**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Valor de JWT exemplo**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |ID do inquilino |Um identificador de imutável, não reutilizável que identifica o inquilino do diretório que emitiu o token. Pode utilizar este valor para aceder aos recursos de diretório específico de inquilino num aplicativo de multi-inquilino. Por exemplo, pode utilizar este valor para identificar o inquilino numa chamada para a Graph API. <br><br> **Valor de SAML do exemplo**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Valor de JWT exemplo**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Duração do Token |Define o intervalo de tempo durante o qual um token é válido. O serviço que valida o token deve verificar que a data atual está dentro da duração do token, outra que ele deve rejeitar o token. O serviço poderá permitir até cinco minutos, além do intervalo de duração do token para levar em conta as diferenças no tempo do relógio ("distorção de tempo") entre o Azure AD e o serviço. <br><br> **Valor de SAML do exemplo**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Valor de JWT exemplo**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |Nome Principal de Utilizador |Armazena o nome de utilizador do principal de utilizador.<br><br> **Valor de JWT exemplo**: <br> `"upn": frankm@contoso.com` |
| `ver` |Versão |Armazena o número de versão do token. <br><br> **Valor de JWT exemplo**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Tokens de acesso
Após a autenticação com êxito, o AD do Azure devolve um token de acesso, que pode ser utilizado para aceder a recursos protegidos. O token de acesso é uma base 64 codificada JSON Web Token (JWT) e seu conteúdo pode ser inspecionado, executando-o através de um Decodificador.

Se a aplicação só *usa* tokens de acesso para obter acesso a APIs, pode e (deve) tratar os tokens de acesso como completamente opaco – eles são apenas cadeias de caracteres que seu aplicativo pode passar para recursos nos pedidos HTTP.

Quando solicita um token de acesso, o Azure AD devolve também alguns metadados sobre o token de acesso para consumo da sua aplicação. Estas informações incluem a hora de expiração do token de acesso e os âmbitos para a qual é válido. Isso permite que a aplicação para efetuar a colocação em cache inteligente de tokens de acesso sem ter que analisar a abrir o token de acesso em si.

Se a aplicação for uma API protegida com o Azure AD que espera que os tokens de acesso nos pedidos HTTP, em seguida, deve realizar validação e a inspeção dos tokens recebidos. A aplicação deve realizar a validação do token de acesso antes de o utilizar para aceder aos recursos. Para obter mais informações sobre a validação, consulte [validação de Tokens](#validating-tokens). Para obter detalhes sobre como fazê-lo com o .NET, consulte [proteger uma API Web com os tokens de portador do Azure AD](quickstart-v1-dotnet-webapi.md).

## <a name="refresh-tokens"></a>Tokens de atualização

Atualizar os tokens são tokens de segurança, o que seu aplicativo pode usar para adquirir novos tokens de acesso num fluxo de OAuth 2.0. Ele permite que a sua aplicação alcançar a longo prazo acesso a recursos em nome de um utilizador sem a necessidade de interação pelo utilizador.

Tokens de atualização são vários recursos. Isto quer dizer que um token de atualização recebido durante um pedido de token para um recurso pode ser resgatada para tokens de acesso a um recurso completamente diferente. Para tal, defina o `resource` parâmetro no pedido para o recurso de destino.

Tokens de atualização são completamente opacos para a sua aplicação. Eles são vida útil longa, mas a aplicação não deve ser gravada esperar que um token de atualização irá durar por qualquer período de tempo. Tokens de atualização podem ser invalidadas a qualquer momento no tempo para uma variedade de motivos - consulte [revogar Token](#token-revocation) por esses motivos. A única forma para a sua aplicação saber se um token de atualização é válido é tentar resgatá-lo ao fazer um pedido de token para o ponto final do token do Azure AD.

Quando utilizar um token de atualização para um novo token de acesso, irá receber um novo token de atualização na resposta de token. Deve guardar o token de atualização recém-emitidos, substituição utilizado no pedido. Tal irá garantir que seus tokens de atualização mantêm-se válidas durante mais tempo.

## <a name="validating-tokens"></a>Tokens de validação

Para validar uma id_token ou um access_token, a sua aplicação deve validar a assinatura do token e as declarações. Para validar os tokens de acesso, a aplicação também deve validar o emissor, o público-alvo e os tokens de assinatura. Estes têm de ser validadas em relação aos valores no documento de deteção do OpenID. Por exemplo, a versão independente de inquilino do documento está localizada em [ https://login.microsoftonline.com/common/.well-known/openid-configuration ](https://login.microsoftonline.com/common/.well-known/openid-configuration). O Azure AD middleware possui capacidades incorporadas para validar os tokens de acesso, e pode navegar pelos nossos [exemplos](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) para encontrar um no idioma da sua preferência. Para obter mais informações sobre como validar explicitamente um token JWT, consulte a [exemplo de validação do JWT manual](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation). 

Fornecemos bibliotecas e exemplos de código que mostram como tratar facilmente a validação de token - o abaixo informações simplesmente é fornecido para aqueles que desejam compreender o processo de subjacente. Também existem várias bibliotecas de código-fonte aberto de terceiros disponíveis para a validação do JWT – existe, pelo menos, uma opção para quase todas as plataformas e a idiomas por aí. Para obter mais informações sobre bibliotecas de autenticação do Azure AD e exemplos de código, consulte [bibliotecas de autenticação do Azure AD](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>A validar a assinatura

Um JWT contém três segmentos, que são separados pelo `.` caráter. O primeiro segmento é conhecido como o **cabeçalho**, o segundo como o **corpo**e a terceira, como o **assinatura**. O segmento de assinatura pode ser utilizado para validar a autenticidade do token, para que podem ser fidedignos pela sua aplicação.

Tokens emitidos pelo Azure AD são assinados com algoritmos de criptografia assimétrica padrão do setor, como RSA 256. O cabeçalho do JWT contém informações sobre o método de encriptação e a chave utilizada para assinar o token:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

O `alg` afirmação indica o algoritmo que foi utilizado para assinar o token, enquanto o `x5t` afirmação indica a chave pública específica que foi utilizada para assinar o token.

Em qualquer determinada altura, do Azure AD pode iniciar sessão uma id_token usando qualquer um de um determinado conjunto de pares de chaves públicas-privadas. Azure AD gira o conjunto possível de chaves numa base periódica, para que a sua aplicação deve ser escrita para lidar com essas alterações principais automaticamente. Uma frequência razoável para procurar atualizações para as chaves públicas usadas pelo Azure AD é a cada 24 horas.

Pode adquirir os dados de chave assinatura necessários para validar a assinatura usando o documento de metadados OpenID Connect localizado em:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Experimente este URL num browser!

Este documento de metadados é um objeto JSON que contém várias partes úteis de informações, como a localização dos vários pontos finais necessários para efetuar a autenticação OpenID Connect. 

Ele também inclui um `jwks_uri`, que dá a localização do conjunto de chaves públicas usadas para assinar os tokens. O documento JSON localizado no `jwks_uri` contém todas as informações de chave públicas utilizada no momento específico. A aplicação pode utilizar o `kid` no cabeçalho do JWT para selecionar qual a chave pública neste documento foi utilizada para assinar um determinado token de afirmação. Em seguida, ele pode executar a validação de assinatura usando a chave pública correta e o algoritmo indicado.

> [!NOTE]
> O ponto de extremidade v1.0 devolve ambos os `x5t` e `kid` afirmações. O `x5t` afirmação está em falta na versão 2.0 tokens. O ponto final v2.0 responde com o `kid` de afirmação. Daqui em diante, recomendamos que utilize o `kid` validar o token de afirmação.

Realizar validação de assinatura está fora do escopo deste documento: há muitas bibliotecas de código aberto disponíveis para ajudá-lo a fazê-lo se necessário.

#### <a name="validating-the-claims"></a>A validar as afirmações

Quando a aplicação recebe um token (um id_token após o início de sessão do utilizador, ou um token de acesso como um token de portador no pedido de HTTP) também deve executar algumas verificações contra as afirmações no token. Estes incluem, mas não sejam limitam a:

* O **público-alvo** alegam - Certifique-se de que o token destinava-se a ser dado à sua aplicação.
* O **não antes** e **hora de expiração** afirma - Certifique-se de que o token não expirou.
* O **emissor** alegam - Certifique-se de que o token foi emitido, de fato, à sua aplicação pelo Azure AD.
* O **Nonce** - para mitigar um ataque de reprodução de tokens.
* e muito mais...

Para obter uma lista completa de seu aplicativo deve executar para Tokens de ID de validações de afirmação, consulte a [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Detalhes dos valores esperados para essas declarações estão incluídas no precedente [id_token](#id-tokens) secção.

## <a name="token-revocation"></a>Revogar token

Atualizar tokens podem ser invalidados ou revogados em qualquer altura, por vários motivos. Elas enquadram-se em duas categorias principais: tempos limite e revogações. 

**Tempos limite de token**

* MaxInactiveTime: Se o token de atualização não foi utilizado no tempo ditado pelo MaxInactiveTime, o Token de atualização já não será válido. 
* MaxSessionAge: Se MaxAgeSessionMultiFactor ou MaxAgeSessionSingleFactor tiver sido definido para algo que não seja o padrão (até-revogados), em seguida, reautenticação será necessária depois de decorrido o tempo definido no MaxAgeSession *. 
* Exemplos:
  * O inquilino tem uma MaxInactiveTime de 5 dias e o utilizador foi em férias durante uma semana e, então, AAD não teve um novo pedido de token do usuário em 7 dias. Da próxima vez que o usuário solicita um novo token, eles encontrarão seu Token de atualização foi revogado e tem de introduzir as credenciais novamente. 
  * Aplicações confidenciais tem um MaxAgeSessionSingleFactor de 1 dia. Se um utilizador inicia sessão na segunda-feira e na Terça-feira (depois de decorridos 25 horas), será necessário autenticar novamente. 

**Revogação**

|   | Cookie de palavra-passe com base em | Token de palavra-passe com base em | Cookie com base de não-palavra-passe | Token de não-palavra-passe com base | Token de cliente confidencial| 
|---|-----------------------|----------------------|---------------------------|--------------------------|--------------------------|
|Palavra-passe expirar| Fique vivo|Fique vivo|Fique vivo|Fique vivo|Fique vivo|
|Palavra-passe alterada pelo utilizador| Revogado | Revogado | Fique vivo|Fique vivo|Fique vivo|
|Utilizador realiza SSPR|Revogado | Revogado | Fique vivo|Fique vivo|Fique vivo|
|Administrador repõe a palavra-passe|Revogado | Revogado | Fique vivo|Fique vivo|Fique vivo|
|Utilizador revoga os tokens de atualização [através do PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | Revogado | Revogado |Revogado | Revogado |Revogado | Revogado |
|Administrador revoga todos os tokens de atualização para o inquilino [através do PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | Revogado | Revogado |Revogado | Revogado |Revogado | Revogado |
|[O início de sessão único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code#single-sign-out) na web | Revogado | Fique vivo |Revogado | Fique vivo |Fique vivo |Fique vivo |

> [!NOTE]
> Um início de sessão "não-palavra-passe com base" é aquele em que o utilizador não escreva uma palavra-passe para obtê-la.  Por exemplo, usando o rosto com o Windows Hello, uma chave de FIDO ou um PIN. 
>
> Existe um problema conhecido com o Windows primário atualizar Token.  Se PRT é obtida por meio de uma palavra-passe e, em seguida, o usuário se conecta por meio de Hello, isso não altera a origem de PRT e vai ser revogada se o utilizador altera a palavra-passe. 

## <a name="sample-tokens"></a>Tokens de exemplo

Esta secção apresenta exemplos de tokens SAML e JWT, que retorna do Azure AD. Estes exemplos permitem-lhe ver as afirmações no contexto.

### <a name="saml-token"></a>SAML Token

Este é um exemplo de um token SAML típico.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>Token JWT - representação de utilizadores
Este é um exemplo de um típico JSON web tokens (JWT) usado num fluxo de concessão do código de autorização.
Para além de afirmações, o token inclui um número de versão em **vidor** e **appidacr**, a referência de classe de contexto de autenticação, que indica como o cliente foi autenticado. Para um cliente público, o valor for 0. Se tiver sido utilizado um ID de cliente ou o segredo do cliente, o valor é 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Conteúdo relacionado
* Veja o Azure AD Graph [operações de política](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) e o [entidade de política](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), para saber mais sobre a gestão de política de duração do token através do Azure AD Graph API.
* Para obter mais informações e exemplos sobre como gerir políticas de cmdlets do PowerShell, incluindo exemplos, consulte [durações de token configuráveis no Azure AD](active-directory-configurable-token-lifetimes.md). 
* Adicione [afirmações personalizadas e opcionais](active-directory-optional-claims.md) para os tokens para a sua aplicação. 
