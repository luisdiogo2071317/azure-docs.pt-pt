---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: bff2543ec48c66c10db697650def0077e3de28be
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133858"
---
Para ativar as palavras-passe detalhadas repor na sua aplicação, utilize um **reposição de palavra-passe** política. Tenha em atenção que a opção de reposição de palavras-passe no âmbito do inquilino está especificada [aqui](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Esta política descreve as experiências que os clientes terão durante a reposição de palavra-passe e os conteúdos de tokens que a aplicação receberá após a conclusão com êxito.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Na secção de políticas de definições, selecione **Políticas de reposição de palavras-passe** e clique em **+ Adicionar**.

![Selecione as políticas de inscrição ou início de sessão e clique no botão Adicionar](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Introduza um **Nome** de política para a aplicação como referência. Por exemplo, introduza `SSPR`.

Selecione **Fornecedores de identidade** e **Repor a palavra-passe com o endereço de e-mail**. Clique em **OK**.

![Selecione Repor a palavra-passe com o endereço de e-mail como fornecedor de identidade e clique no botão OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Selecione **Afirmações de aplicação**. Escolha as afirmações que quer que sejam devolvidas nos tokens de autorização enviados para a sua aplicação depois de uma experiência de edição de reposição de palavras-passe com êxito. Por exemplo, selecione **ID de Objeto do Utilizador**.

![Selecione algumas afirmações de aplicação e clique no botão OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Clique em **Criar** para adicionar a política. A política está listada como **B2C_1_SSPR**. O prefixo **B2C_1_** é acrescentado ao nome.

Abra a política ao selecionar **B2C_1_SSPR**. Verifique as definições especificadas na tabela e clique em **Executar agora**.

![Selecione a política e execute-a](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Definição      | Valor  |
| ------------ | ------ |
| **Aplicações** | Aplicação Contoso B2C |
| **Selecionar o URL de resposta** | `https://localhost:44316/` |

É aberto um novo separador do browser e pode verificar a experiência de consumidor de reposição de palavras-passe na sua aplicação.

> [!NOTE]
> A criação da política e as atualizações demoram até um minuto a serem aplicadas.
>
