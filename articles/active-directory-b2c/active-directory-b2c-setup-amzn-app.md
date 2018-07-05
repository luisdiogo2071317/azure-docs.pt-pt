---
title: Configuração de Amazon no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os consumidores com contas do Amazon em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e3b3d66b913b595e68c03b68990d1a4806952579
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443707"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas da Amazon
## <a name="create-an-amazon-application"></a>Criar uma aplicação da Amazon
Para utilizar o Amazon como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação do Amazon e fornecer os parâmetros certos. Precisa de uma conta da Amazon para fazer isso. Se não tiver uma, pode obtê-la em [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Vá para o [Centro de programadores do Amazon](https://login.amazon.com/) e inicie sessão com as credenciais da conta Amazon.
2. Se ainda não o fez, clique em **Inscreva-se**, siga os passos de registo do desenvolvedor e aceite a política.
3. Clique em **registar o novo aplicativo**.
   
    ![Registar uma nova aplicação no site da Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Fornecer informações sobre a aplicação (**Name**, **Descrição**, e **URL de aviso de privacidade**) e clique em **guardar**.
   
    ![Fornecer informações sobre a aplicação para registar um novo aplicativo no Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. Na **as definições da Web** secção, copie os valores de **ID de cliente** e **segredo do cliente**. (Tem de clicar o **Mostrar segredo** botão para vê-lo.) Terá de ambos para configurar o Amazon como um fornecedor de identidade no seu inquilino. Clique em **editar** na parte inferior da secção. **Segredo do cliente** é uma credencial de segurança importantes.
   
    ![Fornecer o ID de cliente e segredo do cliente para a sua nova aplicação no Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Introduza `https://login.microsoftonline.com` no **origens de JavaScript permitido** campo e `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **URLs de retorno permitidos** campo. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contoso.onmicrosoft.com). Clique em **Guardar**. O **{inquilino}** valor diferencia maiúsculas de minúsculas.
   
    ![Fornecimento de origens de JavaScript e devolvem URLs para a sua nova aplicação no Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Configurar o Amazon como fornecedor de identidade no seu inquilino
1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Amzn".
5. Clique em **tipo de fornecedor de identidade**, selecione **Amazon**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade** e introduza o ID de cliente e segredo do cliente da aplicação Amazon que criou anteriormente.
7. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração da Amazon.

