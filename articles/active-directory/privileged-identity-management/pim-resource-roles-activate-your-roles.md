---
title: Privileged Identity Management para recursos do Azure – ativar funções | Microsoft Docs
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
ms.openlocfilehash: 3e5456e7a632639cb82d7ba2b2e073938b1798ef
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---activate"></a>Ativar o Privileged Identity Management - funções dos recursos-
Ativar funções para recursos do Azure introduz uma nova experiência que permite que os membros da função elegível agendar ativação para uma data/hora no futuro e selecione uma duração de ativação específico no máximo (configurado por administradores). Saiba mais sobre [ativar aqui a funções do Azure AD](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Ativar funções
Navegue para a secção as minhas funções na barra de navegação esquerdo. Clique em "Ativar" para a função que pretende ativar para.
![](media/azure-pim-resource-rbac/rbac-roles.png)

No menu de ativações, introduza a data de início pretendida e a hora para ativar a função. Opcionalmente, reduzir a duração da ativação (o período de tempo a função está ativa) e a introdução de uma justificação se necessário. Clique em ativar.

A data de início e a hora é se não forem modificados, a função será ativada em segundos. Verá que uma função em fila para mensagem de faixa de ativação na página My funções. Clique no botão de atualização para limpar esta mensagem.

![](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Se a ativação estiver agendada para uma data futura hora, o pedido pendente aparecerá no separador de pedidos pendentes do menu de navegação esquerdo. Caso a ativação de função já não é necessária, o utilizador poderá cancelar o pedido ao clicar no botão Cancelar no lado direito da página.

![](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="just-enough-administration"></a>Administração just enough

A utilização de apenas suficiente melhores práticas de administração (JEA) com a suas atribuições de função de recursos é simple com PIM para recursos do Azure. Os utilizadores e os membros do grupo com atribuições de subscrições do Azure ou os grupos de recursos, podem ativar a atribuição de função existente num âmbito reduzida. 

A página de pesquisa, localize o recurso subordinado que tem de gerir.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecione as minhas funções no menu de navegação esquerdo e escolha a função adequada para o ativar. Tenha em atenção o tipo de atribuição é herdada, uma vez que a função foi atribuída à subscrição, em vez do grupo de recursos, conforme mostrado abaixo.

![](media/azure-pim-resource-rbac/my-roles-02.png)
