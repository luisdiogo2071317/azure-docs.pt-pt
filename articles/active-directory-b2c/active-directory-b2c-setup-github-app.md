---
title: Configuração do fornecedor de identidade GitHub no Azure Active Directory B2C | Microsoft Docs
description: Forneça a inscrição e o início de sessão para os clientes com contas de GitHub nas aplicações que estejam protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 16c7f34c00bbd5bd0c2be53df2b781a1852b84ff
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712213"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-github-accounts"></a>O Azure Active Directory B2C: Proporcionar inscrição e o início de sessão aos consumidores com contas do GitHub

> [!NOTE]
> Esta funcionalidade está em pré-visualização.
> 

Este artigo mostra como ativar o início de sessão para os utilizadores com uma conta do GitHub.

## <a name="create-a-github-oauth-application"></a>Criar uma aplicação de GitHub OAuth

Para utilizar o GitHub como um fornecedor de identidade no Azure AD B2C, terá de criar uma aplicação de GitHub OAuth e forneça-lo com os parâmetros corretos.

1. Vá para o [definições do programador do GitHub](https://github.com/settings/developers) depois de iniciarem no GitHub.
1. Clique em **nova aplicação de OAuth**
1. Introduza um **nome da aplicação** e os seus **home page URL**.
1. Para o **URL de chamada de retorno de autorização**, introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Substitua **{inquilino}** com o nome do seu inquilino do Azure AD B2C (por exemplo, contosob2c.onmicrosoft.com).

    >[!NOTE]
    >O valor para "inquilino" deve ser todo em minúsculas no **URL de início de sessão**.

1. Clique em **registar a aplicação**.
1. Guardar os valores de **ID de cliente** e **segredo do cliente**. Terá de ambos na secção seguinte.

## <a name="configure-github-as-an-identity-provider-in-your-azure-ad-b2c-tenant"></a>Configurar o GitHub como um fornecedor de identidade no seu inquilino do Azure AD B2C

1. Siga estes passos para [navegar para o painel de funcionalidades do B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
1. No painel de funcionalidades do B2C, clique em **fornecedores de identidade**.
1. Clique em **+ Adicionar** na parte superior do painel.
1. Forneça um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "GitHub".
1. Clique em **tipo de fornecedor de identidade**, selecione **GitHub**e clique em **OK**.
1. Clique em **configurar este fornecedor de identidade** e introduza o ID de cliente e o segredo do cliente da aplicação OAuth do GitHub que copiou anteriormente.
1. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de GitHub.

## <a name="next-steps"></a>Passos Seguintes

Criar ou editar um [política incorporada](active-directory-b2c-reference-policies.md) e adicione o GitHub como um fornecedor de identidade.