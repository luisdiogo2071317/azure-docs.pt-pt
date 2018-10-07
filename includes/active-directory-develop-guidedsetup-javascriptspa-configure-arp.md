---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: c2b86e79f0364ee84e01fee5e9837db5a6b618a2
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843540"
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
