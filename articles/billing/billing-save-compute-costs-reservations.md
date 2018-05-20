---
title: O que são instâncias reservados do Azure? -Faturação as do azure | Microsoft Docs
description: Saiba mais sobre reservado instâncias de VM do Azure e VM preços para guardar os custos de máquinas virtuais e obter o melhor preço Efetivo.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: 93be4bb037af400599b88bb71f34143ee65a5deb
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="what-are-azure-reserved-vm-instances"></a>Quais são reservados instâncias de VM do Azure?
[Instâncias de VM do Azure reservado](https://azure.microsoft.com/pricing/reserved-vm-instances) ajuda a poupar dinheiro por pagar previamente um ano ou três anos de capacidade de cálculo, permitindo-lhe obter um desconto nas máquinas virtuais que utilizar. Reservado de instâncias do Azure pode reduzir significativamente os custos de máquina virtual — até 72 por cento num preços pay as you go – com o compromisso de compromisso de um ano ou três anos. Instâncias reservadas fornecem um desconto de faturação e não afetam o estado do tempo de execução das suas máquinas virtuais.

Pode comprar uma instância reservado (RI) [portal do Azure](https://aka.ms/reservations). Para obter mais informações, consulte [Prepay para máquinas virtuais e a poupar dinheiro com instâncias reservado](https://go.microsoft.com/fwlink/?linkid=861721).

## <a name="why-should-i-buy-a-reserved-instance"></a>Por que motivo deve a comprar uma instância reservado?
Se tiver máquinas virtuais executadas durante longos períodos de tempo, a compra de uma instância reservado dá-lhe a opção mais económica. Por exemplo, se executar continuamente quatro instâncias de uma VM D2 padrão na região EUA oeste, sem uma instância reservado-lhe é cobrados às taxas de pay as you go. Se comprar uma instância reservado os quatro VMs, as VMs obtêm imediatamente o benefício de faturação. Estes são já não é-lhe cobrados às taxas de pay as you go. 

## <a name="what-charges-does-a-reserved-instance-cover"></a>Os encargos abranger uma instância reservado?
Uma instância reservado abrange apenas os custos de infraestrutura de máquina virtual para as máquinas de virtuais do Windows ou Linux. Uma instância reservado não abrange os encargos de armazenamento, redes ou software adicionais. Para máquinas virtuais do Windows, pode cobrir Windows licenciamento os custos com [Azure híbrida benefício](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reserved-instance"></a>Quem é elegível para comprar uma instância reservado?
Clientes do Azure com estes tipos de subscrições podem comprar uma instância reservado:
-   Enterprise agreement oferta tipo de subscrição (MS-AZR - 0017P).
-   [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) o tipo de oferta de subscrição (MS-AZR - 003 P). Tem de ser a função de "Proprietário" na subscrição comprar uma instância reservado. Para a compra de instâncias reservado de inscrição enterprise, o administrador de empresa tem de ativar compras de instância reservado no portal do EA. Por predefinição, esta definição está ativada.
-   Parceiros de fornecedor de solução (CSP) da nuvem podem utilizar o Portal do Azure ou [Centro de parceiros](https://docs.microsoft.com/partner-center/azure-reservations) comprar instâncias reservado.

## <a name="how-is-a-reserved-instance-purchase-billed"></a>Como é faturada uma compra de instância reservado?
É cobrada a compra de instância reservado para o método de pagamento associado à subscrição. Se tiver uma subscrição do Enterprise, o custo de instância reservado é deducted da sua saldo de compromisso monetário. Se o saldo de compromisso monetário não abrange o custo da instância reservado, será cobrado o excedido.
Se tiver uma subscrição pay as you go, o cartão de crédito que tem na sua conta é faturado imediatamente. Se será cobrado por fatura, verá os encargos na sua fatura seguinte.

## <a name="how-is-the-purchased-reserved-instance-discount-applied"></a>Como é aplicado o adquiridas desconto de instância reservado?
O desconto instância reservado aplica-se às máquinas virtuais que correspondam os atributos que selecionar ao adquirir a instância reservado. Os atributos incluem o âmbito em que executam as VMs correspondentes. Por exemplo, se pretender um desconto de instância reservado para quatro máquinas virtuais que D2 padrão na região EUA oeste, selecione a subscrição onde as VMs estão em execução. Se estiver a executar as máquinas virtuais em diferentes subscrições na sua conta/inscrição, em seguida, selecione o âmbito como partilhada. Âmbito partilhado permite que o desconto de instância reservado sejam aplicadas nas subscrições. Pode alterar o âmbito depois de comprar uma instância reservado. Para alterar o âmbito, consulte a documentação sobre como gerir instâncias reservado.

O desconto instância reservado apenas se aplica a máquinas virtuais associadas enterprise ou tipos de subscrição pay as you go. Máquinas virtuais em execução numa subscrição com outros tipos de oferta não recebem o desconto instância reservado. Para inscrições de empresa, as subscrições de programador/teste empresariais não elegíveis para as vantagens de instância reservado.

Para compreender melhor como instância reservado afeta a faturação da máquina virtual, consulte [Noções sobre a aplicação de instâncias reservado faturação benefício](https://go.microsoft.com/fwlink/?linkid=863405).

## <a name="what-happens-when-the-reserved-instance-term-expires"></a>O que acontece quando o termo de instância reservado expira?
No final do período da instância reservado, o faturação desconto expira e a infraestrutura de máquina virtual é faturada a pay-como-lhe aceda preço. Reservado instâncias não a renovação automática. Para continuar a introdução de desconto de faturação, tem de comprar uma nova instância reservado. 

## <a name="sizes-and-regional-availability"></a>Tamanhos e disponibilidade Regional
Instâncias reservadas estão disponíveis para a maioria dos tamanhos de VM com algumas exceções:
- As VMs em pré-visualização – qualquer série de VM ou o tamanho que está em pré-visualização não estão disponíveis para compra de instância reservado.
- Nuvens – reservadas instâncias não estão disponíveis para compra nas regiões do Azure US Government, Datacenters ou China. 
- Quota suficiente – A instância reservado que está confinada para uma única subscrição tem de ter vCPU quota disponível na subscrição para o novo RI. Por exemplo, se a subscrição de destino tem um limite de quota de 10 vCPUs para a série D, não é possível de comprar uma instância reservado para 11 Standard_D1 instâncias. A verificação de quota para instâncias reservado inclui as VMs já implementadas na subscrição. Por exemplo, se a subscrição tem uma quota de 10 vCPUs para a série D e tiver duas instâncias de standard_D1 implementadas, em seguida, pode adquirir uma instância reservado para 10 standard_D1 instâncias nesta subscrição. 
- Restrições de capacidade – circunstâncias raros, os limites do Azure, a compra de novas instâncias reservado para subconjunto de VM tamanhos, devido a baixa capacidade numa região.

## <a name="next-steps"></a>Passos Seguintes
Início guardar nas suas máquinas virtuais por comprar uma [instância reservado Azure](https://go.microsoft.com/fwlink/?linkid=861721). 

Para saber mais sobre instâncias reservadas, consulte os artigos seguintes:

- [Prepay para máquinas virtuais com instâncias reservadas](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir instâncias reservadas](billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com instâncias reservado](billing-reserved-instance-windows-software-costs.md)
- [Instâncias reservadas no programa de parceiro Center solução fornecedor Cloud (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
