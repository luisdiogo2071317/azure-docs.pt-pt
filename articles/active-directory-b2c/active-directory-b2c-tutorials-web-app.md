---
title: Tutorial - Ativar uma aplicação Web para autenticar com as contas utilizando o Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Azure Active Directory B2C para fornecer início de sessão do utilizador para uma aplicação Web ASP.NET.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 30a94cb5de2d618938f17c4e5733821ac7247785
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851526"
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Ativar uma aplicação web autenticar com contas através do Azure Active Directory B2C

Este tutorial mostra como utilizar o Azure Active Directory (Azure AD) B2C para início de sessão e inscrição de utilizadores numa aplicação Web ASP.NET. O Azure AD B2C permite às aplicações efetuar a autenticação em contas de redes sociais, contas empresariais e contas do Azure Active Directory através de protocolos padrão abertos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar uma aplicação Web ASP.NET de exemplo no inquilino do Azure AD B2C.
> * Criar fluxos de utilizador para o utilizador de inscrição, início de sessão, edição de perfil e reposição de palavra-passe.
> * Configurar a aplicação Web de exemplo para utilizar o inquilino do Azure AD B2C. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Criar o seu próprio [Inquilino do Azure AD B2C](active-directory-b2c-get-started.md)
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de **desenvolvimento na Web e em ASP.NET**.

## <a name="register-web-app"></a>Registar uma aplicação Web

As aplicações têm de ser [registadas](../active-directory/develop/developer-glossary.md#application-registration) no inquilino antes de poderem receber [tokens de acesso](../active-directory/develop/developer-glossary.md#access-token) do Azure Active Directory. O registo de aplicações cria um [ID de aplicação](../active-directory/develop/developer-glossary.md#application-id-client-id) para a aplicação no inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**. Já deve estar a utilizar o inquilino que criou no tutorial anterior. 

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**. 

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições:

    ![Adicionar uma nova aplicação](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | A Minha Aplicação Web de Exemplo | Introduza um **Nome** que descreva a aplicação aos consumidores. | 
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma aplicação Web. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `https://localhost:44316` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação solicite. Neste tutorial, o exemplo é executado localmente (localhost) e escuta na porta 44316. |
    | **Incluir cliente nativo** | Não | Uma vez que se trata de uma aplicação Web e não um cliente nativo, selecione Não. |
    
3. Clique em **Criar** para registar a aplicação.

As aplicações registadas são apresentadas na lista de aplicações para o inquilino do Azure AD B2C. Selecione a aplicação Web na lista. É apresentado o painel de propriedades da aplicação Web.

![Propriedades da aplicação Web](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Aponte o **ID da Aplicação**. O ID identifica exclusivamente a aplicação e é necessário quando configurar a aplicação mais tarde no tutorial.

### <a name="create-a-client-password"></a>Criar uma palavra-passe de cliente

O Azure AD B2C utiliza a autorização de OAuth2 para [aplicações cliente](../active-directory/develop/developer-glossary.md#client-application). As aplicações Web são [clientes confidenciais](../active-directory/develop/developer-glossary.md#web-client) e requerem um ID de cliente ou um ID de aplicação e um segredo de cliente, palavra-passe de cliente ou chave de aplicação.

1. Selecione a página Chaves para a aplicação Web registada e clique em **Gerar chave**.

2. Clique em **Guardar** para apresentar a chave da aplicação.

    ![página de chaves geral da aplicação](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

A chave é apresentada uma vez no portal. É importante copiar e guardar o valor da chave. Este valor é necessário para configurar a aplicação. Mantenha a chave segura. Não partilhe a chave publicamente.

## <a name="create-user-flows"></a>Criar fluxos de utilizador

Um fluxo de utilizador do Azure AD B2C define a experiência do usuário para uma tarefa de identidade. Por exemplo, ao iniciar sessão, inscrever-se, a alteração de palavras-passe e editar perfis é fluxos de utilizador comuns.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Criar um fluxo de utilizador de inscrição ou início de sessão

Para inscrever utilizadores para acederem e iniciarem sessão na aplicação web, crie uma **fluxo de utilizador de inscrição ou início de sessão**.

1. Na página do portal do Azure AD B2C, selecione **fluxos de utilizador** e clique em **novo fluxo de utilizador**.
2. Sobre o **recomendado** separador, clique em **inscrever-se e iniciar sessão**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições:

    ![Adicionar um fluxo de utilizador de inscrição ou início de sessão](media/active-directory-b2c-tutorials-web-app/add-susi-user-flow.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiUpIn | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **b2c_1_**. Utilize o nome do fluxo de utilizador completo **b2c_1_SiUpIn** no código de exemplo. | 
    | **Fornecedores de identidade** | Inscrever-se no e-mail | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |

3. Sob **atributos de utilizador e afirmações**, clique em **mostrar mais** e selecione as seguintes definições:

    ![Adicionar um fluxo de utilizador de inscrição ou início de sessão](media/active-directory-b2c-tutorials-web-app/add-attributes-and-claims.png)

    | Coluna      | Valores sugeridos  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recolher o atributo** | Nome a Apresentar e Código Postal | Selecione os atributos a recolher do utilizador durante a inscrição. |
    | **Declaração de retorna** | Nome a Apresentar, Código Postal, O utilizador é novo, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token). |

4. Clique em **OK**.
5. Clique em **criar** para criar o fluxo de utilizador. 

### <a name="create-a-profile-editing-user-flow"></a>Criar um fluxo de utilizador de edição de perfil

Para permitir que os utilizadores reponham as suas informações de perfil de utilizador por conta própria, crie uma **fluxo de utilizador de edição de perfil**.

1. Na página do portal do Azure AD B2C, selecione **fluxos de utilizador** e clique em **novo fluxo de utilizador**.
2. Sobre o **recomendado** separador, clique em **edição de perfil**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições:

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiPe | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **b2c_1_**. Utilize o nome do fluxo de utilizador completo **b2c_1_SiPe** no código de exemplo. | 
    | **Fornecedores de identidade** | Inscrição da conta local | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |

3. Sob **atributos de utilizador**, clique em **mostrar mais** e selecione as seguintes definições:

    | Coluna      | Valores sugeridos  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recolher o atributo** | Nome a Apresentar e Código Postal | Selecione os atributos que os utilizadores podem modificar durante a edição de perfil. |
    | **Declaração de retorna** | Nome a Apresentar, Código Postal, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token) após uma edição de perfil com êxito. |

4. Clique em **OK**.
5. Clique em **criar** para criar o fluxo de utilizador. 

### <a name="create-a-password-reset-user-flow"></a>Criar um fluxo de utilizador de reposição de palavra-passe

Para ativar na sua aplicação de reposição de palavra-passe, tem de criar uma **fluxo de utilizador de reposição de palavra-passe**. Este fluxo de utilizador descreve a experiência de consumidor durante a reposição de palavra-passe e os conteúdos de tokens que a aplicação recebe a conclusão com êxito.

1. Na página do portal do Azure AD B2C, selecione **Políticas de reposição de palavra-passe** e clique em **Adicionar**.
2. Sobre o **recomendado** separador, clique em **reposição de palavra-passe**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SSPR | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **b2c_1_**. Utilize o nome do fluxo de utilizador completo **b2c_1_SSPR** no código de exemplo. | 
    | **Fornecedores de identidade** | Repor a palavra-passe com o endereço de e-mail | Trata-se do fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |

3. Sob **afirmações de aplicação**, clique em **mostrar mais** e selecione a definição seguinte:
    | Coluna      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Declaração de retorna** | ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token) após uma reposição de palavra-passe com êxito. |

4. Clique em **OK**.
5. Clique em **criar** para criar o fluxo de utilizador. 

## <a name="update-web-app-code"></a>Atualizar o código da aplicação Web

Agora que tem uma aplicação web registada e fluxos de utilizador criados, terá de configurar a sua aplicação para utilizar o inquilino do Azure AD B2C. Neste tutorial, irá configurar uma aplicação Web de exemplo que pode transferir a parir do GitHub. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) ou clone a aplicação Web de exemplo a partir do GitHub. Certifique-se de que extrai o ficheiro de exemplo numa pasta em que o comprimento de carateres total do caminho seja inferior a 260.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

A aplicação Web ASP.NET de exemplo é uma aplicação de lista de tarefas simples para criar e atualizar uma lista de tarefas. A aplicação utiliza [componentes middleware do Microsoft OWIN](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/) para permitir aos utilizadores inscreverem-se para utilizar a aplicação no inquilino do Azure AD B2C. Ao criar um fluxo de utilizador do Azure AD B2C, os utilizadores podem utilizar uma conta de redes sociais ou crie uma conta para utilizar como identidade para aceder à aplicação. 

Existem dois projetos na solução de exemplo:

**Aplicação de exemplo de aplicação da Web (TaskWebApp):** Aplicação Web para criar e editar uma lista de tarefas. A aplicação web utiliza a **inscrição ou início de sessão** fluxo de utilizador para inscrever-se ou iniciar sessão dos utilizadores.

**Aplicação de exemplo da Web API (TaskService):** API Web que suporta o criar, ler, atualizar e eliminar a funcionalidade de lista de tarefas. A API Web é protegida pelo Azure AD B2C e chamada pela aplicação Web.

Tem de alterar a aplicação de modo a utilizar o registo de aplicações no seu inquilino, o que inclui o ID da aplicação e a chave que registou anteriormente. Terá também de configurar os fluxos de utilizador que criou. A aplicação Web de exemplo define os valores de configuração como definições da aplicação no ficheiro Web.config. Para alterar as definições da aplicação:

1. Abra a solução **B2C-WebAPI-DotNet** no Visual Studio.

2. No projeto da aplicação Web **TaskWebApp**, abra o ficheiro **Web.config**. Substitua o valor de `ida:Tenant` pelo nome do inquilino que criou. Substitua o valor de `ida:ClientId` pelo ID da aplicação que registou. Substitua o valor de `ida:ClientSecret` pela chave que registou.

3. No ficheiro **Web.config**, substitua o valor de `ida:SignUpSignInPolicyId` por `b2c_1_SiUpIn`. Substitua o valor de `ida:EditProfilePolicyId` por `b2c_1_SiPe`. Substitua o valor de `ida:ResetPasswordPolicyId` por `b2c_1_SSPR`.

## <a name="run-the-sample-web-app"></a>Executar a aplicação Web de exemplo

No Explorador de Soluções, clique com o botão direito do rato no projeto **TaskWebApp** e clique em **Configurar como Projeto de Arranque**

Prima **F5** para iniciar a aplicação Web. É iniciado o browser predefinido para o endereço do site local `https://localhost:44316/`. 

A aplicação de exemplo suporta inscrição, início de sessão, edição de perfil e reposição de palavra-passe. Este tutorial destaca como um utilizador se inscreve para utilizar a aplicação com um endereço de e-mail. Pode explorar outros cenários.

### <a name="sign-up-using-an-email-address"></a>Inscrever-se com um endereço de e-mail

1. Clique na ligação **Inscrever-se/Iniciar sessão** na faixa superior para se inscrever como um utilizador da aplicação Web. Esta opção utiliza a **b2c_1_SiUpIn** fluxo de utilizador definida no passo anterior.

2. O Azure AD B2C apresenta uma página de início de sessão com uma ligação de inscrição. Uma vez que ainda não tem uma conta, clique na ligação **Inscrever-se agora**. 

3. O fluxo de trabalho de inscrição apresenta uma página para recolher e verificar a identidade do utilizador através de um endereço de e-mail. O fluxo de trabalho de inscrição também recolhe a senha do usuário e os atributos solicitados definidos no fluxo de utilizador.

    Utilize um endereço de e-mail válido e valide com o código de verificação. Defina uma palavra-passe. Introduza os valores para os atributos solicitados. 

    ![Fluxo de trabalho de inscrição](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Clique em **Criar** para criar uma conta local no inquilino do Azure AD B2C.

Agora, o utilizador pode utilizar o respetivo endereço de e-mail para iniciar sessão e utilizar a aplicação Web.

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a criar um inquilino do Azure AD B2C, criar fluxos de utilizador e atualizar a aplicação web de exemplo para utilizar o inquilino do Azure AD B2C. Avance para o próximo tutorial para aprender a registar, configurar e chamar uma API Web ASP.NET protegida pelo inquilino do Azure AD B2C.

> [!div class="nextstepaction"]
> [Tutorial: Utilizar o Azure Active Directory B2C para proteger uma API web ASP.NET](active-directory-b2c-tutorials-web-api.md)
