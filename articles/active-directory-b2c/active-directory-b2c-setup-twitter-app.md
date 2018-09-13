---
title: Configurar a inscrição e início de sessão com uma conta do Twitter com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Twitter nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5732293510a75a3c40df5cf3d31978c5ce599791
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720162"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do Twitter com o Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter

Para utilizar uma conta do Twitter como fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Twitter, pode obtê-la em [ https://twitter.com/signup ](https://twitter.com/signup).

1. Inicie sessão para o [Twitter aplicações](https://apps.twitter.com/) com as suas credenciais do Twitter.
2. Selecione **criar uma aplicação**.
3. Introduza o **nome da aplicação**, **descrição da aplicação**, e **URL do Web site**.
4. Introduza `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/your-policy-name/oauth1/authresp` no **URLs de retorno de chamada**. Substitua `your-tenant-name` com o nome do seu inquilino e `your-policy-name` com o nome da sua política. Por exemplo, `b2c_1_signupsignin`. Tem de utilizar todas as letras minúsculas, ao introduzir o nome de inquilino e o nome da política, mesmo que foram definidos com letras maiúsculas no Azure AD B2C.
5. Concorde com o **contrato de desenvolvedor** e selecione **criar**.
7. Selecione o **chaves e Tokens de acesso** separador.
8. Copie os valores da **chave de API** e **chave de segredo de API**. Terá de ambos para configurar uma conta do Twitter como fornecedor de identidade no seu inquilino.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configurar o Twitter como fornecedor de identidade no seu inquilino

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.  

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *Twitter*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Twitter**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza a chave de API para o **ID de cliente** e a chave de segredo de API para o **segredo do cliente**.
8. Clique em **OK**e, em seguida, clique em **criar** para guardar a configuração do Twitter.