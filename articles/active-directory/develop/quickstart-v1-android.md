---
title: O Azure AD Android, introdução | Documentos da Microsoft
description: Como criar uma aplicação Android que se integra com o Azure AD para início de sessão e as chamadas do Azure AD protegidos as APIs com o OAuth2.0.
services: active-directory
documentationcenter: android
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: cdd0d9ccff608d5882480d1394e2188579cefe75
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576805"
---
# <a name="azure-ad-android-getting-started"></a>O Azure AD Android, introdução
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Se estiver a desenvolver uma aplicação Android, a Microsoft torna simples e fácil de iniciar sessão dos utilizadores do Azure Active Directory (Azure AD). O Azure AD permite que a sua aplicação para aceder a dados de utilizador através do Microsoft Graph ou sua própria API web protegida. 

A biblioteca do Android do Azure AD Authentication Library (ADAL) permite que a aplicação começar a utilizar o [Cloud do Microsoft Azure](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) ao suportar [ Contas do Microsoft Azure Active Directory](https://azure.microsoft.com/services/active-directory/) com o setor padrão OAuth2 e o OpenID Connect. Este exemplo demonstra todas as normais ciclos de vida de que seu aplicativo deve experimentar, incluindo:

* Obter um token para o Microsoft Graph
* Um token de atualização
* Chamar o Microsoft Graph
* Terminar sessão do utilizador

Para começar, terá de inquilino do Azure AD, onde pode criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino [Saiba como obter um](quickstart-create-new-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Cenário: Iniciar sessão dos utilizadores e chamar o Microsoft Graph

![Topologia](./media/quickstart-v1-android/active-directory-android-topology.png)

Esta aplicação pode ser utilizada para todas as contas do Azure AD. Ele oferece suporte a cenários de únicos e com vários organizacionais (discutidos nos passos). Ele demonstra como um desenvolvedor pode criar aplicações para ligar a utilizadores empresariais e aceder aos seus Azure + O365 dados por meio do Microsoft Graph. Durante o fluxo de autenticação, os utilizadores finais será necessário para iniciar sessão e consentimento às permissões da aplicação e, em alguns casos, pode exigir um administrador para autorizar a aplicação. A maioria da lógica neste exemplo mostra como a autenticação de um utilizador final e faça a básico chamar para o Microsoft Graph.

## <a name="example-code"></a>Código de exemplo

Pode encontrar o código de exemplo completo [no Github](https://github.com/Azure-Samples/active-directory-android). 

```Java
// Initialize your app with MSAL
AuthenticationContext mAuthContext = new AuthenticationContext(
        MainActivity.this, 
        AUTHORITY, 
        false);


// Perform authentication requests
mAuthContext.acquireToken(
    getActivity(), 
    RESOURCE_ID, 
    CLIENT_ID, 
    REDIRECT_URI,  
    PromptBehavior.Auto, 
    getAuthInteractiveCallback());

// ...

// Get tokens to call APIs like the Microsoft Graph
mAuthResult.getAccessToken()
```

## <a name="steps-to-run"></a>Passos para executar

### <a name="register-and-configure-your-app"></a>Registar e configurar a sua aplicação 
Terá de ter uma aplicação de cliente nativo registrada com a Microsoft a utilizar o [portal do Azure](https://portal.azure.com). 

1. Conhecendo o registo de aplicações
    - Navegue para o [portal do Azure](https://aad.portal.azure.com). 
    - Selecione ***do Azure Active Directory*** > ***registos das aplicações***. 

2. Criar a aplicação
    - Selecione **Novo registo de aplicação**. 
    - Introduza um nome de aplicação na **nome** campo. 
    - Na **tipo de aplicação** selecionar **nativo**. 
    - Na **URI de redirecionamento**, introduza `http://localhost`. 

3. Configurar o Microsoft Graph
    - Selecione **definições > permissões obrigatórias**.
    - Selecione **Add**, no **selecionar uma API** selecionar ***Microsoft Graph***. 
    - Selecione a permissão **iniciar sessão e ler o perfil de utilizador**, em seguida, pressione **selecione** para guardar. 
        - Esta permissão é mapeado para o `User.Read` âmbito. 
    - Opcional: Lado de dentro **permissões obrigatórias > Windows Azure Active Directory**, remova a permissão selecionada **iniciar sessão e ler o perfil de utilizador**. Isso evitará a página de consentimento do utilizador listagem a permissão duas vezes. 

4. Parabéns! A aplicação é configurada com êxito. Na secção seguinte, terá de:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>Obter o código de exemplo

1. Clone o código.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Abra o exemplo no Android Studio.
    - Selecione **abrir um projeto existente do Android Studio**.

### <a name="configure-your-code"></a>Configurar o seu código

Pode encontrar toda a configuração para este exemplo de código no ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** ficheiro. 

1. Substitua a constante `CLIENT_ID` com o `ApplicationID`.
2. Substitua a constante `REDIRECT URI` com o `Redirect URI` que configurou anteriormente (`http://localhost`). 

### <a name="run-the-sample"></a>Executar o exemplo

1. Selecione **criar > Limpar projeto**. 
2. Selecione **executar > Executar aplicação**. 
3. A aplicação deve criar e mostrar alguns experiência do usuário básica. Quando clica no `Call Graph API` botão, ele irá pedir para um início de sessão e, em seguida, chamar silenciosamente o Microsoft Graph API com o novo token. 

## <a name="important-info"></a>Informações importantes

1. Check-Out do [Wiki Android da ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) para obter mais informações sobre a mecânica de biblioteca e como configurar novos cenários e capacidades. 
2. Em cenários nativos, a aplicação irá utilizar uma Webview incorporado e não deixe a aplicação. O `Redirect URI` pode ser arbitrário. 
3. Localizar quaisquer problemas ou têm pedidos? Pode criar um problema ou publicar no Stackoverflow com a marca `azure-active-directory`. 

### <a name="cross-app-sso"></a>SSO em vária aplicações
Saiba mais [como ativar o SSO em vária aplicações em dispositivos Android ao utilizar a ADAL](howto-v1-enable-sso-on-android.md). 

### <a name="auth-telemetry"></a>Telemetria de autenticação
a biblioteca ADAL expõe a telemetria de autenticação para o ajudar a compreender como as aplicações estão se comportando e criar melhores experiências desenvolvedores de aplicativos. Isto permite-lhe capturar o início de sessão com êxito, os utilizadores ativos e várias outras informações interessantes. Com a telemetria de autenticação, é necessário estabelecer um serviço de telemetria para agregar e armazenar eventos, os programadores de aplicações.

Para obter mais informações sobre a telemetria de autenticação, Check-out [telemetria de autenticação da ADAL Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
