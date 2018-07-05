---
title: Configuração de WeChat no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os consumidores com contas de WeChat em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a18d41a4f45b147790a17664156659d282e710d4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445965"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas de WeChat

> [!NOTE]
> Esta funcionalidade está em pré-visualização.
> 

## <a name="create-a-wechat-application"></a>Criar uma aplicação de WeChat

Para utilizar WeChat como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação de WeChat e fornecer os parâmetros certos. Precisa de uma conta de WeChat para fazer isso. Se não tiver uma, pode obter uma inscrevendo-se através de uma das suas aplicações móveis ou ao utilizar o seu número QQ. Depois disso, obtenha a sua conta registada com o programa de desenvolvedores WeChat. Pode encontrar mais informações [aqui](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registar uma aplicação de WeChat

1. Aceda a [https://open.weixin.qq.com/](https://open.weixin.qq.com/) e inicie sessão.
2. Clique em**管理中心**(Centro de gestão).
3. Siga os passos necessários para registar uma nova aplicação.
4. Para**授权回调域**(URL de retorno de chamada), introduza `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Por exemplo, se sua `tenant_name` é contoso.onmicrosoft.com, definir o URL a ser `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Localize e copie os **ID da aplicação** e **chave da aplicação**. Precisará deles mais tarde.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configurar WeChat como fornecedor de identidade no seu inquilino
1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
2. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "WeChat".
5. Clique em **tipo de fornecedor de identidade**, selecione **WeChat**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade**
7. Introduza o **chave da aplicação** que copiou anteriormente como a **ID de cliente**.
8. Introduza o **segredo da aplicação** que copiou anteriormente como a **segredo do cliente**.
9. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de WeChat.

