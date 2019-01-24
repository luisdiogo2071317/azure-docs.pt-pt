---
title: Tutorial - Ativar a autenticação de aplicações de ambiente de trabalho com contas através do Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Azure Active Directory B2C para fornecer o início de sessão do utilizador a uma aplicação de ambiente de trabalho .NET.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 357b9f4d307624db838b22581097799d7d7fef4c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857000"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Ativar a autenticação de aplicação de ambiente de trabalho com contas através do Azure Active Directory B2C

Este tutorial mostra-lhe como utilizar o Azure Active Directory (Azure AD) B2C para inscrever e iniciar a sessão dos utilizadores numa aplicação de ambiente de trabalho do Windows Presentation Foundation (WPF). O Azure AD B2C permite às aplicações efetuar a autenticação em contas de redes sociais, contas empresariais e contas do Azure Active Directory através de protocolos padrão abertos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registar o exemplo de aplicação de ambiente de trabalho no seu inquilino do Azure AD B2C.
> * Criar fluxos de utilizador para o utilizador de inscrição, início de sessão, edição de perfil e reposição de palavra-passe.
> * Configurar o exemplo de aplicação para utilizar o seu inquilino do Azure AD B2C.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Criar o seu próprio [Inquilino do Azure AD B2C](active-directory-b2c-get-started.md)
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com as cargas de trabalho de **desenvolvimento no ambiente de trabalho em .NET** e **desenvolvimento na Web e em ASP.NET**.

## <a name="register-desktop-app"></a>Registar uma aplicação de ambiente de trabalho

As aplicações têm de ser [registadas](../active-directory/develop/developer-glossary.md#application-registration) no inquilino antes de poderem receber [tokens de acesso](../active-directory/develop/developer-glossary.md#access-token) do Azure Active Directory. O registo de aplicações cria um [ID de aplicação](../active-directory/develop/developer-glossary.md#application-id-client-id) para a aplicação no inquilino. 

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
    | **URI de Redirecionamento Personalizado** | `com.onmicrosoft.contoso.appname://redirect/path` | Introduza `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` fluxos de utilizador para enviar tokens para este URI. |
    
3. Clique em **Criar** para registar a aplicação.

As aplicações registadas são apresentadas na lista de aplicações para o inquilino do Azure AD B2C. Selecione a sua aplicação de ambiente de trabalho da lista. É apresentado o painel de propriedades da aplicação de ambiente de trabalho registada.

![Propriedades da aplicação de ambiente de trabalho](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a aplicação e é necessário quando configurar a aplicação mais tarde no tutorial.

## <a name="create-user-flows"></a>Criar fluxos de utilizador

Um fluxo de utilizador do Azure AD B2C define a experiência do usuário para uma tarefa de identidade. Por exemplo, ao iniciar sessão, inscrever-se, a alteração de palavras-passe e editar perfis é fluxos de utilizador comuns.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Criar um fluxo de utilizador de inscrição ou início de sessão

Para inscrever utilizadores para acederem e iniciarem sessão na aplicação da área de trabalho, crie uma **fluxo de utilizador de inscrição ou início de sessão**.

1. Na página do portal do Azure AD B2C, selecione **fluxos de utilizador** e clique em **novo fluxo de utilizador**.
2. Sobre o **recomendado** separador, clique em **inscrever-se e iniciar sessão**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições:

    ![Adicionar um fluxo de utilizador de inscrição ou início de sessão](media/active-directory-b2c-tutorials-desktop-app/add-susi-user-flow.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiUpIn | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **B2C_1_**. Utilize o nome do fluxo de utilizador completo **B2C_1_SiUpIn** no código de exemplo. | 
    | **Fornecedores de identidade** | Inscrever-se no e-mail | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |

3.  Sob **atributos de utilizador e afirmações**, clique em **mostrar mais** e selecione as seguintes definições:

    ![Adicionar um utilizador e afirmações de atributos](media/active-directory-b2c-tutorials-desktop-app/add-attributes-and-claims.png)

    | Coluna      | Valores sugeridos  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recolher o atributo** | Nome a Apresentar e Código Postal | Selecione os atributos a recolher do utilizador durante a inscrição. |
    | **Declaração de retorna** | Nome a Apresentar, Código Postal, O utilizador é novo, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token). |

4. Clique em **OK**.

5. Clique em **criar** para criar o fluxo de utilizador. 

### <a name="create-a-profile-editing-user-flow"></a>Criar um fluxo de utilizador de edição de perfil

Para permitir que os utilizadores reponham as suas informações de perfil de utilizador por conta própria, crie uma **fluxo de utilizador de edição de perfil**.

1. Na página do portal do Azure AD B2C, selecione **fluxo de utilizador** e clique em **novo fluxo de utilizador**.
2. Sobre o **recomendado** separador, clique em **edição de perfil**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições:

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiPe | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **B2C_1_**. Utilize o nome do fluxo de utilizador completo **B2C_1_SiPe** no código de exemplo. | 
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

1. Na página do portal do Azure AD B2C, selecione **fluxos de utilizador** e clique em **novo fluxo de utilizador**.
2. Sobre o **recomendado** separador, clique em **reposição de palavra-passe**.

    Para configurar o fluxo de utilizador, utilize as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SSPR | Introduza um **nome** para o fluxo de utilizador. O nome do fluxo de utilizador tem o prefixo **B2C_1_**. Utilize o nome do fluxo de utilizador completo **B2C_1_SSPR** no código de exemplo. | 
    | **Fornecedores de identidade** | Repor a palavra-passe com o endereço de e-mail | Trata-se do fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |

3. Sob **afirmações de aplicação**, clique em **mostrar mais** e selecione a definição seguinte:

    | Coluna      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Declaração de retorna** | ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token) após uma reposição de palavra-passe com êxito. |

4. Clique em **OK**.
5. Clique em **criar** para criar o fluxo de utilizador. 

## <a name="update-desktop-app-code"></a>Atualizar o código da aplicação de ambiente de trabalho

Agora que tem uma aplicação de ambiente de trabalho registada e fluxos de utilizador criados, terá de configurar a sua aplicação para utilizar o inquilino do Azure AD B2C. Neste tutorial, irá configurar um exemplo de aplicação de ambiente de trabalho. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [navegar no repositório](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) ou clone o exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

O exemplo de aplicação de ambiente de trabalho WPF demonstra como uma aplicação de ambiente de trabalho pode utilizar o Azure AD B2C para inscrever e iniciar a sessão do utilizador e chamar uma API Web protegida.

Terá de alterar a aplicação para utilizar o registo de aplicação no seu inquilino e configurar os fluxos de utilizador que criou. 

Para alterar as definições da aplicação:

1. Abra a solução `active-directory-b2c-wpf` no Visual Studio.

2. No projeto `active-directory-b2c-wpf`, abra o ficheiro **App.xaml.cs** e faça as seguintes atualizações:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Atualização do **PolicySignUpSignIn** variável com o *fluxo de utilizador de inscrição ou início de sessão* nome criado num passo anterior. Não se esqueça de incluir o prefixo *B2C_1_*.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Executar o exemplo de aplicação de ambiente de trabalho

Prima **F5** para compilar e executar a aplicação de ambiente de trabalho. 

A aplicação de exemplo suporta inscrição, início de sessão, edição de perfil e reposição de palavra-passe. Este tutorial destaca como um utilizador se inscreve para utilizar a aplicação com um endereço de e-mail. Pode explorar outros cenários.

### <a name="sign-up-using-an-email-address"></a>Inscrever-se com um endereço de e-mail

1. Clique no botão **Iniciar Sessão** para se inscrever como utilizador da aplicação de ambiente de trabalho. Esta opção utiliza a **B2C_1_SiUpIn** fluxo de utilizador definida no passo anterior.

2. O Azure AD B2C apresenta uma página de início de sessão com uma ligação de inscrição. Uma vez que ainda não tem uma conta, clique na ligação **Inscrever-se agora**. 

3. O fluxo de trabalho de inscrição apresenta uma página para recolher e verificar a identidade do utilizador através de um endereço de e-mail. O fluxo de trabalho de inscrição também recolhe a senha do usuário e os atributos solicitados definidos no fluxo de utilizador.

    Utilize um endereço de e-mail válido e valide com o código de verificação. Defina uma palavra-passe. Introduza os valores para os atributos solicitados. 

    ![Fluxo de trabalho de inscrição](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Clique em **Criar** para criar uma conta local no inquilino do Azure AD B2C.

Agora, o utilizador pode utilizar o respetivo endereço de e-mail para iniciar sessão e utilizar a aplicação de ambiente de trabalho.

> [!NOTE]
> Se clicar no botão **Chamar API**, receberá o erro "Não autorizado". Recebe este erro porque está a tentar aceder a um recurso a partir do inquilino de demonstração. Como o seu token de acesso só é válido para o seu inquilino do Azure AD, a chamada à API não é autorizada. Avance para o próximo tutorial para criar uma API Web protegida para o seu inquilino. 

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o inquilino do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for necessário, pode [eliminar o inquilino do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a criar um inquilino do Azure AD B2C, criar fluxos de utilizador e atualizar a aplicação de ambiente de trabalho de exemplo para utilizar o inquilino do Azure AD B2C. Avance para o próximo tutorial para aprender a registar, configurar e chamar uma API Web protegida a partir de uma aplicação de ambiente de trabalho.

> [!div class="nextstepaction"]
> 
