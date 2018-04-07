---
title: Privileged Identity Management - fluxo de trabalho de aprovação para funções de recursos do Azure | Microsoft Docs
description: Descreve como processar o fluxo de trabalho de aprovação para recursos do Azure.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c02d595d75b2d63558896054c185102ebb23cc9e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---approve"></a>Aprovar Privileged Identity Management - funções dos recursos-

Com aprovação fluxo de trabalho no PIM para funções de recursos do Azure, os administradores podem ainda mais proteger ou restringir o acesso a recursos críticos ao pedir aprovação para ativar as atribuições de funções. Exclusivo de funções de recursos do Azure é o conceito de uma hierarquia de recursos. Esta hierarquia permite que a herança de atribuições de função de um objeto de recurso principal rapidez para todos os recursos subordinados subordinada no contentor principal. 

Por exemplo Bernardo, um administrador de recursos utiliza PIM atribuir Alice como membro elegível para a função de proprietário na subscrição Contoso. Com esta atribuição Alice também é um proprietário elegível de todos os contentores do grupo de recursos dentro da subscrição da Contoso e todos os recursos (como máquinas virtuais) contidos em cada grupo de recursos, da subscrição. Vamos supor que existem três grupos de recursos na subscrição Contoso; Teste Fabrikam, Fabrikam Dev e Fabrikam Prod. Cada um destes grupos de recursos contém uma única máquina virtual.

Definições do PIM estão configuradas para cada função de um recurso e ao contrário das atribuições estas definições não são herdadas e aplicam-se estritamente para a função de recursos. [Leia mais sobre atribuições de elegíveis e visibilidade do recurso.](pim-resource-roles-eligible-visibility.md)

Utilizando o exemplo acima João utiliza o PIM para exigir que todos os membros na função de proprietário de aprovação de pedidos de subscrição Contoso ativar. Para proteger os recursos contidos no grupo de recursos de Fabrikam Prod, o João requer aprovação de membros da função de proprietário deste recurso também. As funções de proprietário na Fabrikam teste e desenvolvimento Fabrikam não necessitam de aprovação para ativar.

Quando ativação de pedidos de Alice da respetiva função de proprietário da subscrição de Contoso aprovador tem aprovar ou negar o pedido para ela fica ativa na função de. Além disso, se Alice decide [definir o âmbito do respetiva ativação](pim-resource-roles-activate-your-roles.md#just-enough-administration) para o grupo de recursos Fabrikam Prod aprovador tem aprovar ou negar este pedido demasiado. No entanto, se decidir Alice definir o âmbito do respetiva ativação para um ou ambos os Fabrikam teste ou desenvolvimento da Fabrikam, não será necessária aprovação.

Fluxo de trabalho de aprovação não pode ser necessário para todos os membros de uma função. Considere um cenário em que a sua organização hires várias associa contrato para ajudá-lo no desenvolvimento de uma aplicação que será executada uma subscrição do Azure. Como um administrador de recursos, gostaria que os funcionários têm acesso elegível sem aprovação necessária, mas associa o contrato tem de solicitar aprovação. Para configurar fluxo de trabalho de aprovação para apenas o contrato associa pode criar uma função personalizada com as mesmas permissões que a função atribuído aos funcionários e exigir a aprovação para ativar essa função personalizada. [Saiba mais sobre as funções personalizadas](pim-resource-roles-custom-role-policy.md).

Siga os passos abaixo para configurar o fluxo de trabalho de aprovação e especificar quem pode aprovar ou recusar pedidos.

## <a name="require-approval-to-activate"></a>Exigir aprovação para ativar

Navegue para PIM no portal do Azure e selecione um recurso na lista.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

No menu de navegação esquerdo, selecione **definições da função**.

Procure e selecione uma função e clique em **editar** para modificar as definições.

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

Na ativação da caixa de verificação de secção **exigir a aprovação para ativar**.

![](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Especifique aprovadores

Clique em **selecione aprovadores** para abrir o ecrã de seleção.

>[!NOTE]
>Tem de selecionar, pelo menos, um utilizador ou grupo para atualizar a definição. Não existem aprovadores não predefinido.

Os administradores de recursos podem adicionar qualquer combinação dos utilizadores e grupos à lista de aprovadores. 

![](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Solicitar aprovação para ativar

Solicitar a aprovação não tem impacto sobre o procedimento membro tem de seguir para ativar. [Reveja os passos para ativar uma função](pim-resource-roles-activate-your-roles.md).

Se um membro pedido de ativação de uma função que requeira a aprovação e a função já não é necessária, o membro pode cancelar o pedido no PIM.

Para cancelar, navegue para o PIM e selecione "Os meus pedidos de". Localize o pedido e clique em "Cancelar".

![](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Aprovar ou negar um pedido

Para aprovar ou negar um pedido tem de ser um membro da lista aprovador. No PIM, selecione "Aprovar pedidos de" a partir do separador no menu de navegação esquerdo e localize o pedido.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

Selecione o pedido, forneça uma justificação para a decisão e selecione aprovar ou recusar, resolver o pedido.

![](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Notificações de fluxo de trabalho

- Todos os membros da lista aprovador são notificados por e-mail quando um pedido para uma função está a aguardar a respetiva revisão. Notificações por e-mail incluem uma ligação direta para o pedido onde o aprovador pode aprovar ou negar.
- Pedidos são resolvidos pelo primeiro membro da lista que aprova ou nega. 
- Quando um aprovador responde ao pedido, todos os membros da lista aprovador são notificados da ação. 
- Os administradores de recursos são notificados quando um membro aprovado fica ativo na respetiva função. 

>[!Note]
>Um caso em que um administrador de recursos Deteta que os membros aprovados não devem estar ativo, que podem remover a atribuição de função de Active Directory no PIM. Embora os administradores de recursos não são notificados de pedidos pendentes, a menos que são membros da lista aprovador, pode ver e cancelar a pedidos de todos os utilizadores pendentes visualizando pedidos pendentes nos PIM. 

## <a name="next-steps"></a>Passos Seguintes

[Aplicar definições de PIM em exclusivos grupos de utilizadores](pim-resource-roles-custom-role-policy.md)
