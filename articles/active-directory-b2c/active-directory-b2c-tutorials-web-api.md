---
title: Tutorial - Conceder acesso a uma API Web ASP.NET a partir de uma aplicação Web com o Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Active Directory B2C para proteger uma API Web ASP.NET e chamá-lo a partir de uma aplicação Web ASP.NET.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.author: davidmu
ms.date: 01/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: f61a3b103d8738e1b86fb64aff99dab9c6986fdf
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-from-a-web-app-using-azure-active-directory-b2c"></a>Tutorial - Conceder acesso a uma API Web ASP.NET a partir de uma aplicação Web com o Azure Active Directory B2C

Este tutorial mostra-lhe como chamar um recurso de API Web protegida com o Azure Active Directory (Azure AD) B2C a partir de uma aplicação Web ASP.NET.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar uma API Web no seu inquilino do Azure AD B2C
> * Definir e configurar âmbitos para uma API Web
> * Conceder permissões de acesso à API Web à sua aplicação
> * Atualizar o código de exemplo para utilizar o Azure AD B2C para proteger uma API Web

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o [Tutorial sobre como utilizar o Azure Active Directory B2C para Autenticação de Utilizadores numa Aplicação Web ASP.NET](active-directory-b2c-tutorials-web-app.md).
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de **desenvolvimento na Web e em ASP.NET**.

## <a name="register-web-api"></a>Registar uma API Web

Os recursos da API Web têm de ser registados no seu inquilino antes de poderem aceitar e responder a [pedidos de recursos protegidos](../active-directory/develop/active-directory-dev-glossary.md#resource-server) por parte de [aplicações cliente](../active-directory/develop/active-directory-dev-glossary.md#client-application) que apresentem [tokens de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) a partir do Azure Active Directory. O registo estabelece o [objeto da aplicação e do principal de serviço](../active-directory/develop/active-directory-dev-glossary.md#application-object) no seu inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure.

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**.

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições.
    
    ![Adicionar uma API nova](media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | A Minha Aplicação Web de Exemplo | Introduza um **Nome** que descreva a API Web aos programadores. |
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma API Web. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `https://localhost:44332` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação peça. Neste tutorial, a API Web de exemplo é executada localmente (localhost) e escuta na porta 44332. |
    | **URI do ID da Aplicação** | myAPISample | O URI identifica exclusivamente a API no inquilino. Isto permite-lhe registar várias APIs por inquilino. Os [âmbitos](../active-directory/develop/active-directory-dev-glossary.md#scopes) regem o acesso ao recurso protegido da API e são definidos por URI de ID de Aplicação. |
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
| **Âmbito** | Hello.Read | Acesso de leitura a hello |
| **Âmbito** | Hello.Write | Acesso de escrita a hello |

Clique em **Guardar**.

Os âmbitos publicados podem ser utilizados para conceder permissão a uma aplicação cliente à API Web.

### <a name="grant-app-permissions-to-web-api"></a>Conceder permissões de acesso à API Web à sua aplicação

Para chamar uma API Web protegida a partir de uma aplicação, tem de conceder permissões à sua aplicação à API. Neste tutorial, utilize a aplicação Web criada no [Tutorial sobre como utilizar o Azure Active Directory B2C para Autenticação de Utilizadores numa Aplicação Web ASP.NET](active-directory-b2c-tutorials-web-app.md). 

1. No portal do Azure, selecione **Azure AD B2C** na lista de serviços e clique em **Aplicações** para ver a lista de aplicações registadas.

2. Selecione **My Sample Web API** na lista de aplicações, clique em **Acesso à API (pré-visualização)** e clique em **Adicionar**.

3. Na lista pendente **Selecionar API**, selecione a API Web registada, **My Sample Web API**.

4. Na lista pendente **Selecionar Âmbitos**, selecione os âmbitos que definiu no registo da API Web.

    ![selecionar âmbitos para a aplicação](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. Clique em **OK**.

A sua **My Sample Web API** está registada para chamar **My Sample Web API** protegida. Para utilizar a aplicação Web, os utilizadores [autenticam-se](../active-directory/develop/active-directory-dev-glossary.md#authentication) com o Azure AD B2C. A aplicação Web obtém uma [concessão de autorização](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) do Azure AD B2C para aceder à API Web protegida.

## <a name="update-code"></a>Atualizar código

Agora que a API Web está registada e já estão definidos âmbitos, tem de configurar o código da API Web para utilizar o seu inquilino do Azure AD B2C. Neste tutorial, vai configurar uma API Web de exemplo. 

A API Web de exemplo está incluída no projeto que transferiu no tutorial de pré-requisito [Use Azure Active Directory B2C for User Authentication in an ASP.NET Web App tutorial](active-directory-b2c-tutorials-web-app.md) (Tutorial sobre como utilizar o Azure Active Directory B2C para Autenticação de Utilizadores numa Aplicação Web ASP.NET). Se ainda concluiu o tutorial de pré-requisito, faça-o antes de continuar.

Existem dois projetos na solução de exemplo:

**Aplicação de exemplo da aplicação Web (TaskWebApp):** aplicação Web para criar e editar uma lista de tarefas. A aplicação Web utiliza a política de **inscrição ou início de sessão** para inscrição ou início de sessão de utilizadores com um endereço de e-mail.

**Aplicação de exemplo da API Web (TaskService):** API Web que suporta a funcionalidade de criação, leitura, atualização e eliminação da lista de tarefas. A API Web é protegida pelo Azure AD B2C e chamada pela aplicação Web.

A aplicação Web e a API Web de exemplo definem os valores de configuração como definições da aplicação no ficheiro Web.config de cada projeto.

Abra a solução **B2C-WebAPI-DotNet** no Visual Studio.

### <a name="configure-the-web-app"></a>Configurar a aplicação Web

1. Abra **Web.config** no projeto **TaskWebApp**.

2. Para executar a API localmente, utilize a definição de localhost **api:TaskServiceUrl**. Altere o Web.config da seguinte forma: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. Configure o URI da API. Este é o URI que a aplicação Web utiliza para fazer o pedido de API. Além disso, configure as permissões pedidas.

    ```C#
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Configurar a API Web

1. Abra **Web.config** no projeto **TaskService**.

2. Configure a API para utilizar o seu inquilino.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. Defina o ID de cliente como o ID da Aplicação registada para a API.

    ```C#
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. Atualize a definição de política com o nome gerado quando criou a política de inscrição e início de sessão.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="B2C_1_SiUpIn" />
    ```

5. Configure a definição de âmbitos para que corresponda ao que criou no portal.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Executar o exemplo

Tem de executar os dois projetos, **TaskWebApp** e **TaskService**. 

1. No Explorador de Soluções, clique com o botão direito do rato na solução e selecione **Definir Projetos de Arranque...**. 
2. Selecione o botão de opção **Vários projetos de arranque**.
3. Altere a **Ação** em ambos os projetos para **Iniciar**.
4. Clique em OK para guardar a configuração.
5. Prima **F5** para executar as duas aplicações. Cada aplicação é aberta no respetivo separador do browser. `https://localhost:44316/` é a aplicação Web.
    `https://localhost:44332/` é a API Web.

6. Na aplicação Web, clique na liação de inscrição / início de sessão, na faixa de menu, para se inscrever na aplicação Web. Utilize a conta que criou no [tutorial da aplicação Web](active-directory-b2c-tutorials-web-app.md). 
7. Depois de iniciar sessão, clique na ligação **Lista de tarefas** e crie um item de lista de tarefas.

Quando cria um item de lista de tarefas, a aplicação Web envia um pedido para a API Web para gerar esse item. A sua aplicação Web protegida está a chamar a API Web protegida no seu inquilino do Azure AD B2C.

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Este artigo mostrou-lhe como proteger uma API Web ASP.NET ao registar e definir âmbitos no Azure AD B2C. Para saber mais detalhes sobre como desenvolver este cenário, incluindo instruções relativas a código, avance para o tutorial seguinte.

> [!div class="nextstepaction"]
> [Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset](active-directory-b2c-devquickstarts-web-dotnet-susi.md) (Criar uma aplicação Web ASP.NET com processos de inscrição, início de sessão, edição de perfil e reposição de palavras-passe do Azure Active Directory B2C)