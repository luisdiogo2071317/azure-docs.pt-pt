---
title: Como localizar uma API específica necessária para uma aplicação desenvolvida personalizado | Microsoft Docs
description: Como configurar as permissões que precisa de aceder a uma API específica no seu personalizadas desenvolvidas aplicação do Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 94f3c68e9a1433d2a433e2a3bbe557b487e7bf75
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332063"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Como localizar uma API específica necessária para uma aplicação desenvolvida personalizada

O acesso a APIs necessita de configuração de âmbitos de acesso e de funções. Se pretende expor recursos aplicação web APIs para aplicações cliente, terá de configurar as funções e âmbitos de acesso para a API. Se pretender que uma aplicação de cliente para aceder a uma API web, terá de configurar permissões para aceder a API no registo de aplicação.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configurar uma aplicação de recursos para expor as APIs web

Quando expor web API, a API apresentado no **selecionar um API** lista ao adicionar permissões a um registo de aplicação. Para adicionar os âmbitos de acesso, siga os passos descritos em [adicionar âmbitos de acesso à sua aplicação de recurso](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configurar uma aplicação de cliente para aceder a APIs web

Ao adicionar permissões ao registo da aplicação, pode **adicionar acesso à API** para web expostas APIs. Para aceder a web APIs, siga os passos descritos em [adicionar credenciais ou permissões para aceder a web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Passos Seguintes

-   [Integração de aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Compreender o manifesto da aplicação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


