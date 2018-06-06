---
title: Configuração do twitter para o Azure Active Directory B2C | Microsoft Docs
description: Forneça a inscrição e o início de sessão para os clientes com contas do Twitter nas aplicações que estejam protegidas pelo Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 4/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 68b9e9edbacda08bc98b6b7af5d944d3df9edca1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709585"
---
# <a name="provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts-using-azure-ad-b2c"></a>Fornecer inscrição e o início de sessão para os consumidores de contas do Twitter com o Azure AD B2C

## <a name="create-a-twitter-application"></a>Criar uma aplicação do Twitter
Para utilizar o Twitter como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação do Twitter e forneça-lo com os parâmetros corretos. Necessita de uma conta do Twitter para efetuar este procedimento. Se não tiver uma, pode obtê-lo em [ https://twitter.com/signup ](https://twitter.com/signup).

1. Vá para o [Twitter aplicações](https://apps.twitter.com/) e inicie sessão com as suas credenciais.
2. Clique em **criar nova aplicação**. 
3. No formulário, forneça um valor para o **nome**, **Descrição**, e **Web site**.
4. Para o **URL de chamada de retorno**, introduza `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Certifique-se de que substitui **{inquilino}** com o nome do seu inquilino (por exemplo, contosob2c.onmicrosoft.com).
5. Selecione a caixa para aceitar o **programador contrato** e clique em **criar a sua aplicação Twitter**.
6. Depois da aplicação é criada, selecione-o na lista e, em seguida, selecione o **definições** separador.
7. Limpar o **ativar o bloqueio de chamada de retorno** caixa e, em seguida, clique em **atualizar as definições de**.
8. Selecione o **chaves e os Tokens de acesso** separador.
9. Copie o valor da **consumidor chave** e **segredo de consumidor**. Terá de configurar o Twitter como um fornecedor de identidade no seu inquilino.

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