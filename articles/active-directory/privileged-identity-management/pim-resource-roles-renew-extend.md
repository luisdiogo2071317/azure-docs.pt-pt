---
title: Expandir e reveja as funções nos recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Este documento descreve como expandir e renovar funções dos recursos do Azure para os recursos do PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: cd1524bf03256a2ed706d11a8702c7b5eff5dad4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260510"
---
# <a name="extend-and-review-roles-in-azure-resources-by-using-privileged-identity-management"></a>Expandir e reveja as funções nos recursos do Azure utilizando o Privileged Identity Management

Privileged Identity Management (PIM) para recursos do Azure introduz novos controlos para gerir o ciclo de vida de acesso e a atribuição de recursos do Azure. Os administradores podem atribuir associação utilizando propriedades de data / hora de início e de fim. Quando se aproxima do fim de atribuição, o PIM envia notificações de correio eletrónico para os utilizadores afetados ou grupos. Também envia notificações por e-mail aos administradores do recurso para se certificar de que o acesso adequado é mantido. Atribuições de poderão ser renovadas e permaneçam visíveis num estado expirado para 30 dias, mesmo se não for prolongado acesso.

## <a name="who-can-extend-and-renew"></a>Quem pode expandir e renovar?

Apenas os administradores do recurso podem expandir ou renovar atribuições de funções. O membro afetado pode pedir para expandir as funções que estão prestes a expirar e a pedido para renovar funções que já expiraram.

## <a name="when-are-notifications-sent"></a>Quando são enviadas notificações?

PIM envia notificações de correio eletrónico para administradores e os membros afetados das funções que estão prestes a expirar dentro de 14 dias e um dia antes da expiração. Quando uma atribuição oficialmente expira, envia uma mensagem de e-mail adicional. 

Os administradores recebem notificações quando um membro de uma função expirou ou expirando os pedidos para expandir ou renovar o. Quando um administrador específico resolve o pedido, todos os outros administradores são notificados a decisão de resolução (aprovados ou negados). Em seguida, o requerente membro é notificado da decisão. 

## <a name="extend-role-assignments"></a>Expandir atribuições de função

Os passos seguintes descrevem o processo para solicitar, resolver ou administrar uma extensão ou a renovação de uma atribuição de função. 

### <a name="member-extend"></a>Expandir o membro

Membros de uma atribuição de função podem expandir a expiração atribuições de funções diretamente a partir de **elegível** ou **Active Directory** separador no **as minhas funções** página de um recurso e de nível superior **As minhas funções** página do portal do PIM. Os membros podem pedir para expandir elegíveis e Active Directory (atribuídas) as funções que expiram na seguintes de 14 dias.

![Expandir funções](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Quando a atribuição data-hora de fim está dentro de 14 dias, o botão para **expanda** torna-se de uma ligação activa na interface de utilizador. No exemplo seguinte, assumir que a data atual for 27 de Março.

![O botão "Expanda"](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Para pedir uma extensão com esta atribuição de função, selecione **expanda** para abrir o formulário de pedido.

![Abrir o formulário de pedido](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Para ver informações sobre a atribuição de original, expanda **detalhes de atribuição**. Introduza um motivo para o pedido de extensão e, em seguida, selecione **expanda**.

>[!Note]
>Recomendamos, incluindo os detalhes do motivo pelo qual a extensão é necessária e para o período de tempo deve ser concedida a extensão (se tiver estas informações).

![Expandir a atribuição de função](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

De um fim de algum tempo, os administradores de recursos recebem uma notificação de correio eletrónico a solicitar que possam analisar o pedido de extensão. Se já foi submetido um pedido para expandir, é apresentada uma notificação de alerta na parte superior do portal do Azure explicar o erro.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Vá para o **pedidos pendentes** separador no painel da esquerda para ver o estado do seu pedido de ou para cancelar.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Aprovar Admin

Quando um membro submete um pedido para expandir uma atribuição de função, os administradores de recursos recebem uma notificação de correio eletrónico que contém os detalhes sobre a atribuição de original e o motivo para o pedido. A notificação inclui uma ligação direta para o pedido para o administrador aprovar ou negar. 

Além de utilizar seguintes na ligação de e-mail, os administradores podem aprovar ou recusar pedidos acedendo a administração PIM portal e selecionar **aprovar pedidos** no painel esquerdo.

![Captura de ecrã do erro](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Quando seleciona um administrador **aprovar** ou **negar**, os detalhes do pedido são apresentados, juntamente com um campo para fornecer a justificação para os registos de auditoria.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Quando aprovar um pedido para expandir a atribuição de função, os administradores de recursos podem escolher um novo data de início, a data de fim e o tipo de atribuição. Alterar o tipo de atribuição poderá ser necessário se o administrador pretende fornecer acesso limitado para concluir uma tarefa específica (um dia, por exemplo). Neste exemplo, o administrador pode alterar a atribuição de **elegível** para **Active Directory**. Isto significa que podem fornecer acesso para o requerente sem necessidade de ativar.

### <a name="admin-extend"></a>Expandir Admin

Se um membro de função se esquecer ou não é possível pedir uma extensão de associação de função, um administrador pode expandir uma atribuição em nome de membro. Extensões administrativas do membro da função necessita de aprovação, mas as notificações são enviadas para todos os outros administradores depois da função tiver sido expandida.

Para expandir um membro da função, navegue para a vista de função ou membro de recursos no PIM. Localize o membro que necessita de uma extensão. Em seguida, selecione **expanda** na coluna de ação.

![Expandir um membro da função](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Renovar as atribuições de função

Embora, conceptualmente, semelhante ao processo para pedir uma extensão, o processo para renovar uma atribuição de função expirada é diferente. Utilizar os seguintes passos, membros e os administradores podem renovar acesso às funções expiradas quando for necessário.

### <a name="member-renew"></a>Renovação do membro

Os membros que já não podem aceder a recursos podem aceder a 30 dias do histórico de atribuição expirado. Para tal, navegue para **funções My** no painel esquerdo e, em seguida, selecione o **expirado funções** separador na secção de funções de recursos do Azure.

![No separador "Funções expirado"](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

A lista de funções apresentadas por predefinição, **funções elegíveis**. Utilize o menu de lista pendente para alternar entre elegíveis e ativa a atribuição de funções.

Para pedir renovação de qualquer um das atribuições de função na lista, selecione o **renovar** ação. Em seguida, forneça um motivo para o pedido. É útil fornecer uma duração para além de qualquer contexto adicional que ajuda a decidir aprovar ou recusar o administrador de recursos.

![Renovar a atribuição de função](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

Depois do pedido foi submetido, os administradores de recursos são notificados de um pedido pendente para renovar uma atribuição de função.

### <a name="admin-approves"></a>Aprova Admin

Os administradores de recursos podem aceder a requisição de renovação da ligação a notificação por e-mail ou ao aceder ao PIM a partir do portal do Azure e selecionar **aprovar pedidos** no painel esquerdo.

![Aprovar pedidos](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Quando seleciona um administrador **aprovar** ou **negar**, os detalhes do pedido são apresentados juntamente com um campo para fornecer a justificação para os registos de auditoria.

![Aprovar a atribuição de função](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Quando aprovar um pedido de renovação de atribuição de função, os administradores de recursos tem de introduzir um novo data de início, a data de fim e o tipo de atribuição. 

### <a name="admin-renew"></a>Renovar Admin

Os administradores de recursos podem renovar atribuições de função expirada do **membros** separador no menu de navegação esquerdo de um recurso. Estes também podem renovar atribuições de funções expirados a partir do **expirado** separador de funções de uma função de recursos.

Para ver uma lista de todos os expirou atribuições de função, o **membros** ecrã, selecione **expirado funções**.

![Funções expiradas](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Passos Seguintes

[Exigir a aprovação para ativar](pim-resource-roles-approval-workflow.md)

[Ativar uma função](pim-resource-roles-use-the-audit-log.md)


