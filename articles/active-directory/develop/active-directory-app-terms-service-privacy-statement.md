---
title: Termos de serviço e declaração de privacidade para as aplicações do Azure AD | Microsoft Docs
description: Saiba como pode configurar os termos de serviço e declaração de privacidade para aplicações registadas para utilizar o Azure AD.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: celested
ms.reviwer: lenalepa, sureshja
ms.custom: aaddev
ms.openlocfilehash: 2dff8c25ae0704a15cf02eec7a380b6d894b9ce4
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316455"
---
# <a name="terms-of-service-and-privacy-statement-for-registered-azure-active-directory-apps"></a>Termos de serviço e declaração de privacidade para aplicações do Azure Active Directory registados

Os programadores que criarem e gerir aplicações que se integram com o Azure Active Directory (Azure AD) e contas Microsoft devem incluir ligações para os termos da aplicação de serviço e declaração de privacidade. Os termos de serviço e declaração de privacidade são apresentados aos utilizadores através de experiência de consentimento do utilizador. Ajudam os utilizadores que eles podem confiam a aplicação. Os termos de serviço e declaração de privacidade são especialmente importantes para destinada ao utilizador multi-inquilino aplicações – aplicações que são utilizadas por vários diretórios ou estão disponíveis para qualquer conta Microsoft.

É responsável pela criação de termos de serviço e privacidade documentos instrução para a sua aplicação e para fornecer os URLs a estes documentos. Para aplicações de multi-inquilinos que falham para fornecer estas ligações, a experiência de consentimento do utilizador para a sua aplicação irá mostrar um alerta, o que poderá desencorajar-os utilizadores de consenting à sua aplicação.

> [!NOTE]
> * Aplicações de inquilino único não irão mostrar um alerta.
> * Se uma ou ambas as duas ligações estão em falta, a aplicação irá apresentar um alerta.

## <a name="user-consent-experience"></a>Experiência de consentimento do utilizador

Os exemplos seguintes mostram o utilizador consentimento experiência quando os termos de serviço e declaração de privacidade são configurados e quando estas ligações não estão configuradas.

![Capturas de ecrã com e sem uma declaração de privacidade e termos de serviço fornecido](./media/active-directory-integrating-applications/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Formatação ligações para os termos de documentos de declaração de privacidade e de serviço

Antes de adicionar as ligações para os termos da sua aplicação de documentos de declaração de privacidade e de serviço, certifique-se que os URLs siga estas diretrizes.

| Orientação     | Descrição                           |
|---------------|---------------------------------------|
| Formato        | URL válido                             |
| Esquemas válidas | HTTP e HTTPS</br>Recomendamos HTTPS |
| Comprimento máximo    | 2048 carateres                       |

Exemplos: `https://myapp.com/terms-of-service` e `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>A adição de ligações para os termos de serviço e declaração de privacidade

Quando os termos de serviço e declaração de privacidade estiverem prontos, pode adicionar ligações a estes documentos na sua aplicação utilizando um dos seguintes métodos:
* [Através do portal do Azure](#registered-in-azure-portal)
* [No Portal de registo de aplicação ou Dev Center](#registered-in-app-reg-portal)
* [Utilizando o objeto de aplicação JSON](#app-object-json)
* [Utilizar a REST API de beta MSGraph](#msgraph-beta-rest-api)

### <a name="registered-in-azure-portal"></a>Se registou a aplicação no portal do Azure
Siga estes passos se registou a aplicação no portal do Azure.

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/).
2. Navegue para o **registos de aplicação** secção e selecione a sua aplicação.
3. Abra o **propriedades** secção da aplicação.
4. Preencha o **termos do URL do serviço** e **URL da declaração de privacidade** campos.
5. Guarde as alterações.

![Secção de propriedades de aplicação com os termos de URLs de declaração de privacidade e de serviço](./media/active-directory-integrating-applications/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="registered-in-app-reg-portal"></a>Se registou a aplicação no Portal de registo de aplicação
Siga estes passos se registou a aplicação no Portal de registo de aplicação ou Dev Center.

1. Iniciar sessão para o [Portal de registo de aplicação](https://apps.dev.microsoft.com/).
2. Selecione a sua aplicação e desloque-se para o **perfil** secção.
3. Preencha o **termos do URL do serviço** e **URL da declaração de privacidade** campos.
4. Guarde as alterações.

![Secção de perfil de aplicação com os termos de URLs de declaração de privacidade e de serviço](./media/active-directory-integrating-applications/app-registration-portal-profile-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>Utilizando o objeto de aplicação JSON
Se preferir modificar diretamente o objeto de aplicação JSON, pode utilizar o editor de manifesto no portal do Azure ou o Portal de registo de aplicação para incluir ligações para os termos da sua aplicação de serviço e declaração de privacidade.

```json
    "informationalUrls": { 
        "termsOfService": “<your_terms_of_service_url>”, 
        "privacy": “<your_privacy_statement_url>” 
    }
```

### <a name="msgraph-beta-rest-api"></a>Utilizar a REST API de beta MSGraph
Para atualizar programaticamente todas as suas aplicações, pode utilizar o beta MSGraph REST API para atualizar todas as suas aplicações para incluir ligações para os termos de documentos de declaração de privacidade e de serviço.

```
PATCH https://graph.microsoft.com/beta/applications/{application id}
{ 
    "appId": "{your application id}", 
    "info": { 
        "termsOfServiceUrl": "<your_terms_of_service_url>", 
        "supportUrl": null, 
        "privacyStatementUrl": "<your_privacy_statement_url>", 
        "marketingUrl": null, 
        "logoUrl": null 
    }
}
```

> [!NOTE]
> * Tenha cuidado não de substituir quaisquer valores pré-existente que atribuiu a qualquer um destes campos: `supportUrl`, `marketingUrl`, e `logoUrl`
> * A REST API de beta MSGraph funcionará apenas quando iniciar sessão com uma conta do Azure AD. Contas Microsoft pessoais não são suportadas.