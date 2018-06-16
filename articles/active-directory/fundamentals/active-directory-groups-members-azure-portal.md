---
title: Gerir os membros de um grupo no Azure Active Directory | Microsoft Docs
description: Como adicionar ou remover utilizadores e dispositivos de um grupo no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 43514a612cbc8dcba5ebf33bea369c1d7855ab58
ms.sourcegitcommit: 5821eef990c26fa045e4beacce39f6b02b83156b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/15/2018
ms.locfileid: "35664651"
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Gerir a associação de grupo para os utilizadores no seu inquilino do Azure Active Directory
Este artigo explica como gerir os membros de um grupo no Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Como localizar os membros e geri-los?
1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **todos os serviços**, introduza **utilizadores e grupos** na caixa de texto e, em seguida, selecione **Enter**.

   ![Gestão de utilizadores de abertura](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. No **utilizadores e grupos** painel, selecione **todos os grupos**.

   ![Abrir o painel de grupos](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. No **utilizadores e grupos - todos os grupos** painel, selecione um grupo.
5. No **grupo - *groupname***  painel, selecione **membros**.

   ![Abrir o painel de membros](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Para adicionar membros ao grupo, o **grupo - Membros** painel, selecione **adicionar membros**.

   ![Adicione o comando de membros](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. No **membros** painel, selecione um ou mais utilizadores ou dispositivos a adicionar ao grupo e selecione o **selecione** na parte inferior do painel para adicioná-los ao grupo. O **utilizador** caixa filtra a apresentação com base na correspondência a entrada de qualquer parte de um nome de utilizador ou dispositivo. Não existem carateres universais são aceites na caixa de.
8. Para remover membros do grupo no **grupo - Membros** painel, selecione um membro.
9. No ***membername*** painel, selecione o **remover** comando e confirme a sua escolha na linha de.

   ![remover membros de comando](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Quando terminar de alterar os membros para o grupo, selecione **guardar**.

## <a name="additional-information"></a>Informações adicionais
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Consulte os grupos existentes](active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](active-directory-groups-create-azure-portal.md)
* [Gerir as definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas para os utilizadores de um grupo](../active-directory-groups-dynamic-membership-azure-portal.md)
