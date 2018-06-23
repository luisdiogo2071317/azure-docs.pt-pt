---
title: Como configurar uma nova aplicação multi-inquilino | Microsoft Docs
description: Como configurar o início de sessão para uma aplicação personalizada que estiver a desenvolver e a registar com o Azure AD.
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
ms.openlocfilehash: c5e46262c0099dfa7b4522cad5fc3668cd1d69f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333940"
---
# <a name="how-to-configure-a-new-multi-tenant-application"></a>Como configurar uma nova aplicação multi-inquilino

Ativar federado-início de sessão único (SSO) na sua aplicação é ativado automaticamente quando Federação através do Azure AD para o OpenID Connect, SAML 2.0 ou WS Fed. Se os utilizadores finais são ter de iniciar sessão, apesar de já ter uma sessão existente com o Azure AD, é provável que a aplicação pode estar configurado incorretamente.

* Se estiver a utilizar a ADAL/MSAL, certifique-se de que tem **PromptBehavior** definido como **automática** vez **sempre**.

* Se estiver a criar uma aplicação móvel, poderá ter configurações adicionais para permitir SSO mediado ou não mediada.

Para Android, consulte [ativar cruzada SSO de aplicação no Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

Para iOS, consulte [ativar cruzada SSO de aplicações no iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Passos Seguintes

[SSO do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Ativar cruzada SSO de aplicação no Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Ativar cruzada SSO de aplicações no iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integrar aplicações AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Consentimento e Permissioning para v 2.0 de AzureAD convergido aplicações](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
