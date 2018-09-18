---
title: Como editar as suas informações de grupo com o Azure Active Directory | Documentos da Microsoft
description: Saiba como editar as informações de um grupo com o Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731347"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Como: editar as suas informações de grupo com o Azure Active Directory

Utilizar o Azure Active Directory, pode editar as definições de um grupo, incluindo a atualização do seu nome, a descrição ou o tipo de associação.

## <a name="to-edit-your-group-settings"></a>Para editar as definições de grupo
1. Inicie sessão para o [portal do Azure](https://portal.azure.com) com uma conta de Administrador Global do diretório.

2. Selecione **do Azure Active Directory**e, em seguida, selecione **grupos**.

    O **grupos – todos os grupos** é apresentada a página, que mostra todos os grupos do Active Directory.

3. Do **grupos – todos os grupos de** página, escreva o máximo do nome do grupo possível para o **pesquisa** caixa. Para os fins deste artigo, estamos procurando os **política MDM - oeste** grupo.

    Os resultados da pesquisa são apresentados no **pesquisa** caixa, a atualização à medida que escreve mais carateres.

    ![Todos os grupos de página, com texto de pesquisa na caixa de pesquisa](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Selecione o grupo de **política MDM - oeste**e, em seguida, selecione **propriedades** partir o **gerir** área.

    ![Página de descrição geral de grupo com membros e número e a opção de membro realçada](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Atualização do **definições gerais** informações conforme necessário, incluindo:

    ![Definições de propriedades para um grupo](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Nome do grupo.** Edite o nome do grupo existente.
    
    - **Descrição do grupo.** Edite a descrição do grupo existente.

    - **Tipo de grupo.** Não é possível alterar o tipo de grupo depois de ser criado. Para alterar o **tipo de grupo**, tem de eliminar o grupo e criar um novo.
    
    - **Tipo de associação.** Altere o tipo de associação. Para obter mais informações sobre os vários tipos de associação disponíveis, consulte [como: criar um grupo básico e adicionar membros com o portal do Azure Active Directory](active-directory-groups-create-azure-portal.md)
    
    - **ID de objeto.** Não é possível alterar o ID de objeto, mas pode copiá-lo para utilizar nos seus comandos do PowerShell para o grupo. Para mais informações sobre como utilizar cmdlets do PowerShell, veja [cmdlets do Azure Active Directory para configurar definições de grupo](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

- [Ver os seus grupos e membros](active-directory-groups-view-azure-portal.md)

- [Criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md)

- [Como adicionar ou remover membros de um grupo](active-directory-groups-members-azure-portal.md)

- [Gerir regras dinâmicas dos utilizadores num grupo](../users-groups-roles/groups-create-rule.md)

- [Gerir associações de um grupo](active-directory-groups-membership-azure-portal.md)

- [Gerir o acesso a recursos através de grupos](active-directory-manage-groups.md)

- [Associar ou adicionar uma subscrição do Azure ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
