---
title: Tutorial - criar um inquilino do Azure Active Directory B2C | Documentos da Microsoft
description: Aprenda a preparar para registar as suas aplicações através da criação de um inquilino do Azure Active Directory B2C no portal do Azure.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d5831d868bec940c4e38f62e70e456ae84903ada
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604915"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Criar um inquilino do Azure Active Directory B2C

Antes de seus aplicativos podem interagir com o Azure Active Directory (Azure AD) B2C, tem de estar registados num inquilino que gere.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

Saiba como registar uma aplicação no tutorial seguinte.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C

1. Escolher **criar um recurso** no canto superior esquerdo do portal do Azure.
2. Na caixa de pesquisa acima da lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
3. Escolher **criar um novo inquilino do Azure AD B2C**, insira um nome de organização e o nome de domínio inicial, o que é utilizado no nome do inquilino, selecione o país/região e, em seguida, clique em **criar**. Não se esqueça do país do inquilino porque não é possível alterar mais tarde.

    ![Criar um inquilino](./media/tutorial-create-tenant/create-tenant.png)

    Neste exemplo, o nome do inquilino é contoso0522Tenant.onmicrosoft.com

Para começar a gerir o seu inquilino novo, clique na palavra **aqui** em que se lê **clique aqui para gerir o seu novo diretório**. Verá uma **resolução de problemas** mensagem que diz que precisa de associar a sua subscrição para o novo inquilino. 

## <a name="link-your-tenant-to-your-subscription"></a>Ligar o seu inquilino para a sua subscrição

É preciso associar o seu inquilino do Azure AD B2C à sua subscrição do Azure para ativar todas as funcionalidades e pagar os custos de utilização. Se não ligar o seu inquilino para a sua subscrição, seus aplicativos não funcionará corretamente.

Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino. 

![Mudar para o inquilino do Azure AD B2C](./media/tutorial-create-tenant/switch-directories.png)

1. Escolher **criar um recurso** no canto superior esquerdo do portal do Azure.
2. Na caixa de pesquisa acima da lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
3. Escolher **inquilino de ligação do Azure AD B2C existente, a minha subscrição do Azure**, selecione o inquilino que criou, selecione a sua subscrição, introduza *myContosoTenantRG* para o nome do grupo de recursos, aceite o a localização e clique em **criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

> [!div class="nextstepaction"]
> [Ativar uma aplicação web autenticar com contas](active-directory-b2c-tutorials-web-app.md)