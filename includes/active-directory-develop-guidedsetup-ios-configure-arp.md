---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: ff465891075bbb16774b7ecad306de222c2f2a7c
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46473840"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação à sua aplicação

Neste passo, terá de adicionar o ID da aplicação ao seu projeto:

1.  Na `ViewController.swift`, substitua a linha que começa com "`let kClientID`' com:
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
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
            <string>msal[Enter the application Id here]</string>
        </array>
    </dict>
</array>
```
