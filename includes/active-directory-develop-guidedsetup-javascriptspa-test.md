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
ms.openlocfilehash: 9817e94ba77c83b8620274ba41f9d862b6a5ce6d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293561"
---
## <a name="test-your-code"></a>Testar o seu código

### <a name="test-with-visual-studio"></a>Testar com o Visual Studio
Se estiver a utilizar o Visual Studio, prima **F5** para executar o seu projeto. O browser é aberto o http://<span></span>localhost: localização {porta} e ver o **chamar o Microsoft Graph API** botão.

<p/><!-- -->

### <a name="test-with-node-or-other-web-server"></a>Teste com o nó ou outro servidor web
Se não estiver a utilizar o Visual Studio, certifique-se de que o seu servidor web é iniciado. Configurar o servidor para escutar numa porta TCP que baseia-se na localização dos seus **Index** ficheiro. Para o nó, inicie o servidor web para escutar a porta ao executar os seguintes comandos numa linha de comandos a partir da pasta de aplicação:

```bash
npm install
node server.js
```
Abra o browser e escreva http://<span></span>localhost:30662 ou http://<span></span>localhost: {porta} em que **porta** é a porta que está a escutar ao seu servidor web. Deverá ver o conteúdo do ficheiro Index. HTML e o **chamar o Microsoft Graph API** botão.

## <a name="test-your-application"></a>Testar a sua aplicação

Depois do navegador carrega o ficheiro Index. HTML, selecione **chamar o Microsoft Graph API**. A primeira vez que executar a aplicação, o navegador redireciona-o para o ponto de final de v2.0 do Microsoft Azure Active Directory (Azure AD) e lhe for pedido para iniciar sessão:

![Inicie sessão na sua conta do SPA do JavaScript](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Forneça o consentimento para acesso à aplicação

A primeira vez que iniciar sessão na sua aplicação, lhe for pedido para fornecer o seu consentimento para permitir que a aplicação para aceder ao seu perfil e para o início de sessão:

![Forneça o seu consentimento para acesso à aplicação](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Ver resultados da aplicação
Depois de iniciar sessão, deverá ver as informações do perfil de usuário na **resposta de chamada de API do Graph** caixa.
 
![Resultados esperados da chamada de API do Microsoft Graph](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

Também deverá ver informações básicas sobre o token que foi adquirida no **Token de acesso** e **afirmações de Token de ID** caixas.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Obter mais informações sobre âmbitos e permissões delegadas

O Microsoft Graph API requer a **user.read** âmbito para ler um perfil de utilizador. Este âmbito é adicionado automaticamente por predefinição em todos os aplicativos que está registado no portal de registo. Outras APIs para o Microsoft Graph, bem como APIs personalizadas para o seu servidor de back-end, pode necessitar de âmbitos adicionais. O Microsoft Graph API requer a **Calendars.Read** âmbito para listar calendários do utilizador.

Para aceder aos calendários do utilizador no contexto de uma aplicação, adicione a **Calendars.Read** delegado permissão para as informações de registo de aplicação. Em seguida, adicione a **Calendars.Read** definir o âmbito para o **acquireTokenSilent** chamar. 

>[!NOTE]
>O utilizador pode ser pedido para consentimentos adicionais à medida que aumenta o número de âmbitos.

Se uma API de back-end não requer um âmbito (não recomendado), pode utilizar o **clientId** como o âmbito no **acquireTokenSilent** e **acquireTokenRedirect** chamadas.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
