---
title: Configuração do twitter para o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Twitter nas suas aplicações que são protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 6/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dad35f26496306558a6e0105db86321c497a8306
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342972"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Fornecer a inscrição e início de sessão para os consumidores com contas do Twitter com o Azure AD B2C

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter

Para utilizar o Twitter como fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar um aplicativo do Twitter e fornecer os parâmetros certos. Precisa de uma conta do Twitter para fazer isso. Se não tiver uma, pode obtê-la em [ https://twitter.com/signup ](https://twitter.com/signup).

1. Vá para o [Twitter aplicações](https://apps.twitter.com/) e inicie sessão com as suas credenciais.
2. Clique em **criar nova aplicação**.
3. No formulário, forneça um valor para o **Name**, **Descrição**, e **site**.
4. Para o **URL de chamada de retorno**, introduza `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp`. Certifique-se de substituir **{inquilino}** com o nome (por exemplo, contosob2c.onmicrosoft.com) do seu inquilino e o {policyId} com o seu id de política (por exemplo, b2c_1_policy).  Esse retorno de chamada URL tem de ser em letras minúsculas. Deve adicionar um URL de retorno de chamada para todas as políticas que utilizam o início de sessão do Twitter. Certifique-se de usar `b2clogin.com` em vez de ` login.microsoftonline.com` se estiver a utilizá-lo em seu aplicativo.
5. Marque a caixa para aceitar a **contrato de desenvolvedor** e clique em **criar a sua aplicação do Twitter**.
6. Depois da aplicação é criada, selecione-o na lista, selecione o **configurações** separador e, em seguida, clique em **atualizar as definições de**.
7. Selecione o **chaves e Tokens de acesso** separador.
8. Copie o valor da **chave de consumidor** e **segredo de consumidor**. Precisará ambos para configurar o Twitter como fornecedor de identidade no seu inquilino.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Configurar o Twitter como fornecedor de identidade no seu inquilino

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como Administrador Global do inquilino do Azure AD B2C. 
2. Para mudar para o inquilino do Azure AD B2C, selecione o diretório do Azure AD B2C no canto superior direito do portal.
3. Clique em **todos os serviços**e, em seguida, selecione **do Azure AD B2C** na lista de serviços na **segurança + identidade**.
4. Clique em **fornecedores de identidade**.
5. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Twitter".
6. Clique em **tipo de fornecedor de identidade**, selecione **Twitter (pré-visualização)** e clique em **OK**.
7. Clique em **configurar este fornecedor de identidade** e introduza o Twitter **chave de consumidor** para o **id de cliente** e o Twitter **segredo de consumidor** para o **segredo do cliente**.
8. Clique em **OK**e, em seguida, clique em **criar** para guardar a configuração do Twitter.

## <a name="next-steps"></a>Passos Seguintes

Criar ou editar um [política incorporada](active-directory-b2c-reference-policies.md) e adicione o Twitter como fornecedor de identidade.