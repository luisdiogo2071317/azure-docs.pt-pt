---
title: Criar um grupo de utilizadores no Azure AD | Microsoft Docs
description: Como criar um grupo no Azure Active Directory e adicionar membros ao grupo
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/04/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 82d475e5adadb4e7670f24a6193348c9e1b37a16
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767528"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Criar um grupo e adicionar membros no Azure Active Directory
> [!div class="op_single_selector"]
> * [Portal do Azure](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

Este artigo explica como criar e preencher um novo grupo no Azure Active Directory. Utilize um grupo para realizar tarefas de gestão, tais como atribuir licenças ou permissões a vários utilizadores ou dispositivos em simultâneo.

## <a name="how-do-i-create-a-group"></a>Como crio um grupo?
1. Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **Todos os serviços**, introduza **Utilizador e grupos** na caixa de texto e, em seguida, selecione **Enter**.

   ![Abrir gestão de utilizadores](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. No painel **Utilizadores e grupos**, selecione **Todos os grupos**.

   ![Abrir o painel de grupos](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. No painel **Utilizadores e grupos – Todos os grupos**, selecione o comando **Adicionar**.

   ![Selecionar o comando Adicionar](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. No painel **Grupo**, adicione um nome e descrição ao grupo.
6. Para selecionar membros para adicionar ao grupo, selecione **Atribuído** na caixa **Tipo de associação** e, em seguida, selecione **Membros**. Para obter mais informações sobre como gerir a associação de um grupo de forma dinâmica, veja [Utilizar atributos para criar regras avançadas para a associação a um grupo](../active-directory-groups-dynamic-membership-azure-portal.md).

   ![Selecionar membros a adicionar](./media/active-directory-groups-create-azure-portal/select-members.png)
7. No painel **Membros**, selecione um ou mais utilizadores ou dispositivos para adicionar ao grupo e selecione o botão **Selecionar** na parte inferior do painel para adicioná-los ao grupo. A caixa **Utilizador** filtra a apresentação com base na correspondência da sua entrada a qualquer parte de um nome de utilizador ou dispositivo. Os carateres universais não são aceites nessa caixa.
8. Quando terminar de adicionar os membros ao grupo, selecione **Criar** no painel **Grupo**.    

   ![Criar confirmação do grupo](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Passos seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Ver grupos existentes](active-directory-groups-view-azure-portal.md)
* [Gerir definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerir associações de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas dos utilizadores num grupo](../active-directory-groups-dynamic-membership-azure-portal.md)
