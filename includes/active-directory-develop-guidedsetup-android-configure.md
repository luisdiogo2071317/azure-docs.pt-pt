---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7ff04789a4ba5e5a689b3d3815852bc0fbcdc6a7
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988377"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Pode registar a sua aplicação em qualquer uma das duas formas, conforme descrito nas próximas duas secções.

### <a name="option-1-express"></a>Opção 1: Express

1. Aceda ao [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2. Na **nome da aplicação**, introduza um nome para a sua aplicação.
3. Certifique-se de que o **configuração interativa** caixa de verificação está selecionada e, em seguida, selecione **criar**.
4. Siga as instruções para obter o ID da aplicação e colá-lo no seu código.

### <a name="option-2-advanced"></a>Opção 2: avançada

1. Aceda ao [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app).
2. Na caixa **Nome da Aplicação**, introduza um nome para a sua aplicação.
3. Certifique-se de que a caixa de verificação **Configuração Assistida** está desmarcada e, em seguida, selecione **Criar**.
4. Selecione **Adicionar Plataforma**, selecione **Aplicação Nativa** e, em seguida, selecione **Guardar**.
5. Sob **app** > **java** > **{anfitrião}. { espaço de nomes}**, abra `MainActivity`. 
6. Substitua *[Introduza o Id de aplicação aqui]* com a sua aplicação / ID de cliente:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Sob **app** > **manifestos**, abra o *androidmanifest. XML* ficheiro.
8. Na `manifest\application`, adicionar a atividade seguinte. O `BrowserTabActivity` atividade que permite que a Microsoft chamada de retorno para a sua aplicação depois de terminar a autenticação:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            
            <!--Add in your scheme/host from registered redirect URI-->
            <!--By default, the scheme should be similar to 'msal[appId]' -->
            <data android:scheme="msal[Enter the application Id here]"
                android:host="auth" />
        </intent-filter>
    </activity>
    ```
<!-- Workaround for Docs conversion bug -->
9. Na `BrowserTabActivity`, substitua `[Enter the application Id here]` com a aplicação / ID de cliente.
