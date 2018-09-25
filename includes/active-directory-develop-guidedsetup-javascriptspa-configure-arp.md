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
ms.openlocfilehash: eead4c6a66a317c7404205415cbf04c442ffe8d1
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060630"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação à sua aplicação

Neste passo, terá de configurar o URL de redirecionamento das suas informações de registo de aplicação e, em seguida, adicione o ID da aplicação ao seu aplicativo SPA do JavaScript.

### <a name="configure-redirect-url"></a>Configurar o URL de redirecionamento

Configurar o `Redirect URL` campo com o URL para a sua página Index. HTML com base no seu servidor web, em seguida, clique em *atualização*.


> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio instruções para obter o URL de redirecionamento
> Siga estes passos para obter o URL de redirecionamento:
> 1.    Na **Explorador de soluções**, selecione o projeto e descobrir a **propriedades** janela. Se não vir um **propriedades** janela, prima **F4**.
> 2.    Copie o valor de **URL** na área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Cole o valor como um **URL de redirecionamento** na parte superior desta página, em seguida, clique em **Update**

<p/>

> #### <a name="setting-redirect-url-for-node"></a>URL de redirecionamento de definição de nó
> Para node. js, pode definir a web porta do servidor no *Server. js* ficheiro. Este tutorial utiliza a porta 30662 para referência, mas pode usar qualquer outra porta disponível. Siga as instruções abaixo para configurar um URL de redirecionamento as informações de registo de aplicação:<br/>
> Definir `http://localhost:30662/` como uma **URL de redirecionamento** na parte superior desta página ou utilização `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (onde *[porta]* é o número de porta TCP personalizado) e, em seguida, clique em  **Atualização**

### <a name="configure-your-javascript-spa-application"></a>Configurar a sua aplicação SPA do JavaScript

1.  Na `index.html` ficheiro criado durante a configuração do projeto, adicione as informações de registo de aplicação. Adicione o seguinte código na parte superior dentro do `<script></script>` etiquetas no corpo do seu `index.html` ficheiro:

```javascript
var applicationConfig = {
    clientID: "[Enter the application Id here]",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
