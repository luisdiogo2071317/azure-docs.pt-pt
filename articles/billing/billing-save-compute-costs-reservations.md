---
title: Quais são as reservas do Azure? | Microsoft Docs
description: Saiba mais sobre as reservas do Azure e preços para guardar em suas máquinas virtuais, bases de dados SQL e outros custos de recursos.
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
ms.date: 08/08/2018
ms.author: yashar
ms.openlocfilehash: 93c11852a11e0bb57a0b92090368298fc14b8c2a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626309"
---
# <a name="what-are-azure-reservations"></a>Quais são as reservas do Azure?

Capacidade de computação de ajuda de reservas do Azure que poupar dinheiro ao pagar previamente para um ano ou três anos de máquina virtual ou base de dados SQL. Pré-pagamento permite-lhe obter um desconto sobre os recursos que utiliza. Reservas do Azure podem reduzir significativamente sua máquina virtual ou os custos de computação de base de dados do SQL — até 72% em preços pay as you go – compromisso inicial de um ano ou três anos. reservas de fornecem um desconto de faturação e não afetam o estado de tempo de execução das suas máquinas virtuais ou bases de dados SQL.

Pode comprar uma reserva [portal do Azure](https://aka.ms/reservations). Para obter mais informações, consulte os seguintes tópicos:

- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Efetue o pré-pagamento do recursos de computação de base de dados SQL com capacidade de base de dados do SQL Azure reservados](../sql-database/sql-database-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>Por que devo comprar uma reserva?

Se tiver máquinas virtuais ou bases de dados do SQL que são executadas durante longos períodos de tempo, comprar uma reserva dá-lhe a opção mais económica. Por exemplo, se executar quatro instâncias de um serviço sem uma reserva de forma contínua, são cobradas às tarifas pay as you go. Se comprar uma reserva para esses recursos, obtém imediatamente o desconto de reserva. Os recursos já não são cobrados às tarifas pay as you go.

## <a name="what-charges-does-a-reservation-cover"></a>O que os custos é uma capa de reserva?

- Instância de Máquina Virtual reservada: Uma reserva abrange apenas os custos de computação de máquina virtual. Não cobre custos adicionais de software, sistema de rede ou armazenamento.
- Base de dados SQL reservadas vCore: apenas os custos de computação estão incluídos com uma reserva. A licença é faturada em separado.

Para as máquinas virtuais do Windows e a base de dados SQL, que podem ser cobertos os custos de licenciamento com [benefício híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>Quem é elegível para comprar uma reserva?

Os clientes do Azure com esses tipos de subscrições, podem comprar uma reserva:

- Tipo de oferta de subscrição Contrato Enterprise (MS-AZR-0017P).
- Tipo de oferta de subscrição [Pay As You Go](https://azure.microsoft.com/offers/ms-azr-0003p/) (MS-AZR-003P). Tem de ter a função "Proprietário" na subscrição para comprar uma reserva. Para comprar reservas numa inscrição de enterprise, o administrador de empresa tem de ativar as compras de reserva no portal do EA. Por predefinição, esta definição está ativada.
- Parceiros da cloud Solution Provider (CSP) podem utilizar o portal do Azure ou [Centro de parceiros](https://docs.microsoft.com/partner-center/azure-reservations) comprar reservas do Azure. 

Um desconto de reserva do Azure só se aplica a máquinas virtuais ou bases de dados SQL associada com tipos de subscrição Enterprise, pay as you go ou CSP.

## <a name="how-is-a-reservation-billed"></a>Como é faturada uma reserva?

A reserva é cobrada ao método de pagamento associado à subscrição. Se tiver uma subscrição Enterprise, o custo da reserva é deduzido da seu saldo de alocação monetária. Se o seu saldo de alocação monetária não abrange o custo da reserva, é-lhe cobrada a utilização excedida. Se tiver uma subscrição pay as you go, o cartão de crédito que tem na sua conta é cobrado imediatamente. Se a faturação por fatura, verá os custos na sua fatura seguinte.

## <a name="how-is-the-reservation-discount-applied"></a>Como é que o desconto de reserva é aplicado?

O desconto de reserva se aplica a utilização de recursos que corresponda os atributos que selecionou quando comprar a reserva. Os atributos incluem o âmbito em que as VMs correspondentes, bases de dados SQL, ou executam outros recursos. Por exemplo, se pretender que um desconto de reserva para quatro máquinas de virtuais de Standard D2 na região E.U.A. oeste, selecione a subscrição em que as VMs estão em execução. Se as máquinas virtuais são executadas em subscrições diferentes na sua conta/inscrição, em seguida, selecione o âmbito como partilhado. Âmbito partilhado permite que o desconto de reserva a ser aplicado entre subscrições. Pode alterar o âmbito depois de comprar uma reserva. Para obter mais informações, consulte [Gerir reservas no Azure](billing-manage-reserved-vm-instance.md).

O desconto de reserva aplica-se apenas a máquinas virtuais ou bases de dados SQL associado de empresa ou uma subscrição pay as you go tipos. As máquinas virtuais ou bases de dados do SQL que são executados numa subscrição com outros tipos de oferta não recebem o desconto de reserva. Para inscrições de empresas, as subscrições de desenvolvimento/teste empresarial não elegíveis para o benefício de reserva.

Para compreender melhor como as reservas afeta sua máquina virtual ou a cobrança de base de dados SQL, consulte [compreender como o desconto de reserva é aplicado](billing-understand-vm-reservation-charges.md).

## <a name="what-happens-when-the-reservation-term-expires"></a>O que acontece quando o termo de reserva expira?

No final do período de reserva, expira o desconto de faturação e a máquina virtual, a base de dados SQL ou a outros recursos são cobrados ao pay as you acedo preço. Reservas do Azure não a renovação automática. Para continuar a obter o desconto de faturação, tem de comprar uma reserva nova para a reserva serviços elegíveis.

## <a name="next-steps"></a>Passos Seguintes

Iniciar a guardar nas suas máquinas virtuais ao comprar uma [instância de VM reservada](../virtual-machines/windows/prepay-reserved-vm-instances.md) ou [capacidade de reserva de base de dados SQL](../sql-database/sql-database-reserved-capacity.md).

Para saber mais sobre as reservas, veja os artigos seguintes:

- [Gerir reservas do Azure](billing-manage-reserved-vm-instance.md)
- [Compreender como o desconto de reserva é aplicado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com reservas](billing-reserved-instance-windows-software-costs.md)
- [Reservas do Azure no programa de fornecedor de soluções (CSP) do parceiro Center na nuvem](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
