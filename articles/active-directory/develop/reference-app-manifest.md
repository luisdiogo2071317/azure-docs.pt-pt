---
title: Compreender o manifesto de aplicação do Azure Active Directory | Documentos da Microsoft
description: Abordagem detalhada do manifesto de aplicação do Azure Active Directory, que representa a configuração de identidade de uma aplicação no inquilino do Azure AD e é utilizado para facilitar a OAuth autorização, experiência de consentimento e muito mais.
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
ms.date: 08/27/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: sureshja, justhu
ms.openlocfilehash: f336771da334ffd964ecd032654b8549b7a5e847
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127025"
---
# <a name="azure-active-directory-app-manifest"></a>Manifesto da aplicação do Azure Active Directory

Aplicações que se integram com o Azure Active Directory (Azure AD) tem de ser registadas com um inquilino do Azure AD. Pode configurar a aplicação no [portal do Azure](https://portal.azure.com) ao selecionar **Azure Active Directory**, selecionar a aplicação que pretende configurar e, em seguida, selecionar **manifesto**.

## <a name="manifest-reference"></a>Referência do manifesto

> [!NOTE]
> Se não pode ver as descrições, maximize a janela do browser ou rolagem/passar o dedo para ver as descrições.

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]

| Chave  | Tipo de valor | Valor de exemplo | Descrição  |
|---------|---------|---------|---------|
| `appID` | Cadeia de caracteres de identificador | `"601790de-b632-4f57-9523-ee7cb6ceba95"` | Especifica o identificador exclusivo da aplicação que está atribuído a uma aplicação pelo Azure AD. |
| `appRoles` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code> | Especifica a coleção de funções de que uma aplicação poderá declarar. Estas funções podem ser atribuídas a utilizadores, grupos ou principais de serviço. |
| `availableToOtherTenants`| boolean | `true` | Se este valor é definido como VERDADEIRO, a aplicação está disponível para outros inquilinos. Se definido como false, a aplicação só está disponível para o inquilino está registado no. Para mais informações, veja [como: iniciar sessão de qualquer utilizador do Azure AD utilizando o padrão de aplicação multi-inquilino](howto-convert-app-to-be-multi-tenant.md). |
| `displayName` | cadeia | `MyRegisteredApp` | O nome a apresentar para a aplicação. |
| `errorURL` | cadeia | `http://MyRegisteredAppError` | O URL para erros encontrados num aplicativo. |
| `groupMembershipClaims` | cadeia | `1` | Uma máscara de bits que configura o `groups` afirmação emitida num utilizador ou o token de acesso de OAuth 2.0 que a aplicação espera. Os valores de máscara de bits são:<br>0: nenhuma<br>1: grupos de segurança e funções do Azure AD<br>2: reservado<br>4: reservado<br>Definição da bitmask para 7 irá obter todos os grupos de segurança, grupos de distribuição e funções de diretório do Azure AD que o utilizador com sessão iniciada é membro. |
| `optionalClaims` | cadeia | `null` | As afirmações opcionais devolvidas no token pelo serviço de token de segurança para esta aplicação específica. Para mais informações, veja [afirmações opcionais](active-directory-optional-claims.md). |
| `acceptMappedClaims` | boolean | `true` | Se este valor é definido como `true`, ele permite que a aplicação utilizar afirmações de mapeamento sem especificar uma chave de assinatura personalizada. |
| `homepage` | cadeia | `http://MyRegisteredApp` | Especifica o URL para a home page da aplicação. |
| `informationalUrls` | cadeia | <code>{<br>&nbsp;&nbsp;&nbsp;"privacy":"http://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"http://MyRegisteredApp/termsofservice"<br>}</code> | Especifica as ligações para os termos da aplicação de serviço e declaração de privacidade. Os termos de serviço e declaração de privacidade são apresentados aos utilizadores através da experiência de consentimento do utilizador. Para mais informações, veja [como: adicionar termos de serviço e declaração de privacidade para registar aplicações do Azure AD](howto-add-terms-of-service-privacy-statement.md). |
| `identifierUris` | Matriz de cadeia de caracteres | `http://MyRegistererdApp` | URI(s) definidas pelo utilizador que identificam exclusivamente uma aplicação Web no seu inquilino do Azure AD, ou dentro de um domínio personalizado verificado se a aplicação multi-inquilino. |
| `keyCredentials` | Tipo de matriz | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> | Contém referências a aplicação atribuída credenciais, com base na cadeia de caracteres segredos compartilhados e certificados X.509. Estas credenciais são utilizadas ao pedir tokens de acesso (quando a aplicação está a agir como um cliente em vez disso, que como recurso). |
| `knownClientApplications` | Tipo de matriz | `[GUID]` | Utilizado para agrupar consentimento, se tiver uma solução que contém duas partes: uma aplicação de cliente e uma aplicação de API da web personalizada. Se introduzir o appID da aplicação de cliente para este valor, o utilizador terá apenas autorizar uma vez a aplicação de cliente. Do Azure AD saberá que consentir o cliente significa implicitamente consentir a API web e irá aprovisionar automaticamente os principais de serviço para o cliente e a web API ao mesmo tempo. O cliente e a aplicação de web API têm de estar registados no mesmo inquilino. |
| `logoutUrl` | cadeia | `http://MyRegisteredAppLogout` | O URL para terminar a aplicação. |
| `oauth2AllowImplicitFlow` | boolean | `false` | Especifica se esta aplicação web pode pedir tokens de fluxo implícito de OAuth2.0. A predefinição é falso. Este sinalizador é utilizado para aplicações baseadas no browser, como as aplicações de página única de Javascript. |
| `oauth2AllowUrlPathMatching` | boolean | `false` | Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir correspondência de caminho do URI de redirecionamento contra replyUrls da aplicação. A predefinição é falso. |
| `oauth2Permissions` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code> | Especifica a coleção de âmbitos de permissão do OAuth 2.0 que a aplicação de API (recurso) do web expõe a aplicações de cliente. Estes âmbitos de permissão podem ser concedidos às aplicações de cliente durante o consentimento. |
| `oauth2RequiredPostResponse` | boolean | `false` | Especifica se, como parte de pedidos de token de OAuth 2.0, do Azure AD irá permitir pedidos POST, em vez de solicitações GET. A predefinição é false, o que especifica que apenas os pedidos GET serão permitidos. |
| `objectId` | Cadeia de caracteres de identificador | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` | O identificador exclusivo para a aplicação no diretório. Este ID não é o identificador utilizado para identificar a aplicação em qualquer transação de protocolo. Ele é usado para fazer referência ao objeto em consultas de diretório. |
| `parentalControlSettings` | cadeia | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>} </code> | `countriesBlockedForMinors` Especifica os países em que a aplicação é bloqueada para os menores.<br>`legalAgeGroupRule` Especifica a regra de grupo etário legal que se aplica aos utilizadores da aplicação. Pode ser definido como `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, ou `BlockMinors`.  |
| `passwordCredentials` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>] </code> | Consulte a descrição para o `keyCredentials` propriedade. |
| `publicClient` | boolean | `false` | Especifica se uma aplicação é um cliente público, como uma aplicação instalada em execução num dispositivo móvel. O valor predefinido é false. |
| `replyUrls` | Matriz de cadeia de caracteres | `"http://localhost"` | Esta propriedade de valor múltiplos contém a lista de valores de redirect_uri registados que do Azure AD aceitará como destinos ao retornar tokens. |
| `requiredResourceAccess` | Tipo de matriz | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;]<br>&nbsp;}<br>] </code> | Com o consentimento dinâmico, `requiredResourceAccess` unidades a experiência de consentimento de administrador e a experiência de consentimento do utilizador para os utilizadores que estão a utilizar o consentimento estático. No entanto, isso não orientar a experiência de consentimento do usuário para o caso geral.<br>`resourceAppId` é o identificador exclusivo para o recurso que a aplicação necessita de acesso a. Este valor deve ser igual ao appId declarado na aplicação de recurso de destino.<br>`resourceAccess` é uma matriz que lista as funções de aplicação que a aplicação necessita do recurso especificado e os âmbitos de permissão de OAuth2.0. Contém os `id` e `type` valores dos recursos especificados. |
| `samlMetadataUrl` | cadeia | `http://MyRegisteredAppSAMLMetadata` | O URL para os metadados SAML para a aplicação. |

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre a relação entre objetos de Principal de serviço e aplicações de uma aplicação, consulte [aplicativos e objetos de principal de serviço no Azure AD](app-objects-and-service-principals.md).
* Consulte a [Glossário do programador do Azure AD](developer-glossary.md) para definições de alguns dos conceitos básicos do Azure Active Directory (AD) para desenvolvedores.

Utilize a seguinte secção de comentários para fornecer comentários que ajuda a refinar e moldar o nosso conteúdo.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
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

