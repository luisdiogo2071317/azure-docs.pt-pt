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
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: f1dc23729f32a7a9535b887acf638cf5464c24bd
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "36206120"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Para registar a sua aplicação e adicione as informações de registo de aplicação à sua solução, tem duas opções:

### <a name="option-1-express-mode"></a>Opção 1: Modo de Express

Pode registar rapidamente a sua aplicação da seguinte forma:

1. Registar a sua aplicação através do [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Introduza um nome para a aplicação e o seu correio eletrónico
3.  Certifique-se que a opção para a configuração orientado está marcada
4.  Siga as instruções para adicionar um URL de redirecionamento da aplicação

### <a name="option-2-advanced-mode"></a>Opção 2: O modo avançado

Para registar a sua aplicação e adicione as informações de registo de aplicação à sua solução, efetue o seguinte:

1. Vá para o [Portal de registo de aplicações do Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registar uma aplicação
2. Introduza um nome para a aplicação e o seu correio eletrónico 
3. Certifique-se que a opção para a configuração orientado está desmarcada
4. Clique em `Add Platform`, em seguida, selecione `Web`
5. Vá para o Visual Studio e, no Explorador de soluções, selecione o projeto e observe a janela de propriedades (se não for apresentada uma janela de propriedades, prima F4)
6. Alteração SSL ativado para `True`
7. Com o botão direito no projeto no Visual Studio, clique em escolher **propriedades**e o **Web** separador. No *servidores* secção alteração o *Url do projeto* para ser o URL de SSL
8. Copie o URL de SSL e adicionar este URL à lista de URLs de redirecionamento na lista do Portal de registo de URLs de redirecionamento:<br/><br/>![Propriedades do projeto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Adicione o seguinte no `web.config` localizado na pasta raiz na secção `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Substitua `ClientId` com o ID de aplicação que acabou de ser registado
11. Substitua `redirectUri` com o URL de SSL do seu projeto

