---
title: Configurar a inscrição e início de sessão com uma conta Microsoft através do Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os clientes com contas Microsoft nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 16e4dbac4c8146b048d4d9b76544677a6111e2a5
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900834"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta Microsoft através do Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Criar uma aplicação da conta Microsoft

Para utilizar uma conta Microsoft como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta Microsoft, pode obtê-la em [ https://www.live.com/ ](https://www.live.com/).

1. Inicie sessão para o [Portal de registo de aplicação do Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) com as credenciais da conta Microsoft.
2. No canto superior direito, selecione **adicionar uma aplicação**.
3. Fornecer um **Name** para a sua aplicação e clique em **criar**.
4. Na página de registro, copie o valor da **Id da aplicação**. Utilizá-lo para configurar a sua conta Microsoft como fornecedor de identidade no seu inquilino.
5. Selecione **adicionar plataforma**e, em seguida e escolha **Web**.
6. Introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **redirecionar URLs**. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com).
7. Selecione **gerar nova palavra-passe** sob **segredos da aplicação**. Copie a nova palavra-passe apresentada no ecrã. Precisa, mesmo para configurar uma conta Microsoft como fornecedor de identidade no seu inquilino. Esta palavra-passe é uma credencial de segurança importantes.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Configurar uma conta Microsoft como fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-msa-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-msa-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *MSA*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Account Microsoft**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o Id da aplicação que registou anteriormente como a **ID de cliente** e introduza a palavra-passe que registou como o **segredo do cliente**da aplicação de conta Microsoft que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração da conta Microsoft.

