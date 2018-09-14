---
title: Como eliminar um grupo com o Azure Active Directory | Documentos da Microsoft
description: Saiba como eliminar um grupo com o Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 014fe487d23a6c75e94ca2708ed15044bd6cf53b
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574336"
---
# <a name="how-to-delete-a-group-using-azure-active-directory"></a>Como: eliminar um grupo com o Azure Active Directory
Pode eliminar um grupo por diversas razões, mas normalmente é porque:

- Definir incorretamente a **tipo de grupo** para a opção errada

- Criado por engano incorretos ou um grupo duplicado 

- Já não precisar do grupo

## <a name="to-delete-a-group"></a>Para eliminar um grupo
1. Inicie sessão para o [portal do Azure](https://portal.azure.com) com uma conta de Administrador Global do diretório.

2. Selecione **do Azure Active Directory**e, em seguida, selecione **grupos**.

3. Partir do **grupos – todos os grupos** página, procure e selecione o grupo que pretende eliminar. Estes passos, vamos utilizar **política MDM - Leste**.

    ![Página de grupos de todos os grupos, nome do grupo realçado](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. Sobre o **política MDM - descrição geral do Leste** página e, em seguida, selecione **eliminar**.

    O grupo é eliminado do seu inquilino do Azure Active Directory.

    ![Política MDM - página de descrição geral do leste, a opção de eliminação realçado](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>Passos Seguintes

- Se eliminar um grupo por engano, pode criá-la novamente. Para obter mais informações, consulte [como criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md).

- Se eliminar um grupo do Office 365 por engano, poderá restaurá-lo. Para obter mais informações, consulte [restaurar um grupo do Office 365 eliminado](../users-groups-roles/groups-restore-deleted.md).
