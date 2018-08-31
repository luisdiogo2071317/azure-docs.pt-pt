---
title: Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM | Documentos da Microsoft
description: Saiba como aprovar ou recusar pedidos para funções de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7bf1e437e97fdb4d929af23bd7b2a9abb49268df
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189163"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM

Privileged Identity Management, pode configurar funções para exigir a aprovação para a ativação e escolher um ou vários utilizadores ou grupos como aprovadores delegados.

## <a name="view-pending-approvals-requests"></a>Ver pendentes aprovações (pedidos)

Como um aprovador de delegados, receberá notificações por e-mail quando um pedido está com aprovação pendente. Para ver estes pedidos no portal do PIM, a partir do dashboard (na navegação do novo) selecione o separador de "Pedidos de aprovação pendentes" na barra de navegação esquerdo.

![](media/azure-ad-pim-approval-workflow/image023.png)

A partir daí, verá uma lista de pedidos com aprovação pendente:

![](media/azure-ad-pim-approval-workflow/image024.png)

## <a name="approve-or-deny-requests-for-role-elevation-single-andor-bulk"></a>Aprovar ou negar pedidos de elevação de função (único e/ou em massa)

Seleccione os pedidos que pretende aprovar ou negar e clique no botão na barra de ação que corresponde à sua decisão:

![](media/azure-ad-pim-approval-workflow/image025.png)

## <a name="provide-justification-for-my-approvaldenial"></a>Fornecer a justificação para meu aprovação/negação

Esta ação irá abrir um novo painel para aprovar ou negar pedidos de várias ao mesmo tempo. Introduza uma justificativa para a sua decisão, e clique em aprovar (ou negar) na parte inferior ou o painel:

![](media/azure-ad-pim-approval-workflow/image029.png)

Quando o processo de pedido for concluído, o símbolo de estado irá refletir a decisão que tiver feito (neste exemplo, a decisão é aprovar):

![](media/azure-ad-pim-approval-workflow/image031.png)

## <a name="next-steps"></a>Passos Seguintes

- [Aprovar ou recusar pedidos para funções de recursos do Azure no PIM](pim-resource-roles-approval-workflow.md)
- [Notificações por e-mail no PIM](pim-email-notifications.md)
