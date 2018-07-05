---
title: Configuração do Google + no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os consumidores com contas do Google + em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fe5722e8d5a0a8a5bf172577777ccb899fb7b94d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443431"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas do Google +
## <a name="create-a-google-application"></a>Criar uma aplicação do Google +
Para utilizar Google + como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação do Google + e fornecer os parâmetros certos. Precisa de uma conta do Google + para fazer isso. Se não tiver uma, pode obtê-la em [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Vá para o [consola de programadores da Google](https://console.developers.google.com/) e inicie sessão com as credenciais da conta Google +.
2. Clique em **criar projeto**, introduza um **nome do projeto**e, em seguida, clique em **criar**.
   
    ![Google + - introdução](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - novo projeto](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Clique em **Gestor de API** e, em seguida, clique em **credenciais** na navegação à esquerda.
4. Clique nas **ecrã de consentimento do OAuth** separador na parte superior.
   
    ![Google + - credenciais](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Selecione ou especifique válido **endereço de E-Mail**, forneça um **nome do produto**e clique em **guardar**.
   
    ![Google + - ecrã de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Clique em **novas credenciais** e, em seguida, escolha **ID de cliente OAuth**.
   
    ![Google + - ecrã de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Sob **tipo de aplicação**, selecione **aplicação Web**.
   
    ![Google + - ecrã de consentimento do OAuth](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Fornecer um **Name** para a sua aplicação, introduza `https://login.microsoftonline.com` no **JavaScript autorizados origens** campo, e `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **autorizado URIs de redirecionamento** campo. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com). O **{inquilino}** valor diferencia maiúsculas de minúsculas. Clique em **Criar**.
   
    ![Google + - criar ID de cliente](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Copie os valores da **ID de cliente** e **segredo do cliente**. Precisará ambos para configurar o Google + como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.
   
    ![Google + - segredo do cliente](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Configurar o Google + como um fornecedor de identidade no seu inquilino
1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "G +".
5. Clique em **tipo de fornecedor de identidade**, selecione **Google**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade** e introduza o ID de cliente e segredo do cliente da aplicação do Google + que criou anteriormente.
7. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração do Google +.

