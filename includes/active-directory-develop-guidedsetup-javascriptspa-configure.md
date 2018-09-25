---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 07aac49e7aed7c95863a2058a9de3d1e8f2cd1ad
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060359"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Existem várias formas de criar uma aplicação, selecione uma delas:

### <a name="option-1-register-your-application-express-mode"></a>Opção 1: Registar a sua aplicação (modo de Express)
Agora precisa de registar a aplicação no *Portal de registo de aplicação do Microsoft*:

1.  Registar a sua aplicação através da [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Introduza um nome para o seu e-mail e a sua aplicação.
3.  Certifique-se a opção **orientada configuração** está marcada.
4.  Siga as instruções para obter o ID e colá-lo no seu código.

### <a name="option-2-register-your-application-advanced-mode"></a>Opção 2: Registar a sua aplicação (modo avançado)

1. Vá para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app) registar uma aplicação.
2. Introduza um nome para o seu e-mail e a sua aplicação.
3. Certifique-se a opção **orientada configuração** está desmarcada.
4.  Clique em **adicionar plataforma**, em seguida, selecione **Web**.
5. Adicionar a **URL de redirecionamento** que corresponde ao URL da aplicação com base no seu servidor web. Consulte as secções abaixo para obter instruções sobre como definir e obter o URL de redirecionamento no nó e o Visual Studio.
6. Selecione **Guardar**.

> #### <a name="setting-redirect-url-for-node"></a>URL de redirecionamento de definição de nó
> Para node. js, pode definir a web porta do servidor no *Server. js* ficheiro. Este tutorial utiliza a porta 30662 para referência, mas não hesite em utilizar qualquer porta disponível. Em qualquer caso, siga as instruções abaixo para configurar um URL de redirecionamento as informações de registo de aplicação:<br/>
> - Mude novamente para o *Portal de registo de aplicação* e defina `http://localhost:30662/` como uma `Redirect URL`, ou utilize `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (em que *[porta]* é a porta TCP personalizada número) e clique em "Guardar"

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio instruções para obter o URL de redirecionamento
> Siga estes passos para obter o URL de redirecionamento:
> 1.    Na **Explorador de soluções**, selecione o projeto e descobrir a **propriedades** janela. Se não vir um **propriedades** janela, prima **F4**.
> 2.    Copie o valor de **URL** na área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Mude novamente para o *Portal de registo de aplicação* e cole o valor como um **URL de redirecionamento** e selecione **guardar**


#### <a name="configure-your-javascript-spa"></a>Configurar seu SPA do JavaScript

1.  Na `index.html` ficheiro criado durante a configuração do projeto, adicione as informações de registo de aplicação. Adicione o seguinte código na parte superior dentro do `<script></script>` etiquetas no corpo do seu `index.html` ficheiro:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
<ol start="3">
<li>
Substitua <code>Enter the application Id here</code> com o Id da aplicação que acabou de registar.
</li>
</ol>
