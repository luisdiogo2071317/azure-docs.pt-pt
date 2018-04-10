---
title: Tutorial Utilizar o Azure Active Directory B2C para Autenticação de Utilizadores numa Aplicação Web ASP.NET
description: Tutorial sobre como utilizar o Azure Active Directory B2C para fornecer início de sessão do utilizador para uma aplicação Web ASP.NET.
services: active-directory-b2c
author: davidmu1
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 19629f383bdab19a2541ca33dd2937574c2ced17
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="tutorial-authenticate-users-with-azure-active-directory-b2c-in-an-aspnet-web-app"></a>Tutorial: Autenticar utilizadores com o Azure Active Directory B2C numa aplicação Web ASP.NET

Este tutorial mostra como utilizar o Azure Active Directory (Azure AD) B2C para início de sessão e inscrição de utilizadores numa aplicação Web ASP.NET. O Azure AD B2C permite às aplicações efetuar a autenticação em contas de redes sociais, contas empresariais e contas do Azure Active Directory através de protocolos padrão abertos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar uma aplicação Web ASP.NET de exemplo no inquilino do Azure AD B2C.
> * Criar políticas para inscrição e início de sessão de utilizadores, editar um perfil e repor a palavra-passe.
> * Configurar a aplicação Web de exemplo para utilizar o inquilino do Azure AD B2C. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Criar o seu próprio [Inquilino do Azure AD B2C](active-directory-b2c-get-started.md)
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de **desenvolvimento na Web e em ASP.NET**.

## <a name="register-web-app"></a>Registar uma aplicação Web

As aplicações têm de ser [registadas](../active-directory/develop/active-directory-dev-glossary.md#application-registration) no inquilino antes de poderem receber [tokens de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) do Azure Active Directory. O registo de aplicações cria um [ID de aplicação](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) para a aplicação no inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure.

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**.

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições:

    ![Adicionar uma nova aplicação](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | A Minha Aplicação Web de Exemplo | Introduza um **Nome** que descreva a aplicação aos consumidores. | 
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma aplicação Web. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `https://localhost:44316` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação solicite. Neste tutorial, o exemplo é executado localmente (localhost) e escuta na porta 44316. |
    | **Cliente nativo** | Não | Uma vez que se trata de uma aplicação Web e não um cliente nativo, selecione Não. |

3. Clique em **Criar** para registar a aplicação.

As aplicações registadas são apresentadas na lista de aplicações para o inquilino do Azure AD B2C. Selecione a aplicação Web na lista. É apresentado o painel de propriedades da aplicação Web.

![Propriedades da aplicação Web](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a aplicação e é necessário quando configurar a aplicação mais tarde no tutorial.

### <a name="create-a-client-password"></a>Criar uma palavra-passe de cliente

O Azure AD B2C utiliza a autorização de OAuth2 para [aplicações cliente](../active-directory/develop/active-directory-dev-glossary.md#client-application). As aplicações Web são [clientes confidenciais](../active-directory/develop/active-directory-dev-glossary.md#web-client) e requerem um ID de cliente ou um ID de aplicação e um segredo de cliente, palavra-passe de cliente ou chave de aplicação.

1. Selecione a página Chaves para a aplicação Web registada e clique em **Gerar chave**.

2. Clique em **Guardar** para apresentar a chave.

    ![página de chaves geral da aplicação](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

A chave é apresentada uma vez no portal. É importante copiar e guardar o valor da chave. Este valor é necessário para configurar a aplicação. Mantenha a chave segura. Não partilhe a chave publicamente.

## <a name="create-policies"></a>Criar políticas

Uma política do Azure AD B2C define os fluxos de trabalho do utilizador. Por exemplo, iniciar sessão, inscrever, alterar palavras-passe e editar perfis são fluxos de trabalho comuns.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Criar uma política de inscrição ou início de sessão

Para inscrever utilizadores para acederem e iniciarem sessão na aplicação Web, crie uma **política de inscrição ou início de sessão**.

1. Na página do portal do Azure AD B2C, selecione **Políticas de inscrição ou início de sessão** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições:

    ![Adicionar uma política de inscrição ou início de sessão](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiUpIn | Introduza um **Nome** para a política. O nome da política tem o prefixo **b2c_1_**. Utilize o nome completo da política **b2c_1_SiUpIn** no código de exemplo. | 
    | **Fornecedor de identidade** | Inscrever-se no e-mail | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Atributos de inscrição** | Nome a Apresentar e Código Postal | Selecione os atributos a recolher do utilizador durante a inscrição. |
    | **Afirmações da aplicação** | Nome a Apresentar, Código Postal, O utilizador é novo, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/active-directory-dev-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token). |

2. Clique em **Criar** para criar a política. 

### <a name="create-a-profile-editing-policy"></a>Criar uma política de edição de perfil

Para permitir aos utilizadores repor as respetivas informações de perfil por si próprios, crie uma **política de edição de perfil**.

1. Na página do portal do Azure AD B2C, selecione **Políticas de edição de perfil** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições:

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiPe | Introduza um **Nome** para a política. O nome da política tem o prefixo **b2c_1_**. Utilize o nome completo da política **b2c_1_SiPe** no código de exemplo. | 
    | **Fornecedor de identidade** | Inscrição da conta local | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Atributos do perfil** | Nome a Apresentar e Código Postal | Selecione os atributos que os utilizadores podem modificar durante a edição de perfil. |
    | **Afirmações da aplicação** | Nome a Apresentar, Código Postal, O utilizador é novo, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/active-directory-dev-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) após uma edição de perfil com êxito. |

2. Clique em **Criar** para criar a política. 

### <a name="create-a-password-reset-policy"></a>Criar uma política de reposição de palavra-passe

Para ativar a reposição de palavras-passe na aplicação, tem de criar uma **política de reposição de palavra-passe**. Esta política descreve as experiências que os consumidores têm durante a reposição de palavras-passe e os conteúdos de tokens que a aplicação recebe após a conclusão com êxito.

1. Na página do portal do Azure AD B2C, selecione **Políticas de reposição de palavra-passe** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SSPR | Introduza um **Nome** para a política. O nome da política tem o prefixo **b2c_1_**. Utilize o nome completo da política **b2c_1_SSPR** no código de exemplo. | 
    | **Fornecedor de identidade** | Repor a palavra-passe com o endereço de e-mail | Trata-se do fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Afirmações da aplicação** | ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/active-directory-dev-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) após uma reposição de palavra-passe com êxito. |

2. Clique em **Criar** para criar a política. 

## <a name="update-web-app-code"></a>Atualizar o código da aplicação Web

Agora que tem uma aplicação Web registada e as políticas criadas, tem de configurar a aplicação para utilizar o inquilino do Azure AD B2C. Neste tutorial, irá configurar uma aplicação Web de exemplo. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) ou clone a aplicação Web de exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

A aplicação Web ASP.NET de exemplo é uma aplicação de lista de tarefas simples para criar e atualizar uma lista de tarefas. A aplicação utiliza [componentes middleware do Microsoft OWIN](https://docs.microsoft.com/en-us/aspnet/aspnet/overview/owin-and-katana/) para permitir aos utilizadores inscreverem-se para utilizar a aplicação no inquilino do Azure AD B2C. Ao criar uma política do Azure AD B2C, os utilizadores podem utilizar uma conta de rede social ou criar uma conta para utilizar como identidade para acederem à aplicação. 

Existem dois projetos na solução de exemplo:

**Aplicação de exemplo da aplicação Web (TaskWebApp):** aplicação Web para criar e editar uma lista de tarefas. A aplicação Web utiliza a política de **inscrição ou início de sessão** para inscrição ou início de sessão de utilizadores.

**Exemplo de aplicação da API Web (TaskService):** API Web que suporta a funcionalidade de criação, leitura, atualização e eliminação da lista de tarefas. A API Web é protegida pelo Azure AD B2C e chamada pela aplicação Web.

Tem de alterar a aplicação de modo a utilizar o registo de aplicações no seu inquilino, que inclui o ID de cliente ou o ID de aplicação e a palavra-passe de cliente ou a chave de aplicação. Também tem de configurar as políticas que criou. A aplicação Web de exemplo define os valores de configuração como definições da aplicação no ficheiro Web.config. Para alterar as definições da aplicação:

1. Abra a solução **B2C-WebAPI-DotNet** no Visual Studio.

2. No projeto da aplicação Web **TaskWebApp**, abra o ficheiro **Web.config** e efetue as seguintes atualizações:

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    
    <add key="ida:ClientId" value="The Application ID for your web app registered in your tenant" />
    
    <add key="ida:ClientSecret" value="Client password (client secret or app key)" />
    ```
3. Atualize as definições de política com o nome gerado quando criou as políticas.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    <add key="ida:EditProfilePolicyId" value="b2c_1_SiPe" />
    <add key="ida:ResetPasswordPolicyId" value="b2c_1_SSPR" />
    ```

## <a name="run-the-sample-web-app"></a>Executar a aplicação Web de exemplo

No Explorador de Soluções, clique com o botão direito do rato no projeto **TaskWebApp** e clique em **Configurar como Projeto de Arranque**

Prima **F5** para iniciar a aplicação Web. É iniciado o browser predefinido para o endereço do site local `https://localhost:44316/`. 

A aplicação de exemplo suporta inscrição, início de sessão, edição de perfil e reposição de palavra-passe. Este tutorial destaca como um utilizador se inscreve para utilizar a aplicação com um endereço de e-mail. Pode explorar outros cenários.

### <a name="sign-up-using-an-email-address"></a>Inscrever-se com um endereço de e-mail

1. Clique na ligação **Inscrever-se/Iniciar sessão** na faixa superior para se inscrever como um utilizador da aplicação Web. Esta opção utiliza a política **b2c_1_SiUpIn** definida num passo anterior.

2. O Azure AD B2C apresenta uma página de início de sessão com uma ligação de inscrição. Uma vez que ainda não tem uma conta, clique na ligação **Inscrever-se agora**. 

3. O fluxo de trabalho de inscrição apresenta uma página para recolher e verificar a identidade do utilizador através de um endereço de e-mail. O fluxo de trabalho de inscrição também recolhe a palavra-passe do utilizador e os atributos solicitados definidos na política.

    Utilize um endereço de e-mail válido e valide com o código de verificação. Defina uma palavra-passe. Introduza os valores para os atributos solicitados. 

    ![Fluxo de trabalho de inscrição](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Clique em **Criar** para criar uma conta local no inquilino do Azure AD B2C.

Agora, o utilizador pode utilizar o respetivo endereço de e-mail para iniciar sessão e utilizar a aplicação Web.

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar um inquilino do Azure AD B2C, criar políticas e atualizar a aplicação Web de exemplo para utilizar o inquilino do Azure AD B2C. Avance para o próximo tutorial para aprender a registar, configurar e chamar uma API Web ASP.NET protegida pelo inquilino do Azure AD B2C.

> [!div class="nextstepaction"]
> [Tutorial: utilizar o Azure Active Directory B2C para proteger uma API Web ASP.NET](active-directory-b2c-tutorials-web-api.md)
