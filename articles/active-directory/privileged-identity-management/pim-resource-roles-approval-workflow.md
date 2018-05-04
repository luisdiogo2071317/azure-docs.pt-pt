---
title: Fluxo de trabalho de aprovação para funções de recursos do Azure no Privileged Identity Management | Microsoft Docs
description: Descreve o processo de fluxo de trabalho de aprovação para recursos do Azure.
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
ms.openlocfilehash: 7781c858a5c0e4db8593df0cf77b868b6fd23622
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="approval-workflow-for-azure-resource-roles-in-privileged-identity-management"></a>Fluxo de trabalho de aprovação para funções de recursos do Azure no Privileged Identity Management

Com aprovação fluxo de trabalho no Privileged Identity Management (PIM) para funções de recursos do Azure, os administradores podem ainda mais proteger ou restringir o acesso a recursos críticos. Ou seja, os administradores podem exigir a aprovação para ativar as atribuições de funções. 

O conceito de uma hierarquia de recurso é exclusivo para funções de recursos do Azure. Esta hierarquia permite que a herança de atribuições de função de um objeto de recurso principal rapidez para todos os recursos subordinados no contentor principal. 

Por exemplo: Bernardo, um administrador de recursos, utiliza o PIM atribuir Alice como membro elegível para a função de proprietário na subscrição Contoso. Com esta atribuição Alice é um proprietário de todos os contentores do grupo de recursos dentro da subscrição de Contoso elegível. Alice também é um proprietário de todos os recursos (como máquinas virtuais) elegível dentro de cada grupo de recursos da subscrição. 

Vamos supor que existem três grupos de recursos na subscrição Contoso: teste Fabrikam, Fabrikam Dev e Fabrikam Prod. Cada um destes grupos de recursos contém uma única máquina virtual.

Definições do PIM estão configuradas para cada função de um recurso. Ao contrário das atribuições, estas definições não são herdadas e aplicam-se estritamente para a função de recursos. [Saiba mais sobre atribuições elegíveis e visibilidade do recurso](pim-resource-roles-eligible-visibility.md).

Continuar com o exemplo: João utiliza o PIM para exigir que todos os membros na função de proprietário de aprovação de pedidos de subscrição Contoso ser ativado. Para ajudar a proteger os recursos no grupo de recursos de Fabrikam Prod, o João também requer aprovação para membros da função de proprietário deste recurso. As funções de proprietário na Fabrikam teste e desenvolvimento Fabrikam não necessitam de aprovação para a ativação.

Quando Alice pedidos de ativação da respetiva função de proprietário da subscrição de Contoso, um aprovador tem de aprovar ou negar o pedido para ela fica ativa na função de. Se decidir Alice [definir o âmbito do respetiva ativação](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) ao grupo de recursos Fabrikam Prod, o aprovador tem aprovar ou negar este pedido, demasiado. Mas se decidir Alice definir o âmbito do respetiva ativação para um ou ambos os Fabrikam teste ou desenvolvimento da Fabrikam, aprovação não é necessária.

O fluxo de trabalho de aprovação não poderá ser necessário para todos os membros de uma função. Considere um cenário em que a sua organização hires várias associa contrato para o ajudar com o desenvolvimento de uma aplicação que será executada uma subscrição do Azure. Como um administrador de recursos que pretende que os funcionários têm acesso elegível sem aprovação necessária, mas associa o contrato tem de solicitar aprovação. Para configurar o fluxo de trabalho de aprovação para apenas o contrato associa, pode criar uma função personalizada com as mesmas permissões que a função atribuído aos funcionários. Pode exigir a aprovação para ativar essa função personalizada. [Saiba mais sobre as funções personalizadas](pim-resource-roles-custom-role-policy.md).

Para configurar o fluxo de trabalho de aprovação e especificar quem pode aprovar ou recusar pedidos, utilize os procedimentos seguintes.

## <a name="require-approval-to-activate"></a>Exigir aprovação para ativar

1. Navegue até à PIM no portal do Azure e selecione um recurso na lista.

   ![Painel "Recursos do azure" com um recurso selecionado](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. No painel esquerdo, selecione **definições da função**.

3. Procure e selecione uma função e, em seguida, selecione **editar** para modificar as definições.

   ![Botão "Editar" para a função de operador](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. No **ativação** secção, selecione o **exigir a aprovação para ativar** caixa de verificação.

   ![Secção de "Ativação" das definições de função](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Especifique aprovadores

Clique em **selecione aprovadores** para abrir o **selecionar um utilizador ou grupo** painel.

>[!NOTE]
>Tem de selecionar, pelo menos, um utilizador ou grupo para atualizar a definição. Não existem aprovadores não predefinido.

Os administradores de recursos podem adicionar qualquer combinação dos utilizadores e grupos à lista de aprovadores. 

![Painel "Selecionar um utilizador ou grupo" com um utilizador selecionado](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Solicitar aprovação para ativar

Solicitar aprovação não tem impacto sobre o procedimento que um membro tem de seguir para ativar. [Reveja os passos para ativar uma função](pim-resource-roles-activate-your-roles.md).

Se um membro pedido de ativação de uma função que requeira a aprovação e a função já não é necessária, o membro pode cancelar o pedido no PIM.

Para cancelar, navegue até à PIM e selecione **os meus pedidos**. Localize o pedido e selecione **Cancelar**.

![Painel "Os meus pedidos"](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="approve-or-deny-a-request"></a>Aprovar ou negar um pedido

Para aprovar ou negar um pedido, tem de ser um membro da lista aprovador. 

1. No PIM, selecione **aprovar pedidos** partir do separador no menu da esquerda e localize o pedido.

   ![Painel "Aprovar pedidos"](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Selecione o pedido, forneça uma justificação para a decisão e selecione **aprovar** ou **negar**. O pedido, em seguida, for resolvido.

   ![Pedido seleccionado com informações detalhadas](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Notificações de fluxo de trabalho

Eis alguns factos sobre notificações de fluxo de trabalho:

- Todos os membros da lista aprovador são notificados por e-mail quando um pedido para uma função está a aguardar a respetiva revisão. Notificações por e-mail incluem uma ligação direta para o pedido, onde o aprovador pode aprovar ou negar.
- Pedidos são resolvidos pelo primeiro membro da lista de utilizadores que aprova ou nega. 
- Quando um aprovador responde ao pedido, todos os membros da lista aprovador são notificados da ação. 
- Os administradores de recursos são notificados quando um membro aprovado fica ativo na respetiva função. 

>[!Note]
>Um administrador de recursos que Deteta que um membro aprovado não deve estar ativo pode remover a atribuição de função de Active Directory no PIM. Embora os administradores de recursos não são notificados de pedidos pendentes, a menos que são membros da lista aprovador, pode ver e cancelar a pedidos de todos os utilizadores pendentes visualizando pedidos pendentes nos PIM. 

## <a name="next-steps"></a>Passos Seguintes

[Aplicar definições de PIM em exclusivos grupos de utilizadores](pim-resource-roles-custom-role-policy.md)
