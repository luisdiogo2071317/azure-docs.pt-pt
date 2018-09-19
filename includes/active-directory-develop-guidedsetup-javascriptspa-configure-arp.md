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
ms.openlocfilehash: fc06da3b1ad66aa15237a25d2f50374043c860ba
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293609"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação à sua aplicação

Neste passo, terá de configurar o URL de redirecionamento das suas informações de registo de aplicação e, em seguida, adicione o ID da aplicação ao seu aplicativo SPA do JavaScript.

### <a name="configure-redirect-url"></a>Configurar o URL de redirecionamento

Configurar o `Redirect URL` campo com o URL para a sua página Index. HTML com base no seu servidor web, em seguida, clique em *atualização*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio instruções para obter URL de redirecionamento
> Para obter o seu URL de redirecionamento:
> 1.    Na *Explorador de soluções*, selecione o projeto e descobrir a `Properties` janela (se não vir uma janela de propriedades, prima `F4`)
> 2.    Copie o valor de `URL` na área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Cole o valor como um `Redirect URL` na parte superior desta página, em seguida, clique em `Update`

<p/>

> #### <a name="setting-redirect-url-for-node"></a>URL de redirecionamento de definição de nó
> Para node. js, pode definir a web porta do servidor no *Server. js* ficheiro. Este tutorial utiliza a porta 30662 para referência, mas não hesite em utilizar qualquer porta disponível. Em qualquer caso, utilize as seguintes instruções para configurar um URL de redirecionamento as informações de registo de aplicação:<br/>
> Definir `http://localhost:30662/` como uma `Redirect URL` na parte superior desta página ou utilize `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (em que *[porta]* é o número de porta TCP personalizado) e, em seguida, clique em 'Atualizar'

### <a name="configure-your-javascript-spa-application"></a>Configurar a sua aplicação SPA do JavaScript

1.  Crie um ficheiro denominado `msalconfig.js` que contém as informações de registo de aplicação. Se estiver a utilizar o Visual Studio, selecione o projeto (pasta raiz do projeto), o botão direito do mouse e selecione: `Add`  >  `New Item`  >  `JavaScript File`. Nomeie-o `msalconfig.js`
2.  Adicione o seguinte código para seu `msalconfig.js` ficheiro:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
