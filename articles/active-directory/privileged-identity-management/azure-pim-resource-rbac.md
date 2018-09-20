---
title: Ver quem tem funções de recursos do Azure no PIM | Documentos da Microsoft
description: Ver quem tem funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: ce7c96d92938c4e3b4cc0b53271df48350083754
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465236"
---
# <a name="view-who-has-azure-resource-roles-in-pim"></a>Ver quem tem funções de recursos do Azure no PIM

Com o Azure Active Directory Privileged Identity Management (PIM), pode gerir, controlar e monitorizar o acesso aos recursos do Azure na sua organização. Isto inclui as subscrições, grupos de recursos e até mesmo máquinas de virtuais. Qualquer recurso no portal do Azure, que aproveita a funcionalidade de controlo (RBAC) de acesso baseado em função do Azure pode aproveitar a segurança e o ciclo de vida das capacidades de gestão no Azure AD PIM. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM para recursos do Azure ajuda os administradores de recursos

- Veja os utilizadores e grupos são atribuídos a funções para os recursos do Azure que administra
- Ativar a pedido, "just-in-time" acesso para gerir os recursos, tais como as subscrições, grupos de recursos e muito mais
- Expirar acesso aos recursos de utilizadores/grupos atribuídos automaticamente com as novas definições de atribuição de limite de tempo
- Atribuir acesso a recursos temporário para tarefas rápidas ou na chamada agendas
- Impor o multi-factor Authentication para acesso a recursos em qualquer função interna ou personalizada 
- Obter relatórios sobre a atividade de recursos de recursos de acesso correlacionado durante uma sessão de utilizador Active Directory
- Obtenha alertas quando os novos utilizadores ou grupos são atribuídos acesso a recursos e quando ativam atribuições elegíveis

## <a name="view-activation-and-azure-resource-activity"></a>Ativação do modo de exibição e a atividade de recursos do Azure

No caso de precisar de ver as ações realizou de um utilizador específico em vários recursos, pode rever a atividade de recursos do Azure associada a um período de ativação de determinado (para os utilizadores elegíveis). Comece por selecionar um utilizador, da vista de membros ou na lista de membros numa função específica. O resultado mostra uma visão gráfica das ações do usuário em recursos do Azure por data e as ativações de função recente no mesmo período de tempo.

![](media/azure-pim-resource-rbac/user-details.png)

Selecionar uma ativação de função específica irá mostrar os detalhes de ativação de função e atividade de recursos do Azure correspondente que ocorreu enquanto que o utilizador esteve ativo.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="review-who-has-access-in-a-subscription"></a>Revisão de quem tem acesso uma subscrição

Para rever atribuições de funções na sua subscrição, selecione o separador de membros no painel de navegação esquerda, ou selecionar funções e escolha uma função específica para rever os membros. 

Selecione a revisão da barra de ação para ver as revisões de acesso existente e selecione Add para criar uma nova revisão.

![](media/azure-pim-resource-rbac/owner.png)

[Saiba mais sobre as revisões de acesso](pim-how-to-perform-security-review.md)

>[!NOTE]
Revisões só são suportadas para tipos de recursos de subscrição neste momento.

## <a name="next-steps"></a>Passos Seguintes

- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
- [Aprovar ou recusar pedidos para funções de recursos do Azure no PIM](pim-resource-roles-approval-workflow.md)
- [Funções incorporadas no Azure](../../role-based-access-control/built-in-roles.md)
