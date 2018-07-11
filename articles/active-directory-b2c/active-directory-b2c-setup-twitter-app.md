---
title: Configurar a inscrição e início de sessão com uma conta do Twitter com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Twitter nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bf5ae39d83fd021775fbd18cf23d2e6b9078e748
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927901"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do Twitter com o Azure Active Directory B2C

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter

Para utilizar uma conta do Twitter como fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Twitter, pode obtê-la em [ https://twitter.com/signup ](https://twitter.com/signup).

1. Inicie sessão para o [Twitter aplicações](https://apps.twitter.com/) com as suas credenciais do Twitter.
2. Selecione **criar nova aplicação**.
3. Introduza o **Name**, **Descrição**, e **site**.
4. Introduza `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp` no **URLs de retorno de chamada**. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com) e **{policyId}** com o seu ID de política (por exemplo, b2c_1_policy). Deve adicionar um URL de retorno de chamada para todas as políticas que usam a conta do Twitter. Certifique-se de usar `b2clogin.com` em vez de ` login.microsoftonline.com` se estiver a utilizá-lo em seu aplicativo.
5. Concorde com o **contrato de desenvolvedor** e selecione **criar a sua aplicação do Twitter**.
7. Selecione o **chaves e Tokens de acesso** separador.
8. Copie o valor da **chave de consumidor** e **segredo de consumidor**. Terá de ambos para configurar uma conta do Twitter como fornecedor de identidade no seu inquilino.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configurar o Twitter como fornecedor de identidade no seu inquilino

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-twitter-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-twitter-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *Twitter*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Twitter**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza a chave de consumidor para o **ID de cliente** e o **segredo de consumidor** para o **segredo do cliente**.
8. Clique em **OK**e, em seguida, clique em **criar** para guardar a configuração do Twitter.