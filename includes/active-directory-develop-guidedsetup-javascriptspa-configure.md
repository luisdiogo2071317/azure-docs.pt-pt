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
ms.openlocfilehash: b3d46e10facdef26b36c910a5c23b40a415a2894
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988429"
---
## <a name="register-your-application"></a>Registar a sua aplicação

Existem várias formas de registar uma aplicação. Selecione a opção que melhor se adequa às suas necessidades:
* [Modo expresso - utilizar o guia de introdução do SPA para configurar a aplicação](#option-1-register-your-application-express-mode)
* [Modo - avançado, configurar manualmente as definições da aplicação](#option-2-register-your-application-advanced-mode)

### <a name="option-1-register-your-application-express-mode"></a>Opção 1: Registar a sua aplicação (modo de Express)

1. Inicie sessão para o [registo de aplicação do portal do Azure (pré-visualização)](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) registar uma aplicação.
1. Sobre o **registar uma aplicação** página, introduza um nome para a sua aplicação.
1. Sob **tipos de conta suportados**, selecione **contas em qualquer diretório organizacional e contas Microsoft pessoais**.
1. Quando terminar, selecione **registar**.
1. Siga as instruções de início rápido para transferir e configurar automaticamente o seu novo aplicativo para num só clique.

### <a name="option-2-register-your-application-advanced-mode"></a>Opção 2: Registar a sua aplicação (modo avançado)

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) registar uma aplicação.
1. Se a sua conta dá-lhe acesso a mais do que um inquilino, selecione a sua conta no canto superior direito canto e definir a sua sessão do portal para o Azure AD pretendido de inquilino.
1. No painel de navegação do lado esquerdo, selecione o **do Azure Active Directory** serviço e, em seguida, selecione **registos das aplicações (pré-visualização) > novo registo**.
1. Quando o **registar uma aplicação** é apresentada a página, introduza um nome para a sua aplicação.
1. Sob **tipos de conta suportados**, selecione **contas em qualquer diretório organizacional e contas Microsoft pessoais**.
1. Sob o **URI de redirecionamento** secção, selecione a **Web** plataforma e defina o valor para o URL da aplicação com base no seu servidor web. Consulte as secções abaixo para obter instruções sobre como definir e obter o URL de redirecionamento no nó e o Visual Studio.
1. Quando terminar, selecione **registar**.
1. Na aplicação **descrição geral** página, tome nota da **ID de aplicação (cliente)** valor.
1. Este início rápido requer a [fluxo de concessão implícita](../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md) seja ativado. No painel de navegação do lado esquerdo do aplicativo registrado, selecione **autenticação**.
1. Na **definições avançadas**, em **concessão implícita**, permita as duas opções **tokens de ID** e **tokens de acesso** caixas de verificação. Tokens de ID e tokens de acesso são necessários, uma vez que esta aplicação tem de iniciar sessão dos utilizadores e chamar uma API.
1. Selecione **Guardar**.

> #### <a name="setting-the-redirect-url-for-node"></a>Definir o URL de redirecionamento para o nó
> Para node. js, pode definir a web porta do servidor no *Server. js* ficheiro. Este tutorial utiliza a porta 30662 para referência, mas pode usar qualquer outra porta disponível. Siga as instruções abaixo para configurar um URL de redirecionamento as informações de registo de aplicação:<br/>
> - Mude novamente para o *registo de aplicação* e defina `http://localhost:30662/` como uma `Redirect URL`, ou utilize `http://localhost:[port]/` se estiver a utilizar uma porta TCP personalizada (em que *[porta]* é o número de porta TCP personalizado).

<p/>

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio instruções para obter o URL de redirecionamento
> Siga estes passos para obter o URL de redirecionamento:
> 1. Na **Explorador de soluções**, selecione o projeto e descobrir a **propriedades** janela. Se não vir um **propriedades** janela, prima **F4**.
> 2. Copie o valor de **URL** na área de transferência:<br/> ![Propriedades do projeto](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Mude novamente para o *registo de aplicação* e defina o valor como um **URL de redirecionamento**.

#### <a name="configure-your-javascript-spa"></a>Configurar seu SPA do JavaScript

1. Na `index.html` ficheiro criado durante a configuração do projeto, adicione as informações de registo de aplicação. Adicione o seguinte código na parte superior dentro do `<script></script>` etiquetas no corpo do seu `index.html` ficheiro:

    ```javascript
    var applicationConfig = {
        clientID: "[Enter the application Id here]",
        graphScopes: ["user.read"],
        graphEndpoint: "https://graph.microsoft.com/v1.0/me"
    };
    ```

<ol start="2">
<li>
Substitua <code>Enter the application Id here</code> com o ID da aplicação que acabou de registar.
</li>
</ol>
