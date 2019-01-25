---
title: Transferir fatura do Azure e dados de uso diário | Microsoft Docs
description: Descreve como transferir ou visualizar seus dados do Azure faturas da nota fiscal e diário utilização.
keywords: cobrança de nota fiscal, a transferência de faturas, a fatura do azure, utilização do azure
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2018
ms.author: banders
ms.openlocfilehash: 669d4be52cb74296bb034c773b820e14d7eede96
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902198"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Baixe ou exiba sua fatura do Azure e dados de utilização diária

Para a maioria das subscrições, pode transferir a fatura do [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ou enviá-lo no e-mail. Se for um cliente do Azure com um Enterprise Agreement (cliente EA), não é possível transferir as notas fiscais de sua organização. Notas fiscais são enviadas para quem está configurado para receber as notas fiscais para a inscrição.

Se quiser transferir utilização como um cliente EA, está disponível na [portal do Azure](https://portal.azure.com/) > **gestão de custos + faturação** > **utilização e custos**. Para outras subscrições, vá para o [Centro de contas do Azure](https://account.azure.com/Subscriptions).

Apenas determinadas funções do utilizador tem permissão para obter informações de fatura e utilização, como o administrador de conta ou administrador de empresa de faturação. Para saber mais sobre como obter acesso a informações de faturação, veja [Gerir acesso às funções de utilização de faturação do Azure](billing-manage-access.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-or-view-your-invoice"></a>Baixe ou exiba sua fatura

 Se for um cliente EA, não é possível transferir as notas fiscais de sua organização. Notas fiscais são enviadas para quem está configurado para receber as notas fiscais para a inscrição. Para outras subscrições, pode receber a sua fatura no e-mail ou transferi-lo a partir do portal do Azure.

### <a name="get-your-invoice-in-email-pdf"></a>Obter a fatura por correio eletrónico (. pdf)
Pode optar ativamente e configurar os destinatários principais para receber sua fatura do Azure por email. Esta funcionalidade não pode estar disponível para determinadas subscrições como ofertas de suporte, contratos de empresa ou do Azure no Open.

1. Selecione a sua subscrição a partir da [página subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Escolher para cada subscrição que for o proprietário. Clique em **Faturas** , em seguida, **Enviar minha fatura por email**. 

    ![Captura de ecrã que mostra o fluxo de participação ativa](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Clique em **optar ativamente por participar no** e aceite os termos.

    ![Captura de ecrã que mostra a etapa de aceitação fluxo 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Depois de ter aceite o contrato, pode configurar os destinatários adicionais. Quando um destinatário for removido, o endereço de e-mail já não é armazenado. Se mudar de ideias, terá de voltar a adicioná-los.

    ![Captura de ecrã que mostra a etapa de aceitação do fluxo 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Se não receber uma mensagem de e-mail depois de seguir os passos, certifique-se de que o seu endereço de e-mail está correto no [preferências de comunicação no seu perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>Optar ativamente por participar da obtenção de sua fatura no e-mail
Se não quiser receber sua fatura no e-mail, clique em **recusar faturas por e-mail**. Esta opção remove quaisquer endereços de e-mail definidos para receber faturas por e-mail. Se optar ativamente por participar de volta, terá de reconfigurar os destinatários.

 ![Captura de ecrã que mostra o fluxo de recusa](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="download-invoice-from-azure-portal-pdf"></a>Transferir fatura a partir do portal do Azure (. pdf)

1. Selecione a sua subscrição a partir da [página Subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure como [um utilizador com acesso às faturas](billing-manage-access.md).

2. Selecione **Faturas**. 

    ![Captura de ecrã que mostra a opção de faturação e utilização](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Clique em **transferir fatura** para visualizar uma cópia da sua fatura em PDF. Se disser **Não disponível**, confira [Por que motivo não vejo uma fatura para o último período de faturação?](#noinvoice)

    ![Captura de ecrã que mostra períodos de faturação, a opção de transferência e o total das cobranças para cada período de faturação](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Também pode ver a sua utilização diária clicando no período de faturação. 

Para obter mais informações sobre a sua fatura, consulte [compreender a sua fatura do Microsoft Azure](billing-understand-your-bill.md). Para ajudar a gerir os custos, veja [evitar custos inesperados com a faturação do Azure e a gestão de custos](billing-getting-started.md).

### <a name="noinvoice"></a> Por que motivo não vejo uma fatura para o último período de faturação?

Pode haver vários motivos para não ver uma fatura:

- A sua subscrição inclui um montante de crédito mensal que não excedeu ou tem uma Avaliação Gratuita. Uma fatura é gerada apenas quando deve dinheiro.

- Passaram menos de 30 dias desde o dia em que subscreveu o Azure.

- A fatura ainda não foi gerada. Aguarde até ao fim do período de faturação.

- Se não for o Administrador de Conta, as faturas mais antigas podem não estar disponíveis para si.

## <a name="download-usage"></a>Transferir utilização

 Para a maioria das subscrições, encontre o ficheiro de utilização diária no [Centro de contas do Azure](https://account.azure.com/Subscriptions). Se quiser transferir utilização como um cliente EA, está disponível na [portal do Azure](https://portal.azure.com/) > **gestão de custos + faturação** > **utilização e custos**. 

### <a name="download-usage-from-the-account-center-csv"></a>Transferir utilização a partir do Centro de contas (. csv)

1. Inicie sessão no [Centro de contas do Azure](https://account.windowsazure.com/subscriptions) como o administrador de conta.

2. Selecione a subscrição que pretende que as informações da fatura e utilização.

3. Selecione **histórico de FATURAÇÃO**. 

    ![Captura de ecrã que mostra a opção de histórico de faturação](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Pode ver as suas instruções para os últimos seis períodos de faturas e o período de faturação por faturar atual. 

    ![Captura de ecrã que mostra períodos de faturação, as opções a transferir fatura e utilização diária e total das cobranças para cada período de faturação](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Selecione **Ver Extrato Atual** para ver uma estimativa dos custos no momento em que a estimativa foi gerada. Esta informação é atualizada diariamente apenas e não pode incluir toda a sua utilização. A fatura mensal poderá divergir desta estimativa.

    ![Captura de ecrã que mostra a opção de ver extrato atual](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Captura de ecrã que mostra a estimativa de custos atuais](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Selecione **Transferir Utilização** para transferir os dados de utilização diária como um ficheiro CSV. Se vir duas versões disponíveis, transfira a versão 2.

    ![Captura de ecrã que mostra a opção de transferir utilização](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Apenas o administrador de conta pode aceder ao centro de contas do Azure. Outros administradores de faturas como, por exemplo, um proprietário, podem obter a utilização de informações de utilização a [APIs de faturação](billing-usage-rate-card-overview.md).

Para obter mais informações sobre a sua utilização diária, veja [Compreender a fatura do Microsoft Azure](billing-understand-your-bill.md). Para ajudar a gerir os custos, veja [evitar custos inesperados com a faturação do Azure e a gestão de custos](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Transferir utilização para clientes com EA

Para ver e transferir dados de utilização como um cliente EA, tem de ser um administrador de empresa, ou administrador do departamento ou proprietário da conta com a política de encargos de vista ativada.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Pesquisar nos **custo Management + faturação**.

    ![Captura de ecrã que mostra a pesquisa de portal do Azure](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Selecione **utilização e custos**.
1. Para o mês em que deseja baixar, selecione **transferir**.

## <a name="need-help-contact-us"></a>Precisa de ajuda? Contacte-nos.

Se tiver alguma dúvida ou precisar de ajuda, [criar um pedido de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
