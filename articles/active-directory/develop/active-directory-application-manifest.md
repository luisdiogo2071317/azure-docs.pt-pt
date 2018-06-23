---
title: Compreender o manifesto da aplicação do Azure Active Directory | Microsoft Docs
description: Cobertura de detalhado de manifesto da aplicação do Azure Active Directory, que representa a configuração da identidade de uma aplicação num inquilino do Azure AD e é utilizada para facilitar a OAuth autorização, experiência de consentimento e muito mais.
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
ms.openlocfilehash: 7448a6c37df2c0bbffbebf23d211e3ace8d12edc
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317400"
---
# <a name="azure-active-directory-application-manifest"></a>Manifesto da aplicação do Azure Active Directory
As aplicações que se integram com o Azure AD tem de ser registadas com um inquilino do Azure AD. Esta aplicação pode ser configurada com o manifesto da aplicação (sob o painel do Azure AD) no [portal do Azure](https://portal.azure.com).

## <a name="manifest-reference"></a>Referência do manifesto

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Chave  |Tipo de valor |Valor de Exemplo  |Descrição  |
|---------|---------|---------|---------|
|appID     |  Cadeia de identificador       |""|  O identificador exclusivo para a aplicação que está atribuída a uma aplicação pelo Azure AD.|
|appRoles     |    Tipo de matriz     |<code>[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description":"Read-only access to device information",<br>&emsp;"displayName":"Read Only",<br>&emsp;"id":guid,<br>&emsp;"isEnabled":true,<br>&emsp;"value":"ReadOnly"<br>}]</code>|A coleção de funções que pode declarar uma aplicação. Estas funções podem ser atribuídas a utilizadores, grupos ou principais de serviço.|
|availableToOtherTenants|boolean|`true`|Se este valor é definido como VERDADEIRO, a aplicação está disponível para outros inquilinos. Se definido como false, a aplicação só está disponível para o inquilino está registado no. Para obter mais informações, consulte: [como iniciar sessão em qualquer utilizador do Azure Active Directory (AD) através do padrão de aplicação multi-inquilino](active-directory-devhowto-multi-tenant-overview.md). |
|displayName     |cadeia         |`MyRegisteredApp`         |O nome a apresentar para a aplicação. |
|errorURL     |cadeia         |`http://MyRegisteredAppError`         |O URL de erros encontrados numa aplicação. |
|groupMembershipClaims     |    cadeia     |    `1`     |   Uma máscara de bits que configura a afirmação "grupos" emitida num utilizador ou um token de acesso de OAuth 2.0 que espera que a aplicação. Os valores de máscara de bits são: 0: None, 1: grupos de segurança e funções do Azure AD, 2: reservado e 4: reservado. Definir a máscara de bits para 7 obter todos os grupos de segurança, grupos de distribuição e funções de diretório do Azure AD que o utilizador com sessão iniciada é membro. |
|optionalClaims     |  cadeia       |     `null`    |    O [afirmações opcionais](active-directory-optional-claims.md) devolvido no token pelo serviço de token de segurança para esta aplicação específica. |
|acceptMappedClaims    |      boolean   | `true`        |    Se este valor é definido como VERDADEIRO, permite que uma aplicação para utilizar afirmações de mapeamento sem especificar uma chave de assinatura personalizada.|
|Home page     |  cadeia       |`http://MyRegistererdApp`         |    O URL para a home page da aplicação. |
|identifierUris     |  Matriz de cadeia       | `http://MyRegistererdApp`        |   URI(s) definido pelo utilizador que identificam uma aplicação Web no seu inquilino do Azure AD ou dentro de um domínio verificado personalizado se a aplicação multi-inquilino. |
|keyCredentials     |   Tipo de matriz      | <code>[{<br>&nbsp;"customKeyIdentifier":null,<br>"endDate":"2018-09-13T00:00:00Z",<br>"keyId":"\<guid>",<br>"startDate":"2017-09-12T00:00:00Z",<br>"type":"AsymmetricX509Cert",<br>"usage":"Verify",<br>"value":null<br>}]</code>      |   Esta propriedade contém referências a credenciais atribuída de aplicação, com base em cadeia segredos partilhados e certificados x. 509. Estas credenciais são utilizadas quando pedir tokens de acesso (quando a aplicação está a agir como um cliente em vez disso que como recurso). |
|knownClientApplications     |     Tipo de matriz    |    [guid]     |     O valor é utilizado para pedir consentimento agrupamento se tiver uma solução que contém duas partes, uma aplicação de cliente e uma aplicação API de web personalizada. Se introduzir o appID da aplicação de cliente para este valor, o utilizador só tem de autorizar uma vez para a aplicação de cliente. Azure AD irá saber que consenting para o cliente significa implicitamente consenting para a API web e aprovisionará automaticamente principais de serviço para o cliente e a web API ao mesmo tempo. O cliente e aplicação de API web tem de estar registados no mesmo inquilino.|
|logoutUrl     |   cadeia      |     `http://MyRegisteredAppLogout`    |   O URL a fim de sessão da aplicação. |
|oauth2AllowImplicitFlow     |   boolean      |  `false`       |       Especifica se esta aplicação web pode pedir tokens de fluxo implícito OAuth2.0. A predefinição é falsa. Este sinalizador é utilizado para aplicações baseadas no browser, como aplicações de página única de Javascript. |
|oauth2AllowUrlPathMatching     |   boolean      |  `false`       |   Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir caminho correspondente do URI de redirecionamento contra replyUrls da aplicação. A predefinição é falsa. |
|oauth2Permissions     | Tipo de matriz         |      <code>[{<br>"adminConsentDescription":"Allow the application to access resources on behalf of the signed-in user.",<br>"adminConsentDisplayName":"Access resource1",<br>"id":"\<guid>",<br>"isEnabled":true,<br>"type":"User",<br>"userConsentDescription":"Allow the application to access resource1 on your behalf.",<br>"userConsentDisplayName":"Access resources",<br>"value":"user_impersonation"<br>}]  </code> |  A coleção de âmbitos de permissões de OAuth 2.0 que expõe web de aplicação de API (recurso) para aplicações cliente. Estes âmbitos de permissões podem ser concedidos a aplicações de cliente durante a autorização. |
|oauth2RequiredPostResponse     | boolean        |    `false`     |      Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir pedidos POST, por oposição a pedidos GET. A predefinição é false, o que especifica que apenas os pedidos GET serão permitidos. 
|objectId     | Cadeia de identificador        |     ""    |    O identificador exclusivo para a aplicação no diretório. Este ID não é o identificador utilizado para identificar a aplicação em qualquer transação de protocolo. É utilizador para a referenciar o objeto em consultas de diretório.|
|passwordCredentials     | Tipo de matriz        |   <code>[{<br>"customKeyIdentifier":null,<br>"endDate":"2018-10-19T17:59:59.6521653Z",<br>"keyId":"\<guid>",<br>"startDate":"2016-10-19T17:59:59.6521653Z",<br>"value":null<br>}]  </code>    |    Consulte a descrição para a propriedade keyCredentials. |
|publicClient     |  boolean       |      `false`   | Especifica se uma aplicação é um cliente público (por exemplo, uma aplicação instalada em execução num dispositivo móvel). A predefinição é falso. |
|supportsConvergence     |  boolean       |   `false`      | Esta propriedade não deve ser editada. Aceite o valor predefinido. |
|replyUrls     |  Matriz de cadeia       |   `http://localhost`     |  Esta propriedade de vários valores contém a lista de valores de redirect_uri registados que aceitará do Azure AD como destinos quando regressar tokens. |
|requiredResourceAccess     |     Tipo de matriz    |    <code>[{<br>"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>"resourceAccess":[{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;}]<br>}] </code>    |   Especifica os recursos que esta aplicação requer o acesso e o conjunto de âmbitos de permissões de OAuth e funções da aplicação que necessita em cada um desses recursos. Nesta pré-configuração de acesso a recursos necessários unidades a experiência de consentimento.|
|resourceAppId     |    Cadeia de identificador     |  ""      |   O identificador exclusivo para o recurso de que a aplicação requer acesso. Este valor deve ser igual ao appId declarado na aplicação de recurso de destino. |
|resourceAccess     |  Tipo de matriz       | Ver o valor de exemplo para a propriedade requiredResourceAccess. |   A lista de OAuth2.0 âmbitos de permissões e funções de aplicação que a aplicação necessita do recurso especificado (contém os valores de ID e o tipo de recursos especificados)        |
|samlMetadataUrl    |cadeia| `http://MyRegisteredAppSAMLMetadata` |O URL para os metadados SAML da aplicação.| 

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre a relação entre objetos de aplicação e um Principal de serviço de uma aplicação, consulte [aplicação e objetos de principal de serviço no Azure AD][AAD-APP-OBJECTS].
* Consulte o [Glossário de programador do Azure AD] [ AAD-DEVELOPER-GLOSSARY] para definições de alguns dos conceitos de programador do Azure Active Directory (AD) principal.

Utilize a seguinte secção de comentários para enviar comentários que ajuda a refinar e formam o nosso conteúdo.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

