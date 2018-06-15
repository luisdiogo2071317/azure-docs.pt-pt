---
title: Tutorial - Conceder acesso a uma API Web Node.js a partir de uma aplicação de ambiente de trabalho com o Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Active Directory B2C para proteger uma API Web Node.js e chamá-la a partir de uma aplicação de ambiente de trabalho .NET.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/01/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 01c13b214d40fba278ce788047e2b158adc20287
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711601"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Tutorial: Conceder acesso a uma API Web Node.js a partir de uma aplicação de ambiente de trabalho com o Azure Active Directory B2C

Este tutorial mostra-lhe como chamar um recurso de API Web Node.js protegida do Azure Active Directory (Azure AD) B2C a partir de uma aplicação de ambiente de trabalho do Windows Presentation Foundation (WPF).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar uma API Web no seu inquilino do Azure AD B2C
> * Definir e configurar âmbitos para uma API Web
> * Conceder permissões de acesso à API Web à sua aplicação
> * Atualizar o código de exemplo para utilizar o Azure AD B2C para proteger uma API Web

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o tutorial [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de ambiente de trabalho](active-directory-b2c-tutorials-desktop-app.md).
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com as cargas de trabalho de **desenvolvimento no ambiente de trabalho em .NET** e **desenvolvimento na Web e em ASP.NET**.
* Instalar o [Node. js](https://nodejs.org/en/download/)

## <a name="register-web-api"></a>Registar uma API Web

Os recursos da API Web têm de ser registados no seu inquilino antes de poderem aceitar e responder a [pedidos de recursos protegidos](../active-directory/develop/active-directory-dev-glossary.md#resource-server) por parte de [aplicações cliente](../active-directory/develop/active-directory-dev-glossary.md#client-application) que apresentem [tokens de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) a partir do Azure Active Directory. O registo estabelece o [objeto da aplicação e do principal de serviço](../active-directory/develop/active-directory-dev-glossary.md#application-object) no seu inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure.

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**.

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições.
    
    ![Adicionar uma API nova](media/active-directory-b2c-tutorials-desktop-app-webapi/web-api-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | O Meu Exemplo de API Web Node.js | Introduza um **Nome** que descreva a API Web aos programadores. |
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma API Web. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `http://localhost:5000` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação peça. Neste tutorial, a API Web de exemplo é executada localmente (localhost) e escuta na porta 5000. |
    | **URI do ID da Aplicação** | demoapi | O URI identifica exclusivamente a API no inquilino. Isto permite-lhe registar várias APIs por inquilino. Os [âmbitos](../active-directory/develop/active-directory-dev-glossary.md#scopes) regem o acesso ao recurso protegido da API e são definidos por URI de ID de Aplicação. |
    | **Cliente nativo** | Não | Uma vez que se trata de uma API Web e não de um cliente nativo, selecione Não. |
    
3. Clique em **Criar** para registar a API.

As APIs registadas são apresentadas na lista de aplicações do inquilino do Azure AD B2C. Selecione a API Web na lista. É apresentado o painel de propriedades da API Web.

![Propriedades da API Web](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a API e é necessário quando a configurar mais tarde no tutorial.

Registar a API Web no Azure AD B2C define uma relação de confiança. Uma vez que a API está registada no B2C, a API pode agora confiar nos tokens de acesso do B2C que recebe de outras aplicações.

## <a name="define-and-configure-scopes"></a>Definir e configurar âmbitos

Os [âmbitos](../active-directory/develop/active-directory-dev-glossary.md#scopes) proporcionam uma forma de reger o acesso a recursos protegidos. São utilizados pela API Web para implementar o controlo de acesso baseado no âmbito. Por exemplo, alguns utilizadores podem ter o acesso de leitura e de escrita, ao passo que outros podem ter apenas permissões só de leitura. Neste tutorial, vai definir as permissões de leitura e escrita para a API Web.

### <a name="define-scopes-for-the-web-api"></a>Definir âmbitos para a API Web

As APIs registadas são apresentadas na lista de aplicações do inquilino do Azure AD B2C. Selecione a API Web na lista. É apresentado o painel de propriedades da API Web.

Clique em **Âmbitos publicados (Pré-visualização)**.

Para configurar âmbitos para a API, adicione as seguintes entradas. 

![âmbitos definidos na api Web](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Definição      | Valor sugerido  | Descrição                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Âmbito** | demo.read | Acesso de leitura à API de demonstração|

Clique em **Guardar**.

Os âmbitos publicados podem ser utilizados para conceder permissão a uma aplicação cliente à API Web.

### <a name="grant-app-permissions-to-web-api"></a>Conceder permissões de acesso à API Web à sua aplicação

Para chamar uma API Web protegida a partir de uma aplicação, tem de conceder permissões à sua aplicação à API. Neste tutorial, utilize a aplicação de ambiente de trabalho criada no tutorial [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de ambiente de trabalho](active-directory-b2c-tutorials-desktop-app.md).

1. No portal do Azure, selecione **Azure AD B2C** na lista de serviços e clique em **Aplicações** para ver a lista de aplicações registadas.

2. Selecione **O Meu Exemplo de Aplicação WPF** na lista de aplicações, clique em **Acesso à API (Pré-visualização)** e clique em **Adicionar**.

3. Na lista pendente **Selecionar API**, selecione a API Web registada, **O Meu Exemplo de API Web Node.js**.

4. Na lista pendente **Selecionar Âmbitos**, selecione os âmbitos que definiu no registo da API Web.

    ![selecionar âmbitos para a aplicação](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Clique em **OK**.

O seu **O Meu Exemplo de Aplicação WPF** está registada para chamar o **O Meu Exemplo de API Web Node.js** protegido. Para utilizar a aplicação de ambiente de trabalho WPF, os utilizadores [autenticam-se](../active-directory/develop/active-directory-dev-glossary.md#authentication) com o Azure AD B2C. A aplicação de ambiente de trabalho obtém uma [concessão de autorização](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) do Azure AD B2C para aceder à API Web protegida.

## <a name="update-web-api-code"></a>Atualizar o código da API Web

Agora que a API Web está registada e já estão definidos âmbitos, tem de configurar o código da API Web para utilizar o seu inquilino do Azure AD B2C. Neste tutorial, irá configurar um exemplo de aplicação Web Node.js que pode transferir a partir do GitHub. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) ou clone a aplicação Web de exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
O exemplo de API Web Node.js utiliza a biblioteca Passport.js para ativar o Azure AD B2C no sentido de proteger as chamadas à API. 

### <a name="configure-the-web-api"></a>Configurar a API Web

1. Abra o ficheiro `index.html` no exemplo de API Web Node.js.
2. Configure o exemplo com as informações de registo de inquilino do Azure AD B2C. Altere as seguintes linhas de código:

```nodejs
var tenantID = "<your-tenant-name>.onmicrosoft.com";
var clientID = "<Application ID for your Node.js Web API>";
var policyName = "B2C_1_SiUpIn";  // Sign-in / sign-up policy name
```

### <a name="configure-the-desktop-app"></a>Configurar a aplicação de ambiente de trabalho

1. Abra a solução `active-directory-b2c-wpf` a partir do tutorial [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de ambiente de trabalho](active-directory-b2c-tutorials-desktop-app.md) no Visual Studio.

## <a name="run-the-sample"></a>Executar o exemplo

Execute a API Web Node.Js:

1. Inicie uma linha de comandos Node.js.
2. Mude para o diretório que contém o exemplo de Node.js. Por exemplo, `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Execute os seguintes comandos:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```
Execute a aplicação de ambiente de trabalho:

1. Prima **F5** para executar a aplicação de ambiente de trabalho.
2. Inicie sessão com o endereço de e-mail e a palavra-passe utilizados no tutorial [Autenticar utilizadores com o Azure Active Directory B2C numa aplicação de ambiente de trabalho](active-directory-b2c-tutorials-desktop-app.md).
3. Clique no botão **Chamar API**. 

A aplicação de ambiente de trabalho faz um pedido à API Web e obtém uma resposta com o nome a apresentar do utilizador com sessão iniciada. A sua aplicação de ambiente de trabalho protegida está a chamar a API Web protegida no seu inquilino do Azure AD B2C.

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Este artigo mostrou-lhe como proteger uma API Web ASP.NET ao registar e definir âmbitos no Azure AD B2C. Saiba mais com os exemplos de código do Azure AD B2C disponíveis.

> [!div class="nextstepaction"]
> [Exemplos de código do Azure AD B2C](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
