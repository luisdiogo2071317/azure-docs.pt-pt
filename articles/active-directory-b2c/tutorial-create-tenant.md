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
ms.openlocfilehash: 20865fc3adf8610b5a4ce111e3db91aef714fdd6
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448310"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Criar um inquilino do Azure Active Directory B2C

Antes de seus aplicativos podem interagir com o Azure Active Directory (Azure AD) B2C, tem de estar registados num inquilino que gere.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

Saiba como registar uma aplicação no tutorial seguinte.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C

1. Escolher **criar um recurso** no canto superior esquerdo do portal do Azure.
2. Na caixa de pesquisa acima da lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
3. Escolher **criar um novo inquilino do Azure AD B2C**, insira um nome de organização e o nome de domínio inicial, o que é utilizado no nome do inquilino, selecione o país/região e, em seguida, clique em **criar**. Não se esqueça do país do inquilino porque não é possível alterar mais tarde.

    ![Criar um inquilino](./media/tutorial-create-tenant/create-tenant.png)

    Neste exemplo, o nome do inquilino é contoso0522Tenant.onmicrosoft.com

Para começar a gerir o seu inquilino novo, clique na palavra **aqui** em que se lê **clique aqui para gerir o seu novo diretório**. Verá uma **resolução de problemas** mensagem que diz que precisa de associar a sua subscrição para o novo inquilino. 

## <a name="link-your-tenant-to-your-subscription"></a>Ligar o seu inquilino para a sua subscrição

Precisa de associar o inquilino do Azure AD B2C à sua subscrição do Azure para ativar todas as funcionalidades e paga os custos de utilização. Se não ligar o seu inquilino para a sua subscrição, seus aplicativos não funcionará corretamente.

1. Certifique-se de que está a utilizar o diretório que contém a subscrição que pretende associar ao novo inquilino, mudando o diretório no canto superior direito do portal do Azure.

    ![Trocar diretórios](./media/tutorial-create-tenant/switch-directories.png)

    E, em seguida, selecionar o diretório que contém a sua subscrição.

    ![Selecionar o diretório](./media/tutorial-create-tenant/select-directory.png)

2. Escolher **criar um recurso** no canto superior esquerdo do portal do Azure.
3. Na caixa de pesquisa acima da lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
4. Escolher **inquilino de ligação do Azure AD B2C existente, a minha subscrição do Azure**, selecione o inquilino que criou, selecione a sua subscrição, introduza *myContosoTenantRG* para o nome do grupo de recursos, aceite o a localização e clique em **criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

> [!div class="nextstepaction"]
> [Ativar uma aplicação web autenticar com contas](active-directory-b2c-tutorials-web-app.md)