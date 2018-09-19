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
ms.openlocfilehash: 569a7e74df3016fae133066607fdc0bc32c0044d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293538"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Existem várias formas de criar uma aplicação, selecione uma delas:

### <a name="option-1-register-your-application-express-mode"></a>Opção 1: Registar a sua aplicação (modo de Express)
Agora precisa de registar a aplicação no *Portal de registo de aplicação do Microsoft*:

1.  Registar a sua aplicação através do [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Introduza um nome para a sua aplicação e o seu e-mail
3.  Certifique-se a opção *orientada configuração* está marcada
4.  Siga as instruções para obter o ID da aplicação e colá-lo no seu código

### <a name="option-2-register-your-application-advanced-mode"></a>Opção 2: Registar a sua aplicação (modo avançado)

1. Vá para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app) registar uma aplicação
2. Introduza um nome para a sua aplicação e o seu e-mail 
3. Certifique-se a opção *orientada configuração* está desmarcada
4.  Clique em `Add Platform`, em seguida, selecione `Web`
5. Adicionar o `Redirect URL` que correspondem ao URL da aplicação com base no seu servidor web. Veja as secções abaixo para obter instruções sobre como definir / obter o URL de redirecionamento no Visual Studio e Python.
6. Clicar em *Guardar*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio instruções para obter URL de redirecionamento
> Siga as instruções para obter o seu URL de redirecionamento:
> 1.    Na *Explorador de soluções*, selecione o projeto e descobrir a `Properties` janela (se não vir uma janela de propriedades, prima `F4`)
> 2.    Copie o valor de `URL` na área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Mude novamente para o *Portal de registo de aplicação* e cole o valor como um `Redirect URL` e clique em "Guardar"

<p/>

> #### <a name="setting-redirect-url-for-node"></a>URL de redirecionamento de definição de nó
> Para node. js, pode definir a web porta do servidor no *Server. js* ficheiro. Este tutorial utiliza a porta 30662 para referência, mas não hesite em utilizar qualquer porta disponível. Em qualquer caso, siga as instruções abaixo para configurar um URL de redirecionamento as informações de registo de aplicação:<br/>
> - Mude novamente para o *Portal de registo de aplicação* e defina `http://localhost:30662/` como uma `Redirect URL`, ou utilize `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (em que *[porta]* é a porta TCP personalizada número) e clique em "Guardar"


#### <a name="configure-your-javascript-spa"></a>Configurar seu SPA do JavaScript

1.  Crie um ficheiro denominado `msalconfig.js` que contém as informações de registo de aplicação. Se estiver a utilizar o Visual Studio, selecione o projeto (pasta raiz do projeto), o botão direito do mouse e selecione: `Add`  >  `New Item`  >  `JavaScript File`. Nomeie-o `msalconfig.js`
2.  Adicione o seguinte código para seu `msalconfig.js` ficheiro:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Substitua <code>Enter_the_Application_Id_here</code> com o Id da aplicação que acabou de registar
</li>
</ol>
