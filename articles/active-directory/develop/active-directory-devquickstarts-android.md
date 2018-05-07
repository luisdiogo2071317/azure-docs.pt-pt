---
title: Azure AD Android, introdução | Microsoft Docs
description: Como criar uma aplicação Android que se integra com o Azure AD para início de sessão e chamadas do Azure AD protegido APIs OAuth2.0 a utilizar.
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mtillman
editor: ''
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/30/2018
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 7af33005d5984dbf76a16a0daa92f5e1ebf85f9e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="azure-ad-android-getting-started"></a>Azure AD Android, introdução
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

Se estiver a desenvolver uma aplicação Android, Microsoft torna simples e fácil de iniciar sessão de utilizadores do Azure Active Directory (Azure AD). Azure AD permite que a aplicação para aceder a dados de utilizador através do Microsoft Graph ou a suas próprias API web do protegido.  

A biblioteca do Android do Azure AD Authentication Library (ADAL) permite a aplicação para começar a utilizar o [na nuvem do Microsoft Azure](https://cloud.microsoft.com) & [Microsoft Graph API](https://graph.microsoft.io) por suportar [ Contas do Microsoft Azure Active Directory](https://azure.microsoft.com/services/active-directory/) com da indústria padrão OAuth2 e o OpenID Connect. Este exemplo demonstra todos os os normais ciclos de vida que da aplicação deverá experimentar, incluindo:

* Obter um token para o Microsoft Graph
* Um token de atualização
* Chamar o Microsoft Graph
* Termine sessão do utilizador

Para começar, terá de um inquilino do Azure AD, onde pode criar utilizadores e registar uma aplicação. Se ainda não tiver um inquilino, [saber como obter um](active-directory-howto-tenant.md).

## <a name="scenario-sign-in-users-and-call-the-microsoft-graph"></a>Cenário: Inicie sessão em utilizadores e chame o Microsoft Graph

![Topologia](./media/active-directory-android-topology.png)

Esta aplicação pode ser utilizada para todas as contas do Azure AD. Suporta cenários únicos e múltiplos organizacionais (abordados nos passos). Demonstra como um programador pode criar aplicações para estabelecer ligação com os utilizadores empresariais e aceder aos respetivos Azure + O365 dados através do Microsoft Graph. Durante o fluxo de autenticação, os utilizadores finais terão de iniciar sessão e consentimento para as permissões da aplicação e, em alguns casos, pode exigir um admin para autorizar a aplicação. A maioria da lógica neste exemplo mostra como a autenticação de um utilizador final e faça num nível básico a chamada para o Microsoft Graph.

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

### <a name="register-and-configure-your-app"></a>Registar e configure a sua aplicação 
Terá de ter uma aplicação de cliente nativo registada com a Microsoft a utilizar o [portal do Azure](https://portal.azure.com). 

1. Introdução ao registo de aplicação
    - Navegue para o [portal do Azure](https://aad.portal.azure.com).  
    - Selecione ***do Azure Active Directory*** > ***registos de aplicação***. 

2. Criar a aplicação
    - Selecione **Novo registo de aplicação**.  
    - Introduza um nome de aplicação no **nome** campo. 
    - No **tipo de aplicação** selecione **nativo**. 
    - No **URI de redirecionamento**, introduza `http://localhost`.  

3. Configurar o Microsoft Graph
    - Selecione **definições > as permissões necessárias**.
    - Selecione **adicionar**, no **selecionar um API** selecione ***Microsoft Graph***. 
    - Selecione a permissão **iniciar sessão e ler o perfil de utilizador**, em seguida, prima **selecione** para guardar. 
        - Esta permissão é mapeado para o `User.Read` âmbito. 
    - Opcional: Interior **as permissões necessárias > Windows Azure Active Directory**, remova a permissão selecionada **iniciar sessão e ler o perfil de utilizador**. Evitará a página de consentimento do utilizador listagem duas vezes a permissão.   

4. Congrats! A aplicação está configurada com êxito. Na secção seguinte, terá de:
    - `Application ID`
    - `Redirect URI`

### <a name="get-the-sample-code"></a>Obter o código de exemplo

1. O código de clone.
    ```
    git clone https://github.com/Azure-Samples/active-directory-android
    ```
2. Abra o exemplo no Android Studio.
    - Selecione **abrir um projeto existente do Android Studio**.

### <a name="configure-your-code"></a>Configurar o seu código

Pode encontrar toda a configuração para este exemplo de código no ***src/main/java/com/azuresamples/azuresampleapp/MainActivity.java*** ficheiro.  

1. Substitua a constante `CLIENT_ID` com o `ApplicationID`.
2. Substitua a constante `REDIRECT URI` com o `Redirect URI` configurados anteriormente (`http://localhost`). 

### <a name="run-the-sample"></a>Executar o exemplo

1. Selecione **criar > apagar projeto**. 
2. Selecione **executar > aplicação execução**. 
3. A aplicação deve compilar e mostrar algumas UX. básico Ao clicar no `Call Graph API` botão, que irá solicitar a um início de sessão e, em seguida, chame automaticamente o Microsoft Graph API com o novo token.  

## <a name="important-info"></a>Informações importantes

1. Finalizar a [ADAL Android Wiki](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki) para obter mais informações sobre o mechanics de biblioteca e como configurar novos cenários e funcionalidades. 
2. Em cenários nativos, a aplicação utilizará um Webview incorporado e não sair da aplicação. O `Redirect URI` pode ser arbitrários. 
3. Localizar quaisquer problemas ou ter pedidos? Pode criar um problema ou publique no Stackoverflow com a etiqueta `azure-active-directory`. 

### <a name="cross-app-sso"></a>SSO em várias aplicações
Saiba [como ativar a SSO em várias aplicações no Android utilizando ADAL](active-directory-sso-android.md).  

### <a name="auth-telemetry"></a>Telemetria de autenticação
a biblioteca ADAL expõe auth telemetria para ajudar os programadores de aplicações como as aplicações se comportam de compreender e criar melhor experiências.  Isto permite-lhe capturar o início de sessão com êxito, os utilizadores ativos e várias outras insights interessantes. Com a telemetria de autenticação necessita que os programadores de aplicações estabelecer um serviço de telemetria para agregar e armazenar eventos.

Para obter mais informações sobre a telemetria de autenticação, consulte [telemetria de autenticação ADAL Android](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/Telemetry). 

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
