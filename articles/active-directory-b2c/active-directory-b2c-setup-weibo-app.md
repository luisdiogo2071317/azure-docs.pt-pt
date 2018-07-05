---
title: Configuração de Weibo no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os consumidores com contas de Weibo em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bfd7dde290bd040f8457e6d095fdf896e802764b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444794"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas de Weibo

> [!NOTE]
> Esta funcionalidade está em pré-visualização. Não utilize este fornecedor de identidade no seu ambiente de produção.
> 

## <a name="create-a-weibo-application"></a>Criar uma aplicação de Weibo

Para utilizar Weibo como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação de Weibo e fornecer os parâmetros certos. Precisa de uma conta de Weibo para fazer isso. Se não tiver uma, pode obter um de cada [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Registre-se para o programa de desenvolvedores Weibo

1. Vá para o [portal do programador Weibo](http://open.weibo.com/) e inicie sessão com as credenciais da conta Weibo.
2. Depois de iniciar sessão, clique em seu nome de exibição no canto superior direito.
3. Na lista pendente, selecione**编辑开发者信息**(editar informações do desenvolvedor).
4. Introduza as informações necessárias no formulário e clique em**提交**(Submeter).
5. Conclua o processo de verificação de e-mail.
6. Vá para o [página de verificação de identidade](http://open.weibo.com/developers/identity/edit).
7. Introduza as informações necessárias no formulário e clique em**提交**(Submeter).

### <a name="register-a-weibo-application"></a>Registar uma aplicação de Weibo

1. Vá para o [nova página de registo de aplicação Weibo](http://open.weibo.com/apps/new).
2. Introduza as informações de aplicativo necessário.
3. Clique em**创建**(criar).
4. Copie os valores da **chave da aplicação** e **segredo da aplicação**. Irá precisar mais tarde.
5. Carregar as fotografias que necessária e introduza as informações necessárias.
6. Clique em**保存以上信息**(guardar).
7. Clique em**高级信息**(informações avançadas).
8. Clique em**编辑**(editar) junto ao campo para OAuth2.0**授权设置**(URL de redirecionamento).
9. Introduza `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` para OAuth2.0**授权设置**(URL de redirecionamento). Por exemplo, se sua `tenant_name` é contoso.onmicrosoft.com, definir o URL a ser `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Clique em**提交**(Submeter).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Configurar Weibo como fornecedor de identidade no seu inquilino
1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Weibo".
5. Clique em **tipo de fornecedor de identidade**, selecione **Weibo**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade**
7. Introduza o **chave da aplicação** que copiou anteriormente como a **ID de cliente**.
8. Introduza o **segredo da aplicação** que copiou anteriormente como a **segredo do cliente**.
9. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de Weibo.

