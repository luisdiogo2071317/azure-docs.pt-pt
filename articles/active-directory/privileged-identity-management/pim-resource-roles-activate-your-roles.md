---
title: Ativar funções para recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Descreve como ativar funções no PIM.
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
ms.openlocfilehash: a985e67cc566cc45b3ee6b8dc98e91a8f34abd1b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Ativar funções para recursos do Azure utilizando o Privileged Identity Management
Privileged Identity Management (PIM) apresenta uma nova experiência na ativação de funções para recursos do Azure. Membros da função elegível, podem agendar ativação para uma data futura e hora. Também pode selecionar uma duração de ativação específico no máximo (configurado por administradores). Para obter mais informações, consulte [como ativar ou desativar as funções no Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Ativar funções
Navegue para o **as minhas funções** secção no painel esquerdo. Selecione **ativar** para a função que pretende ativar.

![Separador "Elegíveis funções" do painel de "As minhas funções".](media/azure-pim-resource-rbac/rbac-roles.png)

Do **ativações** menu, introduza a data de início e a hora para ativar a função. Opcionalmente, reduzir a duração da ativação (o período de tempo que a função está ativa) e a introdução de uma justificação se necessário. Em seguida, selecione **ativar**.

Se a data de início e a hora não forem modificados, a função está ativada em segundos. No **as minhas funções** painel, uma mensagem de faixa mostra que uma função é colocada na fila para a ativação. Selecione o botão de atualização para limpar esta mensagem.

![Painel "As minhas funções" com uma mensagem de faixa e uma notificação sobre uma aprovação pendente](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Se a ativação estiver agendada para uma data futura e a hora, aparece o pedido pendente de **pedidos pendentes** separador do painel esquerdo. Se a ativação de função já não é necessária, pode cancelar o pedido, selecionando o **Cancelar** botão.

![Lista de pedidos com "Cancelar" botões pendentes](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Aplicam-se apenas suficiente práticas de administração

A utilização de melhores práticas de apenas suficiente administração (JEA) com a suas atribuições de função de recursos é simple com PIM para recursos do Azure. Os utilizadores e os membros do grupo com atribuições de subscrições do Azure ou os grupos de recursos, podem ativar a atribuição de função existente num âmbito reduzida. 

A página de pesquisa, localize o recurso subordinado que precisa de gerir.

![Selecionar um recurso](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecione **as minhas funções** no painel esquerdo e escolha a função adequada para o ativar. O tipo de atribuição é **herdado** porque a função foi atribuída na subscrição, em vez de no grupo de recursos.

![Lista de atribuições de funções elegíveis, com o tipo de atribuição realçado](media/azure-pim-resource-rbac/my-roles-02.png)
