---
title: Configurar a inscrição e início de sessão com uma conta do LinkedIn com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do LinkedIn nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 84b1ad2ecd2c027c7d8a105579059ceb957f41c6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929003"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do LinkedIn com o Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Criar uma aplicação do LinkedIn

Para utilizar uma conta do LinkedIn como fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do LinkedIn, pode obtê-la em [ https://www.linkedin.com/ ](https://www.linkedin.com/).

1. Inicie sessão para o [Web site de desenvolvedores do LinkedIn](https://www.developer.linkedin.com/) com as suas credenciais de conta do LinkedIn.
2. Selecione **as minhas aplicações**e, em seguida, clique em **Criar aplicação**.
3. Introduza **nome da empresa**, **nome da aplicação**, **descrição da aplicação**, **logótipo da aplicação**, **utilização de aplicações** , **URL do site**, **E-Mail empresariais**, e **telefone da empresa**.
4. Concorde com o **LinkedIn API termos de utilização** e clique em **submeter**.
5. Copie os valores da **ID de cliente** e **segredo do cliente**. Pode encontrá-los sob **chaves de autenticação**. Precisará ambos para configurar o LinkedIn como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.
6. Introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **autorizado URLs de redirecionamento**. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contoso.onmicrosoft.com). Selecione **Add**e, em seguida, clique em **atualização**.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Configurar uma conta do LinkedIn como fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-li-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-li-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *LinkedIn*.
6. Selecione **tipo de fornecedor de identidade**, selecione **LinkedIn**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o Id de cliente que registou anteriormente como a **ID de cliente** e introduza o segredo do cliente que registou como o **segredo do cliente**da aplicação de conta do LinkedIn que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de conta do LinkedIn.

