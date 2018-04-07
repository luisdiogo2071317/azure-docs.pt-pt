---
title: Privileged Identity Management para recursos do Azure - expandir e renovar funções | Microsoft Docs
description: Este documento descreve como expandir e renovar funções dos recursos do Azure para os recursos do PIM.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2547b3793688eb51a4114f30bfcf61a9402f2cd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---extend-or-renew"></a>Privileged Identity Management - funções dos recursos - expandir ou renovar

PIM para recursos do Azure introduz novos controlos para gerir o ciclo de vida de acesso e a atribuição de recursos do Azure. Os administradores podem atribuir associação utilizando propriedades de data / hora de início e de fim. Quando se aproxima do fim de atribuição, PIM envia notificações de correio eletrónico para os membros afetados (pode ser um utilizador ou grupo) e para os administradores do recurso para garantir que é mantido acesso adequado. No caso de acesso não for prolongado devido a inaction, atribuições poderão ser renovadas e permaneçam visíveis num estado expirado para 30 dias.

## <a name="who-can-extend-and-renew"></a>Quem pode expandir e renovar?

Apenas os administradores do recurso podem expandir ou renovar atribuições de funções. O membro afetado pode pedir para expandir as funções prestes a expirar e renovação de pedido de funções já expirou.

## <a name="when-are-notifications-sent"></a>Quando são enviadas notificações?

PIM envia notificações de correio eletrónico para administradores e os membros afetados das funções expira dentro de 14 dias e um dia antes da expiração. Uma mensagem de e-mail adicional é enviada quando uma atribuição oficialmente expirou. 

Os administradores recebem notificações quando um membro de uma função expirou ou expirando os pedidos para expandir ou renovar o. Quando um administrador específico resolve o pedido de todos os outros administradores são notificados a decisão de resolução (aprovados ou negados) e o membro requerente é notificado a decisão. 

## <a name="extend-role-assignments"></a>Expandir atribuições de função

Os passos abaixo descrevem os passos e a interface de utilizador envolvido na pedir, resolver ou administrar uma extensão ou a renovação de uma atribuição de função. 

### <a name="member-extend"></a>Expandir o membro

Os membros de uma atribuição de função podem pedir para expandir a expiração atribuições de funções diretamente a partir de "Elegíveis" ou "Ativo" separador no "As minhas funções" página de um recurso e de nível superior as minhas funções do portal do PIM. Os membros podem pedir para expandir elegíveis e Active Directory (atribuídas) as funções que expiram na seguintes de 14 dias.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Quando a atribuição data-hora de fim está dentro de 14 dias, o botão para "Expanda" irá tornar-se uma ligação activa na interface de utilizador. No exemplo abaixo, assumir que a data atual for 27 de Março.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Para pedir uma extensão desta função de atribuição, clique em "Expanda" para abrir o formulário de pedido.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Expanda os detalhes"atribuição" para ver informações sobre a atribuição de original. Introduza um motivo para o pedido de extensão e clique em "Expanda".

>[!Note]
>Recomendamos, incluindo os detalhes do motivo pelo qual a extensão é necessária, e para quanto a extensão deve ser (se for conhecido).

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Num fim de algum tempo, os administradores de recursos irão receber uma notificação de correio eletrónico a solicitar que reveja o pedido de extensão. Se já foi submetido um pedido para expandir, será apresentada uma notificação de alerta na parte superior do portal do Azure explicar o erro.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Visite o separador "pendente pedidos" no menu de navegação à esquerda para ver o estado dos ou cancelar o pedido.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Aprovar Admin

Quando um membro submete um pedido para expandir uma atribuição de função, os administradores de recursos recebem uma notificação de correio eletrónico que contém os detalhes de atribuição de original e o motivo fornecidas pelo requerente. A notificação inclui uma ligação direta para o pedido para o administrador aprovar ou negar. 

Para além das seguintes na ligação de e-mail, os administradores podem aprovar ou negar pedidos de navegar para o portal de administração do PIM e selecionando "Aprovar pedidos" no menu de navegação esquerdo.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Quando um administrador seleciona aprovar ou negar os detalhes do pedido são apresentados juntamente com um campo para fornecer a justificação para os registos de auditoria.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Quando aprovar um pedido para expandir a atribuição de função, os administradores de recursos podem escolher um novo início e de fim data / hora e a atribuição de tipo. Alterar o tipo de atribuição poderá ser necessário se o administrador pretende fornecer acesso limitado para concluir uma tarefa específica (um dia, por exemplo). Neste exemplo, o administrador pode alterar a atribuição de elegível para o Active Directory de fornecer o acesso de requerente sem necessidade de ativar.

### <a name="admin-extend"></a>Expandir o Admin

Em caso de um membro de função se esquecer ou não é possível pedir a extensão de associação de função de um administrador pode expandir uma atribuição em nome de membro. Extensões administrativas do membro da função necessita de aprovação, mas as notificações são enviadas para todos os outros administradores após a conclusão de expandir a função.

Para expandir a uma função de associação, navegue para a vista de função ou membro de recurso no PIM. Localizar o membro que requerem uma extensão e clique em "Expanda" na coluna de ação.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Renovar as atribuições de função

Enquanto, conceptualmente, semelhante, o processo para renovar uma atribuição de função expirada é diferente de pedir uma extensão para membros e administradores. Utilizando os passos abaixo membros e os administradores pode renovar o acesso às funções expiradas quando for necessário.

### <a name="member-renew"></a>Renovar membro

Os membros que já não podem aceder a recursos podem aceder a 30 dias do histórico de atribuição expirada-lo acedendo a minha funções na navegação à esquerda do PIM e selecionando o separador "Expirado funções" na secção de funções de recursos do Azure.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

A lista de funções apresentadas por predefinição, as atribuições elegíveis. Utilize a lista pendente para alternar entre elegíveis e ativa a atribuição de funções.

Ao pedido de renovação para qualquer função de atribuições na lista de selecionar a ação "Renovar" e indicar um motivo para o pedido. É útil fornecer uma duração para além de qualquer contexto adicional que o irão ajudar o administrador de recursos decidir aprovar ou negar.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

Após a submissão do pedido, os administradores de recursos são notificados de um pedido pendente para renovar uma atribuição de função.

### <a name="admin-approves"></a>Aprova Admin

Os administradores de recursos podem aceder a requisição de renovação da ligação a notificação por e-mail ou ao aceder a partir do portal do Auzre PIM e selecionando "Aprovar pedidos" no menu de navegação esquerdo.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Quando um administrador seleciona aprovar ou negar os detalhes do pedido são apresentados juntamente com um campo para fornecer a justificação para os registos de auditoria.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Aprovar um pedido de renovação de atribuição de função, os administradores de recursos tem de introduzir um novo início e de fim data / hora e a atribuição de tipo. 

### <a name="admin-renew"></a>Renovar Admin

Os administradores de recursos podem renovar atribuições de funções expirada partir do separador Membros no menu de navegação esquerdo de um recurso, ou a partir do separador de funções expirado de uma função de recursos.

No ecrã membros Selecione funções de expirado para ver uma lista de todas as atribuições de função expirado.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Passos Seguintes

[Exigir a aprovação para ativar](pim-resource-roles-approval-workflow.md)

[Ativar uma função](pim-resource-roles-use-the-audit-log.md)


