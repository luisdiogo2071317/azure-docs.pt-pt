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
ms.openlocfilehash: 724166d402f81fa3a2c977d107111f5a0c32571d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293722"
---
## <a name="setting-up-your-web-server-or-project"></a>Como configurar seu servidor web ou projeto

> Prefere baixar de projeto este exemplo em vez disso?
> - [Transfira o projeto do Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> ou
> - [Transferir os ficheiros de projeto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip) para um servidor local web, como o nó
>
> E, em seguida, avance para o [passo de configuração](#register-your-application) para configurar o exemplo de código antes de executá-la.

## <a name="prerequisites"></a>Pré-requisitos
Um servidor local web, como [node. js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core), ou a integração de IIS Express com [Visual Studio 2017](https://www.visualstudio.com/downloads/) é necessário para executar este tutorial.

As instruções neste guia se baseiam em node. js e o Visual Studio 2017, mas pode utilizar qualquer outro ambiente de desenvolvimento ou servidor Web.

## <a name="create-your-project"></a>Criar o seu projeto

> ### <a name="option-1-visual-studio"></a>Opção 1: Visual Studio
> Se estiver a utilizar o Visual Studio e criar um novo projeto, siga os passos abaixo para criar uma nova solução do Visual Studio:
> 1.    No Visual Studio:  `File` > `New` > `Project`
> 2.    Em `Visual C#\Web`, selecione `ASP.NET Web Application (.NET Framework)`
> 3.    Nome da sua aplicação e clique em *OK*
> 4.    Em `New ASP.NET Web Application`, selecione `Empty`

<p/><!-- -->

> ### <a name="option-2-node-other-web-servers"></a>Opção 2: Nó / outros servidores web
> Certifique-se de que instalou [node. js](https://nodejs.org/en/download/), em seguida, siga o passo abaixo:
> - Crie uma pasta para alojar a sua aplicação.


## <a name="create-your-single-page-applications-ui"></a>Criar a IU da sua aplicação de página única
1.  Criar uma *Index* ficheiro para o SPA do JavaScript. Se estiver a utilizar o Visual Studio, selecione o projeto (pasta raiz do projeto), clique com botão direito e selecione: `Add`  >  `New Item`  >  `HTML page` e dê-lhe o nome Index. HTML
2.  Adicione o seguinte código à sua página:
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.3/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````
