---
title: Configurar a inscrição e início de sessão com uma conta do Facebook com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Facebook nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: de059e3875b5f15526cb176d43a019fd2d9ee9b9
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901386"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do Facebook com o Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Criar um aplicativo do Facebook

Para utilizar uma conta do Facebook como provedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Facebook, pode obtê-la em [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Inicie sessão no [Facebook para os desenvolvedores](https://developers.facebook.com/) com as suas credenciais de conta do Facebook.
2. Se ainda não o fez, terá de se registrar como um desenvolvedor do Facebook. Para tal, selecione **registar** no canto canto superior direito da página, aceite as políticas do Facebook e conclua os passos de registo.
3. Selecione **as minhas aplicações** e, em seguida, clique em **Adicionar nova aplicação**. 
4. Introduza um **nome a apresentar** e válido **E-Mail de contacto**.
5. Clique em **criar ID de aplicação**. Isso pode exigir a aceitar as políticas de plataforma do Facebook e concluir uma verificação de segurança online.
6. Selecione **configurações** > **básica**.
7. Na parte inferior da página, selecione **adicionar plataforma**e, em seguida, selecione **site**.
8. Introduza `https://login.microsoftonline.com/` no **URL do Site**. Introduza um URL para o **URL de política de privacidade**por exemplo, `http://www.contoso.com`.
9. Selecione **guardar alterações**.
11. Na parte superior da página, copie o valor da **ID da aplicação**. 
12. Clique em **mostrar** e copie o valor de **segredo da aplicação**. Utilizar ambos para configurar o Facebook como um fornecedor de identidade no seu inquilino. **Segredo da aplicação** é uma credencial de segurança importantes.
13. Selecione **produtos**e, em seguida, selecione **configurar** sob **início de sessão do Facebook**.
14. Selecione **configurações** sob **início de sessão do Facebook**.
15. Introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **URIs de redirecionamento OAuth válido** . Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com). Clique em **guardar alterações** na parte inferior da página.
16. Para disponibilizar a aplicação do Facebook para o Azure AD B2C, selecione **revisão da aplicação**, defina **público de tornar a minha aplicação?** para **Sim**, escolha uma categoria, por exemplo `Business and Pages`e, em seguida, clique em **confirmar**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configurar uma conta do Facebook como um fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Introduza um **nome**. Por exemplo, introduza *Facebook*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Facebook**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o ID da aplicação que registou anteriormente como a **ID de cliente** e introduza o segredo de aplicação que registou como o **segredo do cliente** de o aplicativo para Facebook que criou anteriormente).
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração do Facebook.