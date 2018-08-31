---
title: Comprar planos de SUSE Linux - reservas do Azure | Documentos da Microsoft
description: Saiba como pode efetue o pré-pagamento para a sua utilização do SUSE e poupar dinheiro sobre os custos de pay as you go.
services: virtual-machines-linux
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2018
ms.author: yashar
ms.openlocfilehash: 9c3976a5fa98049de03f2a65b71f1fc927947142
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307857"
---
# <a name="prepay-for-suse-software-plans-from-azure-reservations"></a>Efetue o pré-pagamento do planos de software SUSE das reservas do Azure

Efetue o pré-pagamento para a sua utilização do SUSE e poupar dinheiro sobre os custos de pay as you go. Os descontos só se aplicam a medidores de SUSE e não na utilização da máquina virtual. Pode comprar reservas para máquinas virtuais em separado para poupar ainda mais.

Pode comprar planos de software SUSE no portal do Azure. Comprar um plano:

- Deve estar numa função de proprietário de, pelo menos, uma empresa ou uma subscrição pay as you go.
- Para subscrições Enterprise, compras de reserva tem de estar ativadas no [portal EA](https://ea.azure.com).
- Para o programa de fornecedor de soluções Cloud (CSP), os agentes de administrador ou agentes de vendas podem comprar os planos SUSE.

## <a name="buy-a-suse-software-plan"></a>Comprar um plano de software SUSE

1. Aceda a [reservas](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) no portal do Azure.
1. Selecione **adicionar** e selecione o SUSE Linux.
1. Preencha os campos obrigatórios. Qualquer VM do Linux SUSE que corresponda os atributos de compra obtém o desconto. O número real de implementações que obter o desconto depende do escopo e quantidade selecionado.

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta compra.|
    |Subscrição|A subscrição utilizada para pagar para este plano. O método de pagamento da subscrição é cobrado os custos iniciais para a reserva. O tipo de subscrição tem de ser um Contrato Enterprise (número da oferta: MS-AZR-0017P) ou o modelo Pay As You Go (número da oferta: MS-AZR-0003P). Para uma subscrição Enterprise, os custos são deduzidos do saldo de fidelização monetária da inscrição ou cobrados como utilização excedida. Para a subscrição Pay As You Go, os custos são debitados no cartão de crédito ou cobrados de acordo com o método de pagamento indicado na subscrição.|
    |Âmbito       |O âmbito pode abranger uma subscrição ou várias subscrições (âmbito partilhado). Se selecionar: <ul><li>Subscrição individual - o desconto de plano é aplicada a utilização do SUSE Linux nesta subscrição. </li><li>O desconto de plano partilhado - é aplicado a utilização do SUSE Linux em qualquer subscrição com o contexto de faturação. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.</li></ul>|
    |Plano do software     |Selecione o plano de SUSE Linux. Para ajudar a identificar o que comprar, consulte [compreender a forma como o desconto de reserva do software SUSE Linux Enterprise é aplicado](../../billing/billing-understand-suse-reservation-charges.md).|
    |Tamanho da VM     |Preços do SUSE Linux dependem do número de vCPUs na VM. Selecione a opção que representa o número de vCPUs nas suas VMs do Linux SUSE.|
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de VMs que está a comprar este plano de SUSE Linux para. A quantidade é o número de instâncias de SUSE Linux que podem obter o desconto de faturação em execução.|
1. Selecione **Comprar**.
1. Selecione **ver esta reserva** para ver o estado da sua compra.

O desconto de reserva é automaticamente aplicado às quaisquer máquinas de virtuais SUSE em execução que corresponda a reserva. O desconto aplica-se apenas para o medidor SUSE. Os custos de computação VM não são abrangidos por este plano.

## <a name="discount-applies-to-different-vm-sizes-with-instance-size-flexibility"></a>Desconto aplica-se aos diferentes tamanhos de VM com a flexibilidade de tamanho de instância

Como instâncias de VM reservadas, planos SUSE Linux oferecem a flexibilidade de tamanho de instância. Isso significa que se aplica o desconto até mesmo quando implementa uma VM que é um tamanho diferente do esquema de SUSE comprou. Para obter mais informações, consulte [compreender a forma como o desconto de reserva do software SUSE Linux Enterprise é aplicado](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="cancellation-and-exchanges-not-allowed"></a>Cancelamento e trocas não permitidas

Não é possível cancelar ou um plano SUSE que comprou do exchange. Verificar a sua utilização para se certificar de que compra o plano adequado. Para ajudar a identificar o que comprar, consulte [compreender a forma como o desconto de reserva do software SUSE Linux Enterprise é aplicado](../../billing/billing-understand-suse-reservation-charges.md).

## <a name="next-steps"></a>Passos Seguintes

Para saber como gerir uma reserva, consulte [reservas de gerir o Azure](../../billing/billing-manage-reserved-vm-instance.md).

Para obter mais informações, consulte os artigos seguintes:

- [Quais são as reservas do Azure?](../../billing/billing-save-compute-costs-reservations.md)
- [Gerir reservas no Azure](../../billing/billing-manage-reserved-vm-instance.md)
- [Compreender a forma como o desconto de reserva de SUSE é aplicado](../../billing/billing-understand-suse-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](../../billing/billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](../../billing/billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.