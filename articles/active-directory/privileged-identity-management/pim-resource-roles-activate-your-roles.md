---
title: Ativar as funções para recursos do Azure com o Privileged Identity Management | Documentos da Microsoft
description: Descreve como ativar as funções no PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d2f61f1ebdd473a24115c7774801f5b7694f224f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443210"
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Ativar as funções para recursos do Azure com o Privileged Identity Management
Privileged Identity Management (PIM) introduz uma nova experiência na ativação de funções de recursos do Azure. Os membros da função elegível, podem agendar ativação numa data futura e a hora. Também podem selecionar uma duração de ativação específico dentro do máximo (configurado por administradores). Para obter mais informações, consulte [como ativar ou desativar funções no Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Ativar as funções
Navegue para o **minhas funções** secção no painel esquerdo. Selecione **Activate** para a função que pretende ativar.

![Separador "Funções elegíveis" o painel de "As minhas funções".](media/azure-pim-resource-rbac/rbac-roles.png)

Partir do **ativações** menu, introduza a data de início e a hora de ativar a função. Opcionalmente, diminuir a duração de ativação (o período de tempo que a função está ativa) e introduza uma justificativa, se necessário. Em seguida, selecione **Activate**.

Se a data de início e a hora não são modificados, a função é ativada em segundos. Na **minhas funções** painel, uma mensagem de faixa mostra que uma função é colocada na fila para ativação. Selecione o botão de atualização para limpar esta mensagem.

![Painel "As minhas funções" com uma mensagem de faixa e uma notificação sobre uma aprovação pendente](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Se a ativação está agendada para uma data futura e a hora, a solicitação pendente é apresentada no **pedidos pendentes** separador do painel esquerdo. Se a ativação de função já não for necessária, pode cancelar o pedido, selecionando o **Cancelar** botão.

![Lista de pedidos com botões "Cancelar" pendentes](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Aplicar práticas de administração apenas suficiente

Utilizando as melhores práticas de Just Enough Administration (JEA) com as atribuições de função de recursos é simples com o PIM para recursos do Azure. Os utilizadores e os membros do grupo com atribuições de subscrições do Azure ou grupos de recursos podem ativar a sua atribuição de função existentes num âmbito reduzida. 

Na página de pesquisa, localize o recurso subordinado que precisa de gerir.

![Selecionar um recurso](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecione **minhas funções** no painel à esquerda e escolha a função adequada para ativar. É o tipo de atribuição **herdado** porque a função foi atribuída à subscrição, em vez de no grupo de recursos.

![Lista de atribuições de funções elegíveis, com o tipo de atribuição realçado](media/azure-pim-resource-rbac/my-roles-02.png)
