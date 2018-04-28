---
title: Configuração do twitter para o Azure AD B2C | Microsoft Docs
description: Forneça a inscrição e o início de sessão para os clientes com contas do Twitter nas aplicações que estejam protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 4/17/2018
ms.author: davidmu
ms.openlocfilehash: 40e4c5549414765dabc6f37c5ffb5aea519ae673
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Fornecer inscrição e o início de sessão para os consumidores de contas do Twitter com o Azure AD B2C

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter
Para utilizar o Twitter como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação do Twitter e forneça-lo com os parâmetros corretos. Necessita de uma conta do Twitter para efetuar este procedimento. Se não tiver uma, pode obtê-lo em [ https://twitter.com/signup ](https://twitter.com/signup).

1. Vá para o [Twitter aplicações](https://apps.twitter.com/) e inicie sessão com as suas credenciais.
2. Clique em **criar nova aplicação**. 
3. No formulário, forneça um valor para o **nome**, **Descrição**, e **Web site**.
4. Para o **URL de chamada de retorno**, introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Certifique-se de que substitui **{inquilino}** com o nome do seu inquilino (por exemplo, contosob2c.onmicrosoft.com).
5. Selecione a caixa para aceitar o **programador contrato** e clique em **criar a sua aplicação Twitter**.
6. Depois da aplicação é criada, clique em **chaves e os Tokens de acesso**.
7. Copie o valor da **consumidor chave** e **segredo de consumidor**. Terá de configurar o Twitter como um fornecedor de identidade no seu inquilino.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configurar o Twitter como um fornecedor de identidade no seu inquilino
1. Inicie sessão no [portal do Azure](https://portal.azure.com/) como Administrador Global do inquilino do Azure AD B2C. 
2. Para mudar para o seu inquilino do Azure AD B2C, selecione o diretório do Azure AD B2C no canto superior direito do portal.
3. Clique em **todos os serviços**e, em seguida, selecione **do Azure AD B2C** na lista de serviços em **segurança + identidade**.
4. Clique em **fornecedores de identidade**.
4. Forneça um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Twitter".
5. Clique em **tipo de fornecedor de identidade**, selecione **Twitter (pré-visualização)** e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade** e introduza o Twitter **consumidor chave** para o **id de cliente** e o Twitter **segredo de consumidor** para o **segredo do cliente**.
7. Clique em **OK**e, em seguida, clique em **criar** para guardar a configuração do Twitter.

## <a name="next-steps"></a>Passos Seguintes

Criar ou editar um [política incorporada](active-directory-b2c-reference-policies.md) e adicione o Twitter como um fornecedor de identidade.