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
ms.subservice: pim
ms.date: 08/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1c4b3819494ecb4c0897edeb08e2ce45d867b626
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55179077"
---
# <a name="approve-or-deny-requests-for-azure-ad-directory-roles-in-pim"></a>Aprovar ou recusar pedidos para funções de diretório do Azure AD no PIM

Com o Azure AD Privileged Identity Management (PIM), pode configurar funções para exigir a aprovação para a ativação e escolher um ou vários utilizadores ou grupos como aprovadores delegados. Siga os passos neste artigo para aprovar ou recusar pedidos para funções de diretório do Azure AD.

## <a name="view-pending-requests"></a>Ver pedidos pendentes

Como um aprovador de delegados, receberá uma notificação por e-mail quando uma solicitação de função de diretório do Azure AD está com aprovação pendente. Pode ver estes pedidos pendentes no PIM.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **aprovar pedidos**.

    ![Funções de diretório do Azure de PIM AD - funções](./media/azure-ad-pim-approval-workflow/pim-directory-roles-approve-requests.png)

    Verá uma lista de pedidos com aprovação pendente.

## <a name="approve-requests"></a>Aprovar pedidos

1. Seleccione os pedidos que pretende aprovar e, em seguida, clique em **aprovar** abrir a aprovar selecionado no painel de pedidos.

    ![Lista de pedidos de aprovação do PIM](./media/azure-ad-pim-approval-workflow/pim-approve-requests-list.png)

1. Na **aprovar razão** , escreva um motivo.

    ![Pedidos de PIM aprovar selecionados](./media/azure-ad-pim-approval-workflow/pim-approve-selected-requests.png)

1. Clique em **aprovar**.

    Símbolo de estado será atualizado com sua aprovação.

    ![Pedidos de PIM aprovar selecionados](./media/azure-ad-pim-approval-workflow/pim-approve-status.png)

## <a name="deny-requests"></a>Negar pedidos

1. Seleccione os pedidos que pretende negar e, em seguida, clique em **negar** abrir a negar selecionado no painel de pedidos.

    ![Lista de pedidos de aprovação do PIM](./media/azure-ad-pim-approval-workflow/pim-deny-requests-list.png)

1. Na **negar razão** , escreva um motivo.

    ![Pedidos de PIM negar selecionados](./media/azure-ad-pim-approval-workflow/pim-deny-selected-requests.png)

1. Clique em **negar**.

    Símbolo de estado será atualizado com sua negação.

## <a name="next-steps"></a>Passos Seguintes

- [Notificações por e-mail no PIM](pim-email-notifications.md)
- [Aprovar ou recusar pedidos para funções de recursos do Azure no PIM](pim-resource-roles-approval-workflow.md)
