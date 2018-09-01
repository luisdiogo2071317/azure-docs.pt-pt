---
title: Configurar a inscrição e início de sessão com uma conta do Google com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os clientes com contas do Google nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 477bd6047da639dcf21592a7ec0c1b80844e031e
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337739"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do Google com o Azure Active Directory B2C

## <a name="create-a-google-application"></a>Criar uma aplicação do Google

Para utilizar uma conta do Google como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Google pode obtê-la em [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Inicie sessão para o [consola de programadores da Google](https://console.developers.google.com/) com as suas credenciais de conta do Google.
2. Selecione **criar projeto**e, em seguida, clique em **criar**. Se tiver criado projetos antes, selecione a lista de projetos e, em seguida, selecione **novo projeto**.
3. Introduza um **nome do projeto**e, em seguida, clique em **criar**.
3. Selecione **credenciais** no menu à esquerda e, em seguida, selecione **criar credenciais** > **ID de cliente Oauth**.
4. Selecione **ecrã de consentimento de configurar**.
5. Selecione ou especifique válido **endereço de E-Mail**, forneça um **nome do produto apresentado aos utilizadores**e clique em **guardar**.
6. Sob **tipo de aplicação**, selecione **aplicação Web**.
7. Introduza um **Name** para a sua aplicação, introduza `https://{tenant}.b2clogin.com` no **JavaScript autorizados origens**, e `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` no **URIs de redirecionamento de autorizado**. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c).
8. Clique em **Criar**.
9. Copie os valores da **ID de cliente** e **segredo do cliente**. Precisará ambos para configurar o Google como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Configurar uma conta do Google como fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Introduza um **nome**. Por exemplo, introduza *Google*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Google**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o ID de cliente que registou anteriormente como a **ID de cliente** e introduza o segredo do cliente que registou como o **segredo do cliente**da aplicação do Google que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração do Google.

