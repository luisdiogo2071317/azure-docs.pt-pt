---
title: Alterar o seu cartão de crédito do Azure | Documentos da Microsoft
description: Descreve como como alterar o cartão de crédito utilizado para pagar uma subscrição do Azure
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 15252ced-1841-4a66-ae79-2e58af1d3370
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: 6a302c611b7c71dd8bc8c472330b79ea21504770
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824630"
---
# <a name="add-update-or-remove-a-credit-or-debit-card-for-azure"></a>Adicionar, atualizar ou remover um cartão de crédito ou débito para o Azure

No Centro de contas, pode adicionar um novo cartão de crédito, atualizar um cartão de crédito existente ou eliminar um cartão de crédito que não usa. Tem de ser um [conta de administrador](billing-subscription-transfer.md#whoisaa) para efetuar estas alterações.

**Deseja mudar para pagar por fatura?** Ver [comprar subscrições do Azure por fatura](billing-how-to-pay-by-invoice.md).
 
<a id="addcard"></a>

## <a name="add-a-new-credit-or-debit-card"></a>Adicionar um novo cartão de crédito ou débito

1. Inicie sessão para o [Centro de contas](https://account.windowsazure.com/Subscriptions) como o administrador de conta.
1. Selecione uma subscrição.
1. No lado direito da página, selecione **Gerir métodos de pagamento**.

    ![Captura de ecrã que mostra a opção de métodos de pagamento de gerir selecionada.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Selecione "+" para adicionar um cartão.

    ![Captura de ecrã que mostra a opção de editar ao lado do método de pagamento.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Introduza os detalhes do cartão de débito ou crédito.
1. Selecione **Guardar**. 

Se obtiver um erro depois de adicionar o cartão de crédito, consulte [recusado de cartão de crédito no Azure, inscreva-se](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-or-debit-card"></a>Atualizar o cartão de crédito existente ou cartão de débito

Se for renovado seu cartão de crédito e o número permanece o mesmo, atualize os detalhes de cartão de crédito existente como a data de expiração. Se as alterações de número de cartão de crédito porque o cartão é perdido, roubado ou expirado, siga os passos a [adicionar um cartão de crédito como um método de pagamento](#addcard) secção. Não precisa de atualizar o CVV.

1. Inicie sessão para o [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions) como o administrador de conta.
1. Selecione a subscrição que está ligada ao cartão.
1. Selecione **gerir métodos de pagamento**.
1. Selecione **editar** junto do cartão que pretende atualizar.
1. Atualize os detalhes de cartão de crédito ou débito.
1. Selecione **Guardar**.

## <a name="use-a-different-credit-card-for-the-azure-subscription"></a>Utilizar outro cartão de crédito para a subscrição do Azure

1. Inicie sessão para o [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions) como o administrador de conta.
1. Selecione a subscrição que está ligada ao cartão.
1. No lado direito da página, selecione **Gerir métodos de pagamento**.
1. Clique em **utilizar em vez disso** junto ao cartão que pretende utilizar. Este procedimento também atualiza quaisquer outras subscrições que está associadas este cartão. 

## <a name="remove-a-credit-or-debit-card-from-the-account"></a>Remover um cartão de crédito ou débito a partir da conta

1. Inicie sessão para o [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions) como administrador de conta.
1. Selecione a subscrição que está ligada ao cartão.
3. No lado direito da página, selecione **Gerir métodos de pagamento**.
4. Clique em **eliminar** para o cartão de crédito que pretende eliminar.

Se o seu cartão de crédito é associado a outras subscrições do Microsoft Active Directory, não é possível removê-lo a partir da sua conta do Azure. Remova o cartão de crédito de todas as subscrições ativas que tem com a Microsoft e tente novamente.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>A minha subscrição está desativada. Por que motivo não posso remover meu cartão de crédito agora?

Após a sua subscrição está desativada ou foi cancelada, podemos esperar 90 dias antes de serem eliminados permanentemente a sua subscrição. Mantemos o método de pagamento registado durante o período de retenção, no caso de que pretende reativar a subscrição. Depois disso, é completamente eliminar a subscrição.

Se tiver de remover o seu cartão de crédito ou débito antes de terminar o período de retenção de 90 dias, [reativar a sua subscrição](billing-subscription-become-disable.md). Se não é possível reativar, [contacte o suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Por que posso continuar a receber "a sua sessão de início de sessão expirou. Clique aqui para voltar a iniciar sessão"?

Se continuar a receber esta mensagem de erro, mesmo que já tiver iniciado sessão e iniciá-la no, tente novamente com uma sessão de navegação privada.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Como posso utilizar um cartão diferente para cada subscrição que tenho?

Infelizmente, se as suas subscrições já estiver a utilizar o cartão, não é possível para separá-las para utilizar cartões diferentes. No entanto, quando se inscreve para uma nova subscrição, pode optar por utilizar um novo método de pagamento para essa subscrição.

### <a name="how-do-i-make-payments"></a>Como me certifico de pagamentos?

Se configurar um cartão de crédito ou um cartão de débito como o método de pagamento, é cobrada automaticamente seu cartão após cada período de faturação. Não precisa de fazer nada.

Se estiver [pagar por fatura](billing-how-to-pay-by-invoice.md), envie o pagamento para a localização listado na parte inferior da sua fatura.

### <a name="how-do-i-make-a-one-time-payment"></a>Como faço para que um único pagamento?

Infelizmente, o Azure atualmente não suporta pagamentos de uso individual para cartões de crédito ou débito. 

### <a name="how-do-i-change-the-tax-id"></a>Como posso alterar o ID de imposto?

Para adicionar ou atualizar o ID de imposto, visite [ **perfil** no Centro de contas do Azure](https://account.azure.com/Profile), em seguida, selecione **imposto sobre vendas registo**. O NIF é utilizado para cálculos de isenção fiscal e é apresentado na sua fatura.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
