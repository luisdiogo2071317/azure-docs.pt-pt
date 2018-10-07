---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: brandwe
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: brandwe
ms.custom: include file
ms.openlocfilehash: 331d16df55e26df5d49555c636b307499dd052af
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842932"
---
## <a name="register-your-application"></a>Registar a sua aplicação
Pode registar a sua aplicação em qualquer uma das duas formas, conforme descrito nas próximas duas secções.

### <a name="option-1-express-mode"></a>Opção 1: Modo de Express
Agora precisa de registar a aplicação no *Portal de registo de aplicação do Microsoft*:
1. Registar a sua aplicação através do [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Introduza um nome para a sua aplicação e o seu e-mail
3.  Certifique-se a opção para a configuração interativa
4.  Siga as instruções para obter o ID da aplicação e colá-lo no seu código

### <a name="option-2-advanced-mode"></a>Opção 2: Modo avançado

1.  Aceda a [Portal de registo de aplicações da Microsoft](https://apps.dev.microsoft.com/portal/register-app)
2.  Introduza um nome para a sua aplicação
3.  Certifique-se que a opção para a configuração interativa está desmarcada
4.  Clique em `Add Platform`, em seguida, selecione `Native Application` e clique em `Save`
5.  Volte para o Xcode. Na `ViewController.swift`, substitua a linha que começa com "`let kClientID`' com o ID da aplicação que acabou de registar:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Controlar + clique <code>Info.plist</code> para abrir o menu contextual e, em seguida, clique em: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
Sob o <code>dict</code> nó raiz, adicione o seguinte:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Substitua <i> <code>[Your_Application_Id_Here]</code> </i> com o Id da aplicação que acabou de registar
</li>
</ol>
