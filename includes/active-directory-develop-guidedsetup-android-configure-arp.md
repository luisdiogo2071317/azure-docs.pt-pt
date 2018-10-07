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
ms.openlocfilehash: fe2b02b2495b4f37cbc90e1ddbeaca43b41d008c
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842943"
---
## <a name="add-the-applications-registration-to-your-code"></a>Adicionar o registo da aplicação ao seu código

Neste passo, terá de adicionar a aplicação / o ID de cliente ao seu projeto.

1.  Open `MainActivity` (sob `app`  >  `java`  >  *`{host}.{namespace}`*)
2.  Substitua a linha que começa com `final static String CLIENT_ID` por:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Abrir: `app` > `manifests` > `AndroidManifest.xml`
4. Adicionar a atividade seguinte seja `manifest\application`. O`BrowserTabActivity` permite que a Microsoft retorne a chamada para a sua aplicação depois de concluir a autenticação:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
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

