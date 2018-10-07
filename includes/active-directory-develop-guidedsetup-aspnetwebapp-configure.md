---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: faa3ad2376935aee4508b814f1b67fdacb98cf6e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843451"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Para registar a sua aplicação e adicionar as suas informações de registo de aplicação à sua solução, tem duas opções:

### <a name="option-1-express-mode"></a>Opção 1: Modo de Express

Pode registar a sua aplicação rapidamente ao fazer o seguinte:

1. Registar a sua aplicação através do [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Introduza um nome para a sua aplicação e o seu e-mail
3.  Certifique-se a opção para a configuração interativa
4.  Siga as instruções para adicionar um URL de redirecionamento para a sua aplicação

### <a name="option-2-advanced-mode"></a>Opção 2: Modo avançado

Para registar a sua aplicação e adicionar as informações de registo da aplicação à sua solução, faça o seguinte:

1. Vá para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/portal/register-app) registar uma aplicação
2. Introduza um nome para a sua aplicação e o seu e-mail 
3. Certifique-se que a opção para a configuração interativa está desmarcada
4. Clique em `Add Platform`, em seguida, selecione `Web`
5. Vá para o Visual Studio e, no Explorador de soluções, selecione o projeto e observar a janela de propriedades (se não vir uma janela de propriedades, pressione F4)
6. Alterar SSL ativado para `True`
7. Clique com o botão direito do rato no projeto no Visual Studio, em seguida, escolha **propriedades**e o **Web** separador. Na *servidores* secção alteração a *Url de projeto* para ser o URL do SSL
8. Copie o URL de SSL e adicionar este URL à lista de URLs de redirecionamento na lista do Portal de registo de URLs de redirecionamento:<br/><br/>![Propriedades do projeto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. Adicione o seguinte na `web.config` localizado na pasta raiz na secção `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. Substitua `ClientId` com o ID da aplicação que acabou de registar
11. Substitua `redirectUri` com o URL de SSL do seu projeto

