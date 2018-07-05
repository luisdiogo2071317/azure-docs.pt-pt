---
title: Configuração de provedor de identidade do GitHub no Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os clientes com contas do GitHub em seus aplicativos que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3754a169b301bac97f3e12d10b754222e3cf325d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443346"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>O Azure Active Directory B2C: Fornecer inscrição e início de sessão para os consumidores com contas do GitHub

> [!NOTE]
> Esta funcionalidade está em pré-visualização.
> 

Este artigo mostra como ativar o início de sessão para os utilizadores com uma conta do GitHub.

## <a name="create-a-github-oauth-application"></a>Criar uma aplicação de OAuth do GitHub

Para utilizar o GitHub como um fornecedor de identidade no Azure AD B2C, terá de criar uma aplicação de OAuth do GitHub e fornecer os parâmetros certos.

1. Vá para o [definições do programador do GitHub](https://github.com/settings/developers) depois de iniciar sessão no GitHub.
1. Clique em **nova aplicação de OAuth**
1. Introduza um **nome da aplicação** e a sua **URL da home page**.
1. Para o **URL de chamada de retorno de autorização**, introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Substitua **{inquilino}** com o nome do seu inquilino de B2C do Azure AD (por exemplo, contosob2c.onmicrosoft.com).

    >[!NOTE]
    >O valor para "tenant" tem de estar todo em minúsculo no **URL de início de sessão**.

1. Clique em **registar a aplicação**.
1. Guarde os valores da **ID de cliente** e **segredo do cliente**. Precisará ambos na próxima seção.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Configurar o GitHub como um fornecedor de identidade no seu inquilino do Azure AD B2C

1. Siga estes passos para [navegue para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
1. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
1. Clique em **+ Adicionar** na parte superior do painel.
1. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "GitHub".
1. Clique em **tipo de fornecedor de identidade**, selecione **GitHub**e clique em **OK**.
1. Clique em **configurar este fornecedor de identidade** e introduza o ID de cliente e segredo do cliente da aplicação OAuth do GitHub que copiou anteriormente.
1. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração do GitHub.

## <a name="next-steps"></a>Passos Seguintes

Criar ou editar um [política incorporada](active-directory-b2c-reference-policies.md) e adicionar o GitHub como um fornecedor de identidade.