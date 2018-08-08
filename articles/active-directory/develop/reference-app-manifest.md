---
title: Compreender o manifesto da aplicação do Azure Active Directory | Documentos da Microsoft
description: Abordagem detalhada do manifesto da aplicação do Azure Active Directory, que representa a configuração de identidade de uma aplicação no inquilino do Azure AD e é utilizado para facilitar a OAuth autorização, experiência de consentimento e muito mais.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: elisol, sureshja
ms.openlocfilehash: a0b39f5ac4347e48dc834bf5f5408c36df107aff
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601788"
---
# <a name="azure-active-directory-application-manifest"></a>Manifesto da aplicação do Azure Active Directory
Aplicações que se integram com o Azure AD tem de ser registadas com um inquilino do Azure AD. Esta aplicação pode ser configurada com o manifesto da aplicação (sob o painel do Azure AD) no [portal do Azure](https://portal.azure.com).

## <a name="manifest-reference"></a>Referência do manifesto

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Chave  |Tipo de valor |Valor de Exemplo  |Descrição  |
|---------|---------|---------|---------|
|appID     |  Cadeia de caracteres de identificador       |""|  O identificador exclusivo para a aplicação que está atribuído a uma aplicação pelo Azure AD.|
|appRoles     |    Tipo de matriz     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|A coleção de funções que uma aplicação poderá declarar. Estas funções podem ser atribuídas a utilizadores, grupos ou principais de serviço.|
|availableToOtherTenants|boolean|`true`|Se este valor é definido como true, o aplicativo do está disponível para outros inquilinos. Se definido como false, a aplicação só está disponível para o inquilino está registado no. Para obter mais informações, consulte: [como iniciar sessão a qualquer utilizador do Azure Active Directory (AD) utilizando o padrão de aplicação multi-inquilino](howto-convert-app-to-be-multi-tenant.md). |
|displayName     |cadeia         |`MyRegisteredApp`         |O nome a apresentar para a aplicação. |
|errorURL     |cadeia         |`http://MyRegisteredAppError`         |O URL para erros encontrados num aplicativo. |
|groupMembershipClaims     |    cadeia     |    `1`     |   Uma máscara de bits que configura a afirmação de "groups" emitida num utilizador ou token de acesso de OAuth 2.0 que o aplicativo espera. Os valores de máscara de bits são: 0: nenhum, 1: grupos de segurança e funções do Azure AD, 2: reservado e 4: reservado. Definição da bitmask para 7 irá obter todos os grupos de segurança, grupos de distribuição e funções de diretório do Azure AD que o utilizador com sessão iniciada é membro. |
|optionalClaims     |  cadeia       |     `null`    |    O [afirmações opcionais](active-directory-optional-claims.md) devolvido no token pelo serviço de token de segurança para esta aplicação específica. |
|acceptMappedClaims    |      boolean   | `true`        |    Se este valor é definido como true, ele permite que uma aplicação para utilizar o mapeamento de afirmações sem especificar uma chave de assinatura personalizada.|
|Home page     |  cadeia       |`http://MyRegistererdApp`         |    O URL para a home page do aplicativo. |
|identifierUris     |  Matriz de cadeia de caracteres       | `http://MyRegistererdApp`        |   URI(s) definidas pelo utilizador que identificam exclusivamente uma aplicação Web no seu inquilino do Azure AD, ou dentro de um domínio personalizado verificado se o aplicativo for multi-inquilino. |
|keyCredentials     |   Tipo de matriz      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Esta propriedade contém referências a credenciais atribuídas a aplicação, com base na cadeia de caracteres segredos compartilhados e certificados X.509. Estas credenciais são utilizadas ao pedir tokens de acesso (quando a aplicação está a agir como um cliente em vez disso, que como recurso). |
|knownClientApplications     |     Tipo de matriz    |    [guid]     |     O valor é utilizado para agrupar consentimento, se tiver uma solução que contém duas partes, um aplicativo cliente e uma aplicação de API de web personalizada. Se introduzir o appID do aplicativo de cliente para este valor, o utilizador terá apenas dar consentimento de uma vez para a aplicação cliente. Do Azure AD saberá que consentir o cliente significa implicitamente consentir a API web e irá aprovisionar automaticamente os principais de serviço para o cliente e a web API ao mesmo tempo. O cliente e a aplicação API da web têm de estar registados no mesmo inquilino.|
|logoutUrl     |   cadeia      |     `http://MyRegisteredAppLogout`    |   O URL para terminar a sessão do aplicativo. |
|oauth2AllowImplicitFlow     |   boolean      |  `false`       |       Especifica se esta aplicação web pode pedir tokens de fluxo implícito de OAuth2.0. A predefinição é falso. Este sinalizador é utilizado para aplicações baseadas no browser, como as aplicações de página única de Javascript. |
|oauth2AllowUrlPathMatching     |   boolean      |  `false`       |   Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir correspondência de caminho do URI de redirecionamento contra replyUrls do aplicativo. A predefinição é falso. |
|oauth2Permissions     | Tipo de matriz         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  A coleção de âmbitos de permissão do OAuth 2.0 que a aplicação de API (recurso) do web expõe a aplicações de cliente. Estes âmbitos de permissão podem ser concedidos às aplicações cliente durante o consentimento. |
|oauth2RequiredPostResponse     | boolean        |    `false`     |      Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir pedidos POST, em vez de solicitações GET. A predefinição é false, o que especifica que apenas os pedidos GET serão permitidos. 
|objectId     | Cadeia de caracteres de identificador        |     ""    |    O identificador exclusivo para a aplicação no diretório. Este ID não é o identificador utilizado para identificar a aplicação em qualquer transação de protocolo. É utilizador para a referenciar o objeto em consultas de diretório.|
|passwordCredentials     | Tipo de matriz        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    Consulte a descrição para a propriedade keyCredentials. |
|publicClient     |  boolean       |      `false`   | Especifica se um aplicativo é um cliente público (por exemplo, um aplicativo instalado em execução num dispositivo móvel). A predefinição é falso. |
|supportsConvergence     |  boolean       |   `false`      | Esta propriedade não deve ser editada. Aceite o valor predefinido. |
|replyUrls     |  Matriz de cadeia de caracteres       |   `http://localhost`     |  Esta propriedade de valores múltiplos contém a lista de valores de redirect_uri registados que do Azure AD aceitará como destinos ao retornar tokens. |
|requiredResourceAccess     |     Tipo de matriz    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Especifica os recursos de que esta aplicação requer acesso a e o conjunto de âmbitos de permissão do OAuth e funções de aplicação que necessita em cada um desses recursos. Nesta pré-configuração de acesso a recursos necessária unidades a experiência de consentimento.|
|resourceAppId     |    Cadeia de caracteres de identificador     |  ""      |   O identificador exclusivo para o recurso que o aplicativo requer acesso a. Este valor deve ser igual ao appId declarado no aplicativo de recurso de destino. |
|resourceAccess     |  Tipo de matriz       | Ver o valor de exemplo para a propriedade requiredResourceAccess. |   A lista de âmbitos de permissão de OAuth2.0 e funções de aplicação que a aplicação requer o recurso especificado (contém os valores de ID e o tipo de recursos especificados)        |
|samlMetadataUrl    |cadeia| `http://MyRegisteredAppSAMLMetadata` |O URL para os metadados SAML da aplicação.| 

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre a relação entre objetos de Principal de serviço e aplicações de uma aplicação, consulte [aplicativos e objetos de principal de serviço no Azure AD](app-objects-and-service-principals.md).
* Consulte a [Glossário do programador do Azure AD](developer-glossary.md) para definições de alguns dos conceitos básicos do Azure Active Directory (AD) para desenvolvedores.

Utilize a seguinte secção de comentários para fornecer comentários que ajuda a refinar e moldar o nosso conteúdo.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:../../../azure-ad-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

