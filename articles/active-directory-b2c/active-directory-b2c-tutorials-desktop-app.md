---
title: Tutorial - Ativar a autenticação de aplicações de ambiente de trabalho com contas através do Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Azure Active Directory B2C para fornecer o início de sessão do utilizador a uma aplicação de ambiente de trabalho .NET.
services: active-directory-b2c
author: PatAltimore
ms.author: patricka
ms.reviewer: parja
ms.date: 2/28/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 86d8b105828bdb2a83eac24aebf227b9ae7615e2
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial - Ativar a autenticação de aplicações de ambiente de trabalho com contas através do Azure Active Directory B2C

Este tutorial mostra-lhe como utilizar o Azure Active Directory (Azure AD) B2C para inscrever e iniciar a sessão dos utilizadores numa aplicação de ambiente de trabalho do Windows Presentation Foundation (WPF). O Azure AD B2C permite às aplicações efetuar a autenticação em contas de redes sociais, contas empresariais e contas do Azure Active Directory através de protocolos padrão abertos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar o exemplo de aplicação de ambiente de trabalho no seu inquilino do Azure AD B2C.
> * Criar políticas para inscrição e início de sessão de utilizadores, editar um perfil e repor a palavra-passe.
> * Configurar o exemplo de aplicação para utilizar o seu inquilino do Azure AD B2C.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Criar o seu próprio [Inquilino do Azure AD B2C](active-directory-b2c-get-started.md)
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com as cargas de trabalho de **desenvolvimento no ambiente de trabalho em .NET** e **desenvolvimento na Web e em ASP.NET**.

## <a name="register-desktop-app"></a>Registar uma aplicação de ambiente de trabalho

As aplicações têm de ser [registadas](../active-directory/develop/active-directory-dev-glossary.md#application-registration) no inquilino antes de poderem receber [tokens de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) do Azure Active Directory. O registo de aplicações cria um [ID de aplicação](../active-directory/develop/active-directory-dev-glossary.md#application-id-client-id) para a aplicação no inquilino. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure. 

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**. 

    Para registar a aplicação Web de exemplo no inquilino, utilize as seguintes definições:
    
    ![Adicionar uma nova aplicação](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | O Meu Exemplo de Aplicação WPF | Introduza um **Nome** que descreva a aplicação aos consumidores. | 
    | **Incluir aplicação/API Web** | Não | Selecione **Não** para uma aplicação de ambiente de trabalho. |
    | **Incluir cliente nativo** | Sim | Uma vez que esta é uma aplicação de ambiente de trabalho e é considerada um cliente nativo. |
    | **URI de Redirecionamento** | Valores predefinidos | O identificador exclusivo para o qual o Azure AD B2C redireciona o agente de utilizador numa resposta do OAuth 2.0. |
    | **URI de Redirecionamento Personalizado** | `com.onmicrosoft.contoso.appname://redirect/path` | Introduza Políticas `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` para enviar tokens para este URI. |
    
3. Clique em **Criar** para registar a aplicação.

As aplicações registadas são apresentadas na lista de aplicações para o inquilino do Azure AD B2C. Selecione a sua aplicação de ambiente de trabalho da lista. É apresentado o painel de propriedades da aplicação de ambiente de trabalho registada.

![Propriedades da aplicação de ambiente de trabalho](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a aplicação e é necessário quando configurar a aplicação mais tarde no tutorial.

## <a name="create-policies"></a>Criar políticas

Uma política do Azure AD B2C define os fluxos de trabalho do utilizador. Por exemplo, iniciar sessão, inscrever, alterar palavras-passe e editar perfis são fluxos de trabalho comuns.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Criar uma política de inscrição ou início de sessão

Para inscrever os utilizadores para acederem e iniciarem sessão na aplicação de ambiente de trabalho, crie uma **política de inscrição ou início de sessão**.

1. Na página do portal do Azure AD B2C, selecione **Políticas de inscrição ou início de sessão** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições:

    ![Adicionar uma política de inscrição ou início de sessão](media/active-directory-b2c-tutorials-desktop-app/add-susi-policy.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiUpIn | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SiUpIn** no código de exemplo. | 
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
    | **Nome** | SiPe | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SiPe** no código de exemplo. | 
    | **Fornecedor de identidade** | Inscrição da conta local | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Atributos do perfil** | Nome a Apresentar e Código Postal | Selecione os atributos que os utilizadores podem modificar durante a edição de perfil. |
    | **Afirmações da aplicação** | Nome a Apresentar, Código Postal, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/active-directory-dev-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) após uma edição de perfil com êxito. |

2. Clique em **Criar** para criar a política. 

### <a name="create-a-password-reset-policy"></a>Criar uma política de reposição de palavra-passe

Para ativar a reposição de palavras-passe na aplicação, tem de criar uma **política de reposição de palavra-passe**. Esta política descreve as experiências que os consumidores têm durante a reposição de palavras-passe e os conteúdos de tokens que a aplicação recebe após a conclusão com êxito.

1. Na página do portal do Azure AD B2C, selecione **Políticas de reposição de palavra-passe** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SSPR | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SSPR** no código de exemplo. | 
    | **Fornecedor de identidade** | Repor a palavra-passe com o endereço de e-mail | Trata-se do fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Afirmações da aplicação** | ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/active-directory-dev-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/active-directory-dev-glossary.md#access-token) após uma reposição de palavra-passe com êxito. |

2. Clique em **Criar** para criar a política. 

## <a name="update-desktop-app-code"></a>Atualizar o código da aplicação de ambiente de trabalho

Agora que registou uma aplicação de ambiente de trabalho e criou as políticas, tem de configurar a aplicação para utilizar o inquilino do Azure AD B2C. Neste tutorial, irá configurar um exemplo de aplicação de ambiente de trabalho. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) ou clone o exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

O exemplo de aplicação de ambiente de trabalho WPF demonstra como uma aplicação de ambiente de trabalho pode utilizar o Azure AD B2C para inscrever e iniciar a sessão do utilizador e chamar uma API Web protegida.

Tem de alterar a aplicação para utilizar o registo de aplicações no seu inquilino e configurar as políticas criadas por si. 

Para alterar as definições da aplicação:

1. Abra a solução `active-directory-b2c-wpf` no Visual Studio.

2. No projeto `active-directory-b2c-wpf`, abra o ficheiro **App.xaml.cs** e faça as seguintes atualizações:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Atualize a variável **PolicySignUpSignIn** com o nome da *política de inscrição ou início de sessão* que criou no passo anterior. Não se esqueça de incluir o prefixo *B2C_1_*.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Executar o exemplo de aplicação de ambiente de trabalho

Prima **F5** para compilar e executar a aplicação de ambiente de trabalho. 

A aplicação de exemplo suporta inscrição, início de sessão, edição de perfil e reposição de palavra-passe. Este tutorial destaca como um utilizador se inscreve para utilizar a aplicação com um endereço de e-mail. Pode explorar outros cenários.

### <a name="sign-up-using-an-email-address"></a>Inscrever-se com um endereço de e-mail

1. Clique no botão **Iniciar Sessão** para se inscrever como utilizador da aplicação de ambiente de trabalho. Esta opção utiliza a política **B2C_1_SiUpIn** definida por si num passo anterior.

2. O Azure AD B2C apresenta uma página de início de sessão com uma ligação de inscrição. Uma vez que ainda não tem uma conta, clique na ligação **Inscrever-se agora**. 

3. O fluxo de trabalho de inscrição apresenta uma página para recolher e verificar a identidade do utilizador através de um endereço de e-mail. O fluxo de trabalho de inscrição também recolhe a palavra-passe do utilizador e os atributos solicitados definidos na política.

    Utilize um endereço de e-mail válido e valide com o código de verificação. Defina uma palavra-passe. Introduza os valores para os atributos solicitados. 

    ![Fluxo de trabalho de inscrição](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Clique em **Criar** para criar uma conta local no inquilino do Azure AD B2C.

Agora, o utilizador pode utilizar o respetivo endereço de e-mail para iniciar sessão e utilizar a aplicação de ambiente de trabalho.

> [!NOTE]
> Se clicar no botão **Chamar API**, receberá o erro "Não autorizado". Recebe este erro porque está a tentar aceder a um recurso a partir do inquilino de demonstração. Como o seu token de acesso só é válido para o seu inquilino do Azure AD, a chamada à API não é autorizada. Avance para o próximo tutorial para criar uma API Web protegida para o seu inquilino. 

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar um inquilino do Azure AD B2C, criar políticas e atualizar o exemplo de aplicação de ambiente de trabalho para utilizar o inquilino do Azure AD B2C. Avance para o próximo tutorial para aprender a registar, configurar e chamar uma API Web protegida a partir de uma aplicação de ambiente de trabalho.

> [!div class="nextstepaction"]
> 