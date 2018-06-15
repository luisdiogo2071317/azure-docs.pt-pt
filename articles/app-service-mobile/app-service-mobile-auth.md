---
title: Autenticação e autorização no serviço de aplicações do Azure para aplicações móveis | Microsoft Docs
description: Referência conceptual e descrição geral sobre a autenticação / autorização funcionalidades do App Service do Azure, especificamente para aplicações móveis
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 237310c607eb8488e53631b6e69d01703d1ebf99
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30839733"
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Autenticação e autorização no serviço de aplicações do Azure para aplicações móveis

Este artigo descreve como a autenticação e autorização funciona quando desenvolver as aplicações móveis nativas com um back-end do serviço de aplicações. O App Service fornece integrada de autenticação e autorização, pelo que as suas aplicações móveis podem iniciar sessão utilizadores sem alterar qualquer código no App Service. Fornece uma forma fácil para proteger a sua aplicação e trabalhar com dados por utilizador. 

Este artigo incida no desenvolvimento de aplicações móveis. Para começar a trabalhar rapidamente com autenticação do serviço de aplicações e a autorização para a sua aplicação móvel, consulte um dos seguintes tutoriais [adicionar autenticação à sua aplicação iOS] [ iOS] (ou [Android], [Windows], [xamarin. IOS], [xamarin. Android], [xamarin. Forms], ou [Cordova ]). 

Para obter informações sobre como a autenticação e autorização funcionam no App Service, consulte [autenticação e autorização no serviço de aplicações do Azure](../app-service/app-service-authentication-overview.md).

## <a name="authentication-with-provider-sdk"></a>Autenticação com o fornecedor de SDK

Depois de está tudo configurado no App Service, pode modificar os clientes móveis iniciar sessão com o serviço de aplicações. Existem duas abordagens aqui:

* Utilize um SDK que publica um fornecedor de identidade fornecido para estabelecer a identidade e, em seguida, obter acesso ao serviço de aplicações.
* Utilize uma única linha de código, para que o SDK do cliente de Mobile Apps pode iniciar sessão de utilizadores.

> [!TIP]
> A maioria das aplicações devem utilizar um fornecedor de SDK para obter uma experiência mais consistente quando os utilizadores iniciarem sessão, para utilizar o suporte de token de atualização bem como obter outras vantagens que especifica o fornecedor.
> 
> 

Quando utiliza um fornecedor de SDK, os utilizadores poderem iniciar sessão para uma experiência que integra-se mais rigidamente com o sistema operativo que a aplicação está em execução no. Este método também dá-lhe um token de fornecedor e algumas informações de utilizador no cliente, o que torna mais fácil e consumir APIs do gráfico e personalizar a experiência de utilizador. Ocasionalmente em blogues e fóruns, este é referido como "fluxo de cliente" ou "fluxo direcionadas para o cliente" porque o código no cliente inicia sessão dos utilizadores e o código de cliente tem acesso a um token de fornecedor.

Depois de um token de fornecedor é obtido, tem de ser enviadas para o App Service para validação. Depois do serviço de aplicações valida o token, o App Service cria um novo token de serviço de aplicações, que é devolvido ao cliente. O SDK do cliente de Mobile Apps tem métodos de programa auxiliar para gerir este exchange e anexar automaticamente o token para todos os pedidos para a aplicação de back-end. Os programadores também podem manter uma referência para o token de fornecedor.

Para obter mais informações sobre o fluxo de autenticação, consulte [fluxo de autenticação do serviço de aplicações](../app-service/app-service-authentication-overview.md#authentication-flow). 

## <a name="authentication-without-provider-sdk"></a>Autenticação sem fornecedor de SDK

Se não pretender configurar um fornecedor de SDK, pode permitir que a funcionalidade de aplicações móveis do serviço de aplicações do Azure para iniciar sessão para si. O SDK do cliente de Mobile Apps irá abrir uma vista web para o fornecedor da sua escolha e o utilizador iniciar sessão. Ocasionalmente em blogues e fóruns, é denominado o "fluxo de servidor" ou "direcionadas para o servidor de fluxo" porque o servidor gere o processo que inicia sessão dos utilizadores e o cliente SDK nunca recebe o token de fornecedor.

Código para iniciar este fluxo está incluído no tutorial autenticação para cada plataforma. No final do fluxo, o cliente SDK tem um token de serviço de aplicações e o token é automaticamente anexado a todos os pedidos para o back-end da aplicação.

Para obter mais informações sobre o fluxo de autenticação, consulte [fluxo de autenticação do serviço de aplicações](../app-service/app-service-authentication-overview.md#authentication-flow). 
## <a name="more-resources"></a>Mais recursos

Os tutoriais seguintes mostram como adicionar autenticação para os seus clientes móveis utilizando o [direcionadas para o servidor de fluxo](../app-service/app-service-authentication-overview.md#authentication-flow):

* [Adicionar autenticação à sua aplicação iOS][iOS]
* [Adicionar autenticação à sua aplicação Android][Android]
* [Adicionar autenticação à sua aplicação do Windows][Windows]
* [Adicionar autenticação à sua aplicação xamarin. IOS][xamarin. IOS]
* [Adicionar autenticação à sua aplicação xamarin. Android][xamarin. Android]
* [Adicionar autenticação à sua aplicação xamarin. Forms][xamarin. Forms]
* [Adicionar autenticação à sua aplicação Cordova][Cordova ]

Utilize os seguintes recursos, se pretender utilizar o [direcionadas para o cliente fluxo](../app-service/app-service-authentication-overview.md#authentication-flow) do Azure Active Directory:

* [Utilizar a biblioteca de autenticação do Active Directory para iOS][ADAL-iOS]
* [Utilize a biblioteca de autenticação do Active Directory para Android][ADAL-Android]
* [Utilizar a biblioteca de autenticação do Active Directory para o Windows e Xamarin][ADAL-dotnet]

Utilize os seguintes recursos, se pretender utilizar o [direcionadas para o cliente fluxo](../app-service/app-service-authentication-overview.md#authentication-flow) para Facebook:

* [Utilizar o SDK do Facebook para iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Utilize os seguintes recursos, se pretender utilizar o [direcionadas para o cliente fluxo](../app-service/app-service-authentication-overview.md#authentication-flow) para Twitter:

* [Utilizar recursos de infraestrutura do Twitter para iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Utilize os seguintes recursos, se pretender utilizar o [direcionadas para o cliente fluxo](../app-service/app-service-authentication-overview.md#authentication-flow) para o Google:

* [Utilizar o SDK do início de sessão Google para iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[xamarin. IOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[xamarin. Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[xamarin. Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova ]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
