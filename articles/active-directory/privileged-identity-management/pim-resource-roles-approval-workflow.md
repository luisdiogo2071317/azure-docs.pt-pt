---
title: Aprovar ou recusar pedidos para funções de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como aprovar ou recusar pedidos para funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e5cda31af5ac95e7ebe2b858b1d7355ea3f2a6bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189809"
---
# <a name="approve-or-deny-requests-for-azure-resource-roles-in-pim"></a>Aprovar ou recusar pedidos para funções de recursos do Azure no PIM

Para aprovar ou recusar um pedido, tem de ser um membro da lista de aprovador. 

1. No PIM, selecione **aprovar pedidos** do separador no menu da esquerda e localize o pedido.

   ![Painel "Aprovar pedidos de"](media/azure-pim-resource-rbac/aadpim_rbac_approve_requests_list.png)

2. Selecione o pedido, forneça uma justificativa para a decisão e selecione **aprovar** ou **negar**. O pedido, em seguida, for resolvido.

   ![Pedido seleccionado com informações detalhadas](media/azure-pim-resource-rbac/aadpim_rbac_approve_request_approved.png)

## <a name="workflow-notifications"></a>Notificações do fluxo de trabalho

Aqui estão alguns fatos sobre notificações de fluxo de trabalho:

- Todos os membros da lista aprovador notificados por e-mail quando um pedido para uma função está a aguardar a revisão. As notificações de e-mail incluem uma ligação direta para o pedido, em que o aprovador pode aprovar ou negar.
- Pedidos são resolvidos pelo primeiro membro da lista que aprova ou nega. 
- Quando um aprovador responde à solicitação, todos os membros da lista aprovador são notificados da ação. 
- Os administradores de recursos são notificados quando um membro aprovado se torna ativo na sua função. 

>[!Note]
>Um administrador de recursos que acredita que um membro aprovado não deve estar ativo pode remover a atribuição de função ativa no PIM. Embora os administradores de recursos não são notificados de pedidos pendentes, a menos que são membros da lista aprovador, pode ver e cancelar solicitações de todos os utilizadores pendentes visualizando pedidos no PIM pendentes. 

## <a name="next-steps"></a>Passos Seguintes

- [Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM](azure-ad-pim-approval-workflow.md)
- [Notificações por e-mail no PIM](pim-email-notifications.md)
