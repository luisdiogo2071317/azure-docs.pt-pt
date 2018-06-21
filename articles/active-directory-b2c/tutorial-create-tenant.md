---
title: Tutorial - criar um inquilino do Azure Active Directory B2C | Microsoft Docs
description: Saiba como preparar para registar as suas aplicações através da criação de um inquilino do Azure Active Directory B2C no portal do Azure.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/19/2018
ms.author: davidmu
ms.openlocfilehash: 04f3dbbe461bfe0f07b6930a92bdd8a721e55098
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296100"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Criar um inquilino do Azure Active Directory B2C

Antes das aplicações podem interagir com o Azure Active Directory (Azure AD) B2C, tem de estar registados num inquilino que gere.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

Saiba como registar uma aplicação no próximo tutorial.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C

1. Escolha **crie um recurso** no canto superior esquerdo do portal do Azure.
2. Na caixa de pesquisa acima a lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
3. Escolha **criar um novo inquilino do Azure AD B2C**, introduza um nome da organização e o nome de domínio inicial, que é utilizado no nome do inquilino, selecione o país/região e, em seguida, clique em **criar**. Não se esqueça de país do inquilino porque não é possível alterar mais tarde.

    ![Criar um inquilino](./media/tutorial-create-tenant/create-tenant.png)

    Neste exemplo, o nome do inquilino é contoso0522Tenant.onmicrosoft.com

Para começar a gerir o seu inquilino novo, clique na palavra **aqui** onde diz **clique aqui para gerir o seu novo diretório**. Verá um **Troubleshoot** mensagem que indica que precisa de associar a sua subscrição ao novo inquilino. 

## <a name="link-your-tenant-to-your-subscription"></a>Ligar o seu inquilino para a sua subscrição

Terá de ligar o seu inquilino do Azure AD B2C a sua subscrição do Azure para ativar todas as funcionalidades e paga custos de utilização. Se não ligar o seu inquilino para a sua subscrição, as suas aplicações poderá não funcionam corretamente.

1. Certifique-se de que está a utilizar o diretório que contém a subscrição que pretende associar ao novo inquilino ao mudar o diretório no canto superior direito do portal do Azure.

    ![Trocar diretórios](./media/tutorial-create-tenant/switch-directories.png)

    E, em seguida, selecionar o diretório que contém a sua subscrição.

    ![Selecionar o diretório](./media/tutorial-create-tenant/select-directory.png)

2. Escolha **crie um recurso** no canto superior esquerdo superior do portal do Azure.
3. Na caixa de pesquisa acima a lista de recursos do Azure Marketplace, procure e selecione **Active Directory B2C**e, em seguida, clique em **criar**.
4. Escolha **inquilino de ligação do Azure AD B2C existente, a minha subscrição do Azure**, selecione o inquilino que criou, selecione a sua subscrição, introduza *myContosoTenantRG* para o nome do grupo de recursos, aceite o localização e, em seguida, clique em **criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição

> [!div class="nextstepaction"]
> [Ativar uma aplicação web para autenticar com contas](active-directory-b2c-tutorials-web-app.md)