---
title: Permissões no Azure AD | Microsoft Docs
description: Saiba mais sobre os âmbitos e as permissões no Azure Active Directory e como utilizá-los
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 6c0dc122-2cd8-4d70-be5a-3943459d308e
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/20/2017
ms.author: celested
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: 749253d6a082bcdc2b80c5984f20c4b8c4039ad0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="permissions-in-azure-ad"></a>Permissões no Azure AD
O Azure Active Directory (Azure AD) utiliza intensivamente as permissões tanto para fluxos de OAuth, como de OpenID Connect (OICD). Quando a sua aplicação recebe um token de acesso do Azure AD, este incluirá afirmações que descrevem as permissões (também conhecidas como âmbitos) que a aplicação tem relativamente a um determinado recurso. Isto facilita autorização para o recurso porque apenas tem de verificar que o seu token contém a permissão adequada para a API que esteja a chamar. 

## <a name="types-of-permissions"></a>Tipos de permissões
O Azure AD define dois tipos de permissões: 
* **Permissões delegadas** - são utilizadas por aplicações que têm um utilizador com sessão iniciada presente. Nestas aplicações, ou o utilizador ou um administrador autoriza as permissões que a aplicação pede e é delegada a esta permissão para agir como o utilizador com sessão iniciada quando forem feitas chamadas para uma API. Dependendo da API, o utilizador poderá não conseguir consentir diretamente a API e, em alternativa, terá de [pedir a um administrador que forneça "consentimento de administrador".](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent)
* **Permissões de aplicação** - são utilizadas por aplicações que são executadas sem um utilizador com sessão iniciada presente; por exemplo, as aplicações que são executadas como serviços em segundo plano ou daemons. As permissões da aplicação só podem ser [consentidas por um administrador](/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant) porque, regra geral, são muito poderosas e permitem acesso a dados em limites do utilizador ou a dados que podem estar limitados a administradores. 

As permissões efetivas são aquelas que a sua aplicação terá quando fizer pedidos para uma API. 

* Relativamente às permissões delegadas, as permissões efetivas da aplicação serão o ponto comum com menos privilégios das permissões delegadas que a aplicação concedeu (através do consentimento) e dos privilégios do utilizador com sessão iniciada atualmente. A aplicação nunca pode ter mais privilégios do que o utilizador com sessão iniciada. Nas organizações, os privilégios do utilizador com sessão iniciada podem ser determinados por uma política ou por associação a uma ou mais funções de administrador. Para obter mais informações sobre as funções de administrador, veja [Atribuir funções de administrador no Azure AD](/azure/active-directory/active-directory-assign-admin-roles-azure-portal.md).
    Por exemplo, suponha foi foi concedida à sua aplicação a permissão delegada `User.ReadWrite.All` no Microsoft Graph. Esta permissão concede nominalmente permissão à aplicação para ler e atualizar o perfil de todos os utilizadores de uma organização. Se o utilizador com sessão iniciada for administrador global, a aplicação poderá atualizar o perfil de todos os utilizadores da organização. No entanto, se o utilizador com sessão iniciada não tiver uma função de administrador, a aplicação só poderá atualizar o perfil desse utilizador. Não poderá atualizar os perfis dos outros utilizadores da organização porque o utilizador em cujo nome tem permissão para agir não tem esses privilégios.
* Relativamente às permissões da aplicação, as permissões efetivas da sua aplicação são o nível completo de privilégios implícitos nas permissões. Por exemplo, uma aplicação que tenha a permissão de aplicação `User.ReadWrite.All` pode atualizar o perfil de cada utilizador da organização. 

## <a name="permission-attributes"></a>Atributos das permissões
As permissões no Azure AD têm várias propriedades que ajudam os utilizadores, os administradores ou os programadores de aplicações a tomar decisões informadas relativamente ao que é que a permissão concede acesso. 

> [!NOTE]
> Pode utilizar o portal do Azure ou o PowerShell para ver as permissões que uma Aplicação do Azure AD ou Principal de Serviço expõe. Experimente este script para ver as permissões que o Microsoft Graph expõe.
> ```powershell
> Connect-AzureAD
> 
> # Get OAuth2 Permissions/delegated permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").OAuth2Permissions
> 
> # Get App roles/application permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").AppRoles
> ```

| Nome da propriedade | Descrição | Exemplo | 
| --- | --- | --- |
| ID | Este é um valor de GUID que identifica exclusivamente esta permissão. | 570282fd-fa5c-430d-a7fd-fc8dc98a9dca | 
| IsEnabled | Indica se este âmbito está disponível para utilização. | true | 
| Tipo | Indica se esta permissão requer consentimento do utilizador ou consentimento do administrador. | Utilizador | 
| AdminConsentDescription | Esta é uma descrição que é apresentada aos administradores durante as experiências de consentimento de administrador | Permite que a aplicação leia e-mails nas caixas de correio do utilizador. | 
| AdminConsentDisplayName | É o nome amigável que é apresentado aos administradores durante a experiência de consentimento de administrador. | Ler o correio do utilizador | 
| UserConsentDescription | Esta é uma descrição que é apresentada aos utilizadores durante uma experiência de consentimento de utilizador. |  Permite que a aplicação leia e-mails na sua caixa de correio. | 
| UserConsentDisplayName | É o nome amigável que é apresentado aos utilizadores durante uma experiência de consentimento de utilizador. | Ler o seu correio | 
| Valor | Esta é a cadeia que é utilizada para identificar a permissão durante os fluxos de autorização de OAuth 2.0. Também pode ser combinada com a cadeia de URI de ID da Aplicação, de modo a formar um nome de permissão completamente qualificado. | `Mail.Read` | 

## <a name="types-of-consent"></a>Tipos de consentimentos
As aplicações no Azure AD dependem de consentimento para ter acesso a recursos ou APIs necessárias. Para ter êxito, a sua aplicação poderá ter de conhecer vários tipos de consentimentos. Se estiver a definir permissões, também terá de saber como é que os utilizadores vão obter acesso à sua aplicação ou API.

* **Consentimento de utilizador estático** - ocorre automaticamente durante o [fluxo de autorização de OAuth 2.0](/azure/active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) quando especifica o recurso com o qual a aplicação quer interagir. No cenário de consentimento de utilizador estático, é necessário que a aplicação já tenha especificado todas as permissões de que precisa na respetiva configuração no portal do Azure. Se o utilizador (ou o administrador, conforme apropriado) não tiver concedido consentimento para esta aplicação, o Azure AD pedirá ao utilizador para dar consentimento neste momento. 

    Saiba mais sobre como registar uma aplicação do Azure AD que pede acesso a um conjunto estático de APIs.
* **Consentimento de utilizador dinâmico** - é uma funcionalidade do modelo de aplicações do Azure AD v2. Neste cenário, a aplicação pede um conjunto de âmbitos de que precisa no [fluxo de autorização de OAuth 2.0 para aplicações de v2](/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent). Se o utilizador ainda não tiver consentido, ser-lhe-á pedido que consinta neste momento. [Saiba mais sobre o consentimento dinâmico](/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent).

    > [!NOTE]
    > O consentimento dinâmico pode ser prático, mas apresenta um desafio grande para as permissões que precisam de consentimento de administrador, uma vez que a experiência deste tipo de consentimento desconhece essas permissões no momento do consentimento. Se precisar de âmbitos com privilégio de administrador, a aplicação tem de registá-los no portal do Azure.
  
* **Consentimento de administrador** - é necessário quando a aplicação precisa de aceder a determinadas permissões de privilégio elevado. Isto garante que os administradores têm alguns controlos adicionais antes de autorizarem as aplicações ou os utilizadores a aceder a dados com privilégios elevados da organização. [Saiba mais sobre como conceder o consentimento de administrador](/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="best-practices"></a>Melhores práticas

### <a name="resource-best-practices"></a>Melhores práticas para recursos
Os recursos que expõem APIs devem definir permissões que sejam muito específicas para os dados ou ações que pretendem proteger. Isto ajuda a garantir que os clientes não acabam por receber permissões para aceder a dados de que não precisam e que os utilizadores são bem informados quanto aos dados que estão a consentir.

Os recursos devem definir explicitamente as permissões `Read` e `ReadWrite` em separado. 

Os recursos devem marcar todas as permissões que permitem acesso aos dados em limites do utilizador como permissões `Admin`. 

Os recursos devem seguir o seguinte padrão de nomenclatura `Subject.Permission[.Modifier]`, em que `Subject` corresponde ao tipo de dados que está disponível, `Permission` corresponde à ação que um utilizador pode realizar nesses dados e `Modifier` é utilizado opcionalmente para descrever especializações de outra permissão. Por exemplo: 
* Mail.Read - permite aos utilizadores lerem correio. 
* Mail.ReadWrite - permite aos utilizadores lerem ou escreverem correio.
* Mail.ReadWrite.All - permite que um administrador ou utilizador aceda a todo o correio da organização.

### <a name="client-best-practices"></a>Melhores práticas para clientes
Peça permissão apenas para os âmbitos de que a sua aplicação precisa. As aplicações com demasiadas permissões estão em risco de expor os dados dos utilizadores se ficarem comprometidas.

Os clientes não devem pedir permissões de aplicação nem permissões delegadas à mesma aplicação. Esta situação pode resultar numa elevação de privilégios e permitir que um utilizador obtenha acesso aos dados que as suas próprias permissões não permitem. 




