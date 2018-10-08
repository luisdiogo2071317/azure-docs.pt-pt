---
title: Transferir fatura do Azure e dados de uso diário | Microsoft Docs
description: Descreve como transferir ou visualizar seus dados do Azure faturas da fatura e diário utilização.
keywords: cobrança de fatura, a transferência de faturas, a fatura do azure, utilização do azure
services: billing
documentationcenter: ''
author: genlin
manager: tonguyen
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: f0cdfef50c07674a08766933f2f7edfc946462a4
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395020"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Baixe ou exiba sua fatura do Azure e dados de utilização diária
Pode transferir a fatura do [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ou enviá-lo no e-mail. Para transferir a sua utilização diária, vá para o [Centro de contas do Azure](https://account.azure.com/Subscriptions). Apenas determinadas funções do utilizador tem permissão para obter fatura e as informações de utilização, como o administrador de conta. Para saber mais sobre como obter acesso a informações de faturação, veja [Gerir acesso às funções de utilização de faturação do Azure](billing-manage-access.md).

Este artigo não é aplicável a clientes Enterprise Agreement (EA). Se for um cliente EA, as faturas são enviadas diretamente para os administradores de inscrição.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

> [!div class="nextstepaction"]
> [Ajude a melhorar os documentos de faturação do Azure](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="get-your-invoice-in-email-pdf"></a>Obter a fatura por correio eletrónico (. pdf)
Pode optar ativamente e configurar os destinatários principais para receber sua fatura do Azure por email. Esta funcionalidade não pode estar disponível para determinadas subscrições como ofertas de suporte, contratos de empresa ou do Azure no Open.

1. Selecione a sua subscrição a partir da [página subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Participar para cada subscrição que for o proprietário. Clique em **Faturas** , em seguida, **Enviar minha fatura por email**. 

    ![Captura de ecrã que mostra o fluxo de participação ativa](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Clique em **optar ativamente por participar no** e aceite os termos.

    ![Captura de ecrã que mostra a etapa de aceitação fluxo 2](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Depois de ter aceite o contrato, pode configurar os destinatários adicionais. Quando um destinatário for removido, o endereço de e-mail já não é armazenado. Se mudar de ideias, terá de voltar a adicioná-los.

    ![Captura de ecrã que mostra a etapa de aceitação do fluxo 3](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Se não receber uma mensagem de e-mail depois de seguir os passos, certifique-se de que o seu endereço de e-mail está correto no [preferências de comunicação no seu perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>Optar ativamente por participar da obtenção de sua fatura no e-mail
Se não quiser receber sua fatura no e-mail, clique em opcional horizontalmente de faturas por e-mail. Esta ação remove quaisquer endereços de e-mail definidos para receber faturas por e-mail. Se optar back, terá de reconfigurar os destinatários.

 ![Captura de ecrã que mostra o fluxo de recusa](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

## <a name="download-invoice-from-azure-portal-pdf"></a>Transferir fatura a partir do portal do Azure (. pdf)

1. Selecione a sua subscrição a partir da [página Subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure como [um utilizador com acesso às faturas](billing-manage-access.md).

2. Selecione **Faturas**. 

    ![Captura de ecrã que mostra a opção de faturação e utilização](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Clique em **transferir fatura** para visualizar uma cópia da sua fatura em PDF. Se disser **Não disponível**, confira [Por que motivo não vejo uma fatura para o último período de faturação?](#noinvoice)

    ![Captura de ecrã que mostra períodos de faturação, a opção de transferência e o total das cobranças para cada período de faturação](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Também pode ver a sua utilização diária clicando no período de faturação. 

Para obter mais informações sobre a sua fatura, consulte [compreender a sua fatura do Microsoft Azure](billing-understand-your-bill.md). Para ajudar a gerir os custos, veja [evitar custos inesperados com a faturação do Azure e a gestão de custos](billing-getting-started.md).

## <a name="download-usage-from-the-account-center-csv"></a>Transferir utilização a partir do Centro de contas (. csv)

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

## <a name="noinvoice"></a> Por que motivo não vejo uma fatura para o último período de faturação?

Pode haver vários motivos para não ver uma fatura:

- Tem um montante de crédito mensal com a sua subscrição que não foi excedido ou tiver uma avaliação gratuita. Uma fatura é gerada apenas quando deve dinheiro.

- É menos de 30 dias a partir do dia que subscrito para o Azure.

- A fatura ainda não foi gerada. Aguarde até ao fim do período de faturação.

- Se não for o administrador de conta, as faturas mais antigas podem não estar disponíveis para.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.
Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.

