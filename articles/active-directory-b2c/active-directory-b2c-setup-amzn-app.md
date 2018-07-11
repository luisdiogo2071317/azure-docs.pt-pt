---
title: Configurar a inscrição e início de sessão com uma conta de Amazon com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Amazon nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5fb6289f75f0c98cc218233d8adb900484ee4a17
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916501"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta de Amazon com o Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Criar uma aplicação da Amazon

Para utilizar uma conta do Amazon como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Amazon pode obtê-la em [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Entrar para o [Centro de programadores do Amazon](https://login.amazon.com/) com as suas credenciais de conta da Amazon.
2. Se ainda não o fez, clique em **Inscreva-se**, siga os passos de registo do desenvolvedor e aceite a política.
3. Selecione **registar o novo aplicativo**.
4. Introduza um **Name**, **Descrição**, e **URL de aviso de privacidade**e, em seguida, clique em **guardar**.
5. Na **as definições da Web** secção, copie os valores de **ID de cliente**. Selecione **Mostrar segredo** para obter o segredo do cliente e, em seguida, copiá-lo. Terá de ambos para configurar uma conta do Amazon como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.
6. No **as definições da Web** secção, selecione **editar**e, em seguida, introduza `https://login.microsoftonline.com` no **permitido origens do JavaScript** e `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` na **permitidos Devolver a URLs**. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contoso.onmicrosoft.com). 
7. Clique em **Guardar**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Configurar uma conta do Amazon como fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione as suas informações de subscrição e, em seguida, selecione **Trocar Diretório**. 

    ![Mudar para o inquilino do Azure AD B2C](./media/active-directory-b2c-setup-fb-app/switch-directories.png)

    Escolha o diretório que contém o seu inquilino.

    ![Selecionar o diretório](./media/active-directory-b2c-setup-fb-app/select-directory.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Introduza um **nome**. Por exemplo, introduza *Amazon*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Amazon**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o ID de cliente que registou anteriormente como a **ID de cliente** e introduza o segredo do cliente que registou como o **segredo do cliente**da aplicação Amazon que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração da Amazon.

