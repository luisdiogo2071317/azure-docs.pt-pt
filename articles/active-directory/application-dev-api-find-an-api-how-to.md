---
title: Como encontrar uma API específica necessária para uma aplicação personalizada desenvolvida | Documentos da Microsoft
description: Como configurar as permissões que necessárias para aceder a uma API específica em seu custom desenvolveu a aplicação do Azure AD
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
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: f15e84d6efa30d2e66c7ff1d92551bece94938e7
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363351"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Como encontrar uma API específica necessária para uma aplicação personalizada desenvolvida

O acesso a APIs exigem a configuração das funções e âmbitos de acesso. Se quiser expor seu recurso aplicação APIs web para aplicativos cliente, terá de configurar âmbitos de acesso e as funções para a API. Se pretender que uma aplicação de cliente para aceder a uma API web, tem de configurar permissões para aceder à API do registo de aplicação.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configurar uma aplicação de recurso para expor as APIs web

Quando expõe a web API, a API seja apresentado no **selecionar uma API** lista ao adicionar permissões a um registo de aplicações. Para adicionar âmbitos de acesso, siga os passos descritos em [adicionando os âmbitos de acesso à sua aplicação de recurso](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configurar uma aplicação de cliente para aceder a APIs web

Quando adicionar permissões ao registo da aplicação, pode **adicionar acesso à API** para APIs da web expostas. Para aceder a web APIs, siga os passos descritos em [adicionar credenciais ou permissões para aceder a web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Passos Seguintes

-   [Integração de aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Compreender o manifesto da aplicação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


