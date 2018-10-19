---
title: Gerir o acesso a faturação a utilização de funções do Azure | Documentos da Microsoft
description: ''
services: ''
documentationcenter: ''
author: vikramdesai01
manager: vikdesai
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/22/2017
ms.author: cwatson
ms.openlocfilehash: 38cfd354f11ef3d888ad70e71549868d398495f5
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429648"
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a>Gerir o acesso a informações de faturação para o Azure com o controlo de acesso baseado em funções

Pode conceder acesso a informações de faturação do Azure para membros de sua equipe atribuindo uma das seguintes funções de utilizador à sua subscrição: administrador de conta, administrador de serviços, coadministrador, proprietário, Contribuidor, leitor e leitor de faturação. Teriam acesso às informações de faturas no [portal do Azure](https://portal.azure.com/), e eles podem usar o [APIs de faturação](billing-usage-rate-card-overview.md) para obter programaticamente notas fiscais (uma vez optado por participar) e detalhes de utilização. Para obter mais informações sobre quem pode conceder funções e que funções podem fazer o que, consulte [funções no Azure RBAC](../role-based-access-control/built-in-roles.md).

## <a name="opt-in"></a> Permitir que os utilizadores adicionais aceder a notas fiscais

O administrador de conta tem de participar no utilizando o [portal do Azure](https://portal.azure.com/) permitir o acesso às notas fiscais para outros utilizadores e através da API.

1. Como administrador de conta, selecione a sua subscrição do [painel de subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure.

1. Selecione **notas fiscais** e, em seguida **acesso às notas fiscais**.

    ![Captura de ecrã mostra como delegar o acesso às notas fiscais](./media/billing-manage-access/AA-optin.png)

1. Ative **no** funções para transferir fatura de âmbito de acesso seguido de a guardar as alterações, para permitir que os utilizadores na subscrição.

    ![Captura de ecrã mostra no-off para delegar o acesso à fatura](./media/billing-manage-access/AA-optinAllow.png)

Aceitar permite que o administrador de serviço, coadministrador, proprietário, Contribuidor, leitor e leitor de faturação na subscrição para transferir faturas PDF no portal do Azure. No entanto, a notas fiscais com mais de Dezembro de 2016 estão disponíveis apenas para o administrador de conta por agora.

O administrador de conta também pode configurar ter faturas enviadas por e-mail. Para obter mais informações, consulte [receber sua fatura no e-mail](billing-download-azure-invoice-daily-usage-date.md).

## <a name="adding-users-to-the-billing-reader-role"></a>Adicionar utilizadores à função do leitor de faturação

A função de leitor de Faturação tem acesso só de leitura às informações de faturação de subscrição no portal do Azure e nenhum acesso a serviços, como VMs e contas de armazenamento. Atribua a função de leitor de faturação para alguém que precise acessar as informações de faturação de subscrição, mas não a capacidade de gerir serviços do Azure. Esta função é adequada para os utilizadores numa organização que apenas executam gerenciamento financeiro e de custo para as subscrições do Azure.

1. Selecione a sua subscrição a partir da [painel de subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure.

1. Selecione **controlo de acesso (IAM)** e, em seguida, clique em **Add**.

    ![Captura de ecrã mostra IAM no painel da subscrição](./media/billing-manage-access/select-iam.PNG)

1. Escolher **leitor de faturação** no **selecionar uma função** página.

    ![Captura de ecrã mostra o leitor de faturação na vista de pop-up](./media/billing-manage-access/select-roles.PNG)

1. Escreva a mensagem de e-mail para o utilizador que pretende convidar, em seguida, clique em **OK** para enviar o convite.

    ![Captura de ecrã que mostra que introduza o e-mail para convidar alguém](./media/billing-manage-access/add-user.PNG)

1. Siga instruções no e-mail de convite para iniciar sessão como um leitor de faturação.

    ![Captura de ecrã que mostra o que pode ver o leitor de faturação no portal do Azure](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> A funcionalidade de leitor de faturação está em pré-visualização e ainda não suporta nuvens não global. As subscrições empresariais podem ver os custos, se o administrador da empresa tiver ativado os custos de vista.

## <a name="adding-users-to-other-roles"></a>Adicionar utilizadores a outras funções

Utilizadores nas outras funções, como proprietário ou contribuinte, podem aceder a informações de faturação não apenas, mas também para serviços do Azure. Para gerir estas funções, consulte [gerir o acesso com RBAC e o portal do Azure](../role-based-access-control/role-assignments-portal.md).

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a>Quem pode aceder a [Centro de contas](https://account.windowsazure.com)?

Apenas o administrador de conta pode iniciar sessão para o Centro de contas. O administrador de conta é o proprietário legal da subscrição. Por predefinição, a pessoa que inscreveu ou comprou a subscrição do Azure é o administrador de conta, a menos que o [a propriedade de subscrição foi transferida](billing-subscription-transfer.md) para outra pessoa. O administrador de conta pode criar subscrições, cancelar subscrições, alterar o endereço de cobrança de uma subscrição e gerir políticas de acesso para a subscrição.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
