---
title: Configuração do Facebook no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os consumidores com contas do Facebook em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 8/7/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 316e44ea92a25ab804c8cc499f91c45e4a66ef02
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445505"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas do Facebook
## <a name="create-a-facebook-application"></a>Criar um aplicativo do Facebook
Para utilizar o Facebook como provedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar um aplicativo do Facebook e fornecer os parâmetros certos. Precisa de uma conta do Facebook para fazer isso. Se não tiver uma, pode obtê-la em [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Vá para o [Facebook para os desenvolvedores](https://developers.facebook.com/) credenciais da conta de Web site e inicie sessão com o Facebook.
2. Se ainda não o fez, terá de se registrar como um desenvolvedor do Facebook. Para tal, clique em **registar** (no canto direito superior da página), aceite as políticas do Facebook e conclua os passos de registo.
3. Clique em **as minhas aplicações** e, em seguida, clique em **adicionar uma nova aplicação**. 
4. No formulário, indique um **nome a apresentar** e válido **E-Mail de contacto**.
5. Clique em **criar ID de aplicação**. Isso pode exigir a aceitar as políticas de plataforma do Facebook e concluir uma verificação de segurança online.
6. Na coluna esquerda, clique em **configurações** e, em seguida, selecione **básica** se não selecionou já.
7. Selecione um **categoria**. 
8. Clique em **+ adicionar plataforma** e selecione **site**.
   
    ![Facebook - definições](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Web site do Facebook - Settings-](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Introduza `https://login.microsoftonline.com/` no **URL do Site** campo e, em seguida, clique em **guardar alterações** na parte inferior da página.
   
    ![Facebook - URL do Site](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Copie o valor da **ID da aplicação**. Clique em **mostrar** e copie o valor de **segredo da aplicação**. Precisará ambos para configurar o Facebook como provedor de identidade no seu inquilino. **Segredo da aplicação** é uma credencial de segurança importantes.
   
    ![Facebook - ID de aplicação e segredo da aplicação](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Clique em **+ adicionar produto** no painel de navegação esquerdo e, em seguida, o **configurada** botão para **início de sessão do Facebook**.
   
    ![Facebook - início de sessão do Facebook](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Clique em **configurações** na barra de navegação direita em **logon no Facebook**

    ![Facebook - definições de início de sessão do Facebook](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` no **URIs de redirecionamento OAuth válido** campo no **definições de cliente OAuth** secção. Substitua **{inquilino}** com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com). Clique em **guardar alterações** na parte inferior da página.
    
    ![Facebook - URI de redirecionamento OAuth](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. Para tornar seu aplicativo para Facebook utilizável pelo Azure AD B2C, terá de torná-la disponível publicamente. Pode fazê-lo ao clicar em **revisão da aplicação** no painel de navegação esquerdo e ao ativar o comutador na parte superior da página para **Sim** e clicando em **confirmar**.
    
    ![Facebook - público de aplicação](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Configurar o Facebook como provedor de identidade no seu inquilino
1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Facebook".
5. Clique em **tipo de fornecedor de identidade**, selecione **Facebook**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade** e introduza o ID da aplicação e o segredo da aplicação (do aplicativo do Facebook que criou anteriormente) no **ID de cliente** e **segredo do cliente** campos, respetivamente.
7. Clique em **OK**e, em seguida, clique em **criar** para guardar a configuração do Facebook.

> [!NOTE]
> Adicionar uma **fornecedor de identidade** ao seu inquilino não modifica as políticas existentes. Lembre-se atualizar as suas políticas, incluindo o fornecedor de identidade que acabou de criar.
>