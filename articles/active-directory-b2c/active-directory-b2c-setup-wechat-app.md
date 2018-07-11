---
title: Configurar a inscrição e início de sessão com uma conta de WeChat através do Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas de WeChat nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e88187c5035abc28ca9deecaf8517e8a21e38d1d
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952339"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta de WeChat através do Azure Active Directory B2C

> [!NOTE]
> Esta funcionalidade está em pré-visualização.
> 

## <a name="create-a-wechat-application"></a>Criar uma aplicação de WeChat

Para utilizar uma conta de WeChat como fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta de WeChat, pode obter informações em [ http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html ](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registar uma aplicação de WeChat

1. Inicie sessão no [ https://open.weixin.qq.com/ ](https://open.weixin.qq.com/) com as suas credenciais de WeChat.
2. Selecione**管理中心**(Centro de gestão).
3. Siga os passos para registar uma nova aplicação.
4. Introduza `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` no**授权回调域**(URL de retorno de chamada). Por exemplo, se sua `tenant_name` é contoso.onmicrosoft.com, definir o URL a ser `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Copiar o **ID da aplicação** e **chave da aplicação**. Estes são necessários para adicionar o fornecedor de identidade para o seu inquilino.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configurar WeChat como fornecedor de identidade no seu inquilino

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-wechat-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-wechat-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *WeChat*.
6. Selecione **tipo de fornecedor de identidade**, selecione **WeChat (pré-visualização)** e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o ID da aplicação que registou anteriormente como a **ID de cliente** e introduza a chave da aplicação que registou como o **segredo do cliente** dos Aplicação de WeChat que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de WeChat.

