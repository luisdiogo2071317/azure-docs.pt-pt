---
title: Autenticação com o Azure Maps | Documentos da Microsoft
description: Autenticação para utilizar os serviços do Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b97c303433eb8fadcda51257d37447f052ce4a3b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56119283"
---
# <a name="authentication-with-azure-maps"></a>Autenticação com o Azure Maps

Mapas do Azure suporta duas formas de autenticar pedidos. Partilhado chave ou o Azure Active Directory (Azure AD) oferta métodos distintos para autorizar a cada pedido enviado para o Azure Maps. O objetivo deste artigo é explicar a ambos os métodos de autenticação para ajudar a orientar a sua implementação de autenticação.

## <a name="shared-key-authentication"></a>Autenticação de chave partilhada

Autenticação de chave partilhada depende de passar chaves de conta gerada do Azure Maps com cada solicitação para o Azure Maps.  Duas chaves são geradas quando é criada a sua conta do Azure Maps.  Cada solicitação de serviços do Azure Maps requer a chave de subscrição para ser adicionado como um parâmetro para o URL.

> [!Tip]
> Recomendamos a regeneração das chaves regularmente. São fornecidas duas chaves para que possa manter as ligações utilizando uma chave enquanto Regenera a outra. Quando regenerar as chaves, tem de atualizar quaisquer aplicações que acedam esta conta para utilizar as novas chaves.

Para ver as suas chaves, consulte [detalhes de autenticação](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Autenticação com o Azure Active Directory (pré-visualização)

O Azure oferece agora de mapas [do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) integração para a autenticação de pedidos para os serviços do Azure Maps.  O Azure AD fornece incluindo autenticação baseada em identificar [controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) para conceder acesso de nível utilizador ou aplicação aos recursos do Azure Maps. O objetivo deste artigo é ajudar a compreender os conceitos e componentes de integração do Azure Maps do Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticação com tokens de acesso de OAuth

Mapas do Azure aceita **OAuth 2.0** tokens de acesso para inquilinos do Azure AD associados à subscrição do Azure que contém uma conta do Azure Maps.  Mapas do Azure aceita tokens para:

* Utilizadores do Azure AD 
* Aplicações de terceiros usando permissões delegadas pelos utilizadores
* Identidades geridas para os recursos do Azure

O Azure Maps gera um `unique identifier (client ID)` para cada conta do Azure Maps.  Quando o ID de cliente é combinado com parâmetros adicionais, os tokens podem ser pedidos do Azure AD, especificando o valor abaixo:

```
https://login.microsoftonline.com
```
Para obter mais informações sobre como configurar o Azure AD e solicitar tokens para o Azure Maps, consulte [como para gerir a autenticação](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Para obter informações gerais sobre como solicitar tokens do Azure AD, consulte [Noções básicas de autenticação no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="requesting-azure-map-resources-with-oauth-tokens"></a>Solicitar recursos de mapa do Azure com tokens de OAuth

Depois de um token é obtido a partir do Azure AD, pode ser enviado um pedido para o Azure Maps, com o seguinte conjunto de cabeçalhos de solicitação necessários dois:

| Cabeçalho do pedido    |    Value    |
|:------------------|:------------|
| x-MS-client-ID    | 30d7cc….9f55|
| Autorização     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` é a conta do Azure Maps com base em GUID apresentado na página de autenticação do Azure Maps

Abaixo é exemplo de pedido de rota de mapas do Azure com o OAuth token:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Para ver o seu ID de cliente, consulte [detalhes de autenticação](https://aka.ms/amauthdetails).

## <a name="control-access-with-role-based-access-control-rbac"></a>Controlar o acesso com controlo de acesso baseado em funções (RBAC)

Um recurso chave do Azure AD é controlar o acesso a recursos protegidos através do RBAC. Depois de criar a conta do Azure Maps e registar a aplicação de mapas do Azure do Azure AD no seu inquilino do Azure AD, agora é possível configurar o RBAC para um utilizador, aplicação ou recurso do Azure na página de portal de conta de mapa do Azure. 

Mapas do Azure suporta atualmente o controlo de acesso de leitura para indivíduo utilizadores do Azure AD, aplicações ou serviços do Azure através de identidades de gerido para os recursos do Azure.

![Conceito](./media/azure-maps-authentication/concept.png)

Para ver as definições de RBAC, veja [como configurar o RBAC para o Azure Maps](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identidades geridas para recursos do Azure e o Azure Maps

[Gerido identidades para recursos do Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) fornecer serviços do Azure (Azure App service, as funções do Azure, máquinas virtuais, etc.) com uma identidade gerida automaticamente, que pode ser autorizada para acesso aos serviços do Azure Maps.  

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como autenticar uma aplicação com o Azure AD e o Azure Maps, consulte [como para gerir a autenticação](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Para saber mais sobre autenticação, o Azure Maps, o controle de mapa e o Azure AD, veja [do Azure AD e o controle de mapa do Azure Maps](https://aka.ms/amaadmc).