---
title: Compreender a fatura do Azure | Microsoft Docs
description: Saiba como leia e compreenda a sua utilização e a fatura para a sua subscrição do Azure
services: ''
documentationcenter: ''
author: tonguyen10
manager: tonguyen
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2018
ms.author: tonguyen
ms.openlocfilehash: 689ea9e0d029bb65bc579fc914c6ed3073b4a96b
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064118"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Compreender a sua fatura do Microsoft Azure
Para compreender a fatura do Azure, compare da sua fatura com o ficheiro de utilização diária detalhadas e os relatórios de gestão de custos no portal do Azure.

>[!NOTE]
>Este artigo não se aplica aos clientes do Enterprise Agreement (EA). Se for um cliente EA [pode encontrar documentação fatura no Enterprise Portal.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

Para obter um PDF da sua fatura e uma cópia de detalhado diária utilização ficheiro CSV transferência, consulte [obter o Azure faturação dados de utilização de uma fatura e diariamente](billing-download-azure-invoice-daily-usage-date.md). 

Para os termos de detalhado e descrições da sua fatura e ficheiros de utilização diária detalhadas, consulte [compreender os termos de licenciamento na sua fatura do Microsoft Azure](billing-understand-your-invoice.md) e [termos de compreender sobre o Microsoft Azure detalhadas utilização](billing-understand-your-usage.md). 

Para obter detalhes sobre os relatórios de gestão de custo, consulte [portal do Azure a gestão de custos](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="charges"></a>Como torno se de que os encargos na minha fatura corretos?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Se não houver cobrada uma taxa na sua fatura que pretende obter mais detalhes nos, existem algumas das opções.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Opção 1: Rever a sua fatura e comparar a utilização e os custos com o ficheiro CSV de utilização detalhada

O ficheiro CSV de utilização detalhada mostra os custos por período de faturação e de utilização diária. Para obter o ficheiro CSV de utilização detalhada, consulte [obter o Azure faturação dados de utilização de uma fatura e diariamente](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

Os custos de utilização são apresentados ao nível do medidor. Os seguintes termos significam a mesma coisa na fatura e o ficheiro de utilização detalhada. Por exemplo, o ciclo de faturação a fatura é equivalente ao período de faturação apresentado no ficheiro de utilização detalhada.

 | Fatura (PDF) | Utilização de detalhado (CSV)|
 | --- | --- |
|Ciclo de faturação | Período de Faturação |
 |Nome |Categoria do Medidor |
 |Tipo |Subcategoria de medidor |
 |Recurso |Nome do Medidor |
 |Região |Região do Medidor |
 |Consumido |Quantidade Consumida |
 |Incluída |Quantidade Incluída |
 |A Cobrar |Quantidade de Utilização Excedente |

O **custos de utilização** secção da sua fatura tem o valor total para cada medidor consumida durante o período de faturação. Por exemplo, a seguinte captura de ecrã mostra um custo de utilização para o serviço de agendador do Azure.

![Custos de utilização da fatura](./media/billing-understand-your-bill/1.png)

O **instrução** secção da sua utilização detalhada CSV mostra a taxa mesma. Tanto o *consumido* quantidade e *valor* corresponder a fatura.

![Custos de utilização CSV](./media/billing-understand-your-bill/2.png)

Para ver uma divisão este gratuitamente diariamente, vá para o **utilização diária** secção do CSV. Filtrar por "Programador" em *categoria de medidor* e pode ver que dias foi utilizada a medir e quantidade consumido. O *recursos* e *grupo de recursos* informações também estão listadas para comparação. O *consumido* valores devem ser adicionados até ao que é apresentado na fatura.

![Secção utilização diária no CSV](./media/billing-understand-your-bill/3.png)

Para obter o custo por dia, multiplique a *consumido* quantidades com o *taxa* valor a partir do **instrução** secção.

Para saber mais sobre a fatura, consulte o artigo [compreender a fatura do Azure](billing-understand-your-invoice.md).

Para mais informações sobre cada uma das colunas no CSV, consulte o artigo [compreender a utilização do Azure de detalhado](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Opção 2: Consultar da sua fatura e comparar com a utilização e os custos no portal do Azure

O portal do Azure também pode ajudar a verificar os custos. O portal do Azure fornece gráficos de gestão de custo de uma rápida descrição geral da sua utilização e os encargos na sua fatura.

Para continuar com o exemplo de acima, visite o [página de subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecione a sua subscrição e, em seguida, escolha **análise de custos**. A partir daí, pode especificar o intervalo de tempo e ver os encargos de utilização para o serviço de agendador do Azure.

![Vista de custo de análise no portal do Azure](./media/billing-understand-your-bill/4.png)

Para ver a divisão diária de custo em **custo histórico**, clique na linha.

![Vista de custo de histórico no portal do Azure](./media/billing-understand-your-bill/5.png)

Para obter mais informações, consulte [evitar custos inesperados com faturação do Azure e custos de gestão](billing-getting-started.md#costs).

## <a name="external"></a>O que sobre externos encargos de serviço?
Os serviços externos (também conhecido como Azure Marketplace ordena) são fornecidos pelos fornecedores do serviço independentes e são cobrados separadamente. Os encargos não apareçam na sua fatura do Azure. Para obter mais informações, consulte [compreender a Azure cobra serviço externa](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Como efetuar um pagamento?

Se configurou um cartão de crédito ou de um débito como método de pagamento, é cobrado o pagamento automaticamente dentro de 10 dias após terminar o período de faturação. A instrução de cartão de crédito, o item de linha iria diga **MSFT Azure**.

Se lhe [pague por faturação](billing-how-to-pay-by-invoice.md), envie o pagamento para a localização indicado na parte inferior da sua fatura. Para obter mais ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Como posso verificar o estado de um pagamento efetuado por cartão de crédito?

[Criar um pedido de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pedir para o estado do pagamento. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>Existem tipos diferentes de cliente do Azure? Como posso saber que tipo de cliente sou?
Existem diferentes tipos de clientes do Azure. Para compreender melhor os preços e fatura, consulte as seguintes descrições de tipo de cliente.

- **Enterprise**: os clientes empresariais tiverem assinado um Enterprise Agreement com o Azure para tornar negociados compromissos monetários e obter acesso aos preços personalizadas para recursos do Azure.
- **Web direta**: clientes Web direta não tiverem assinado qualquer contrato personalizado com o Azure. Estes clientes tenham efetuado a inscrição do Azure através de azure.com e recebem preços destinado ao públicos para todos os recursos do Azure.
- **Fornecedor de serviços de nuvem**: fornecedores de serviços em nuvem são, normalmente, as empresas que tem sido recrutadas por um cliente de fim para criar soluções por cima do Azure.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>Por que motivo não vejo o custo do recurso criei na minha fatura?
Azure efetua não fatura diretamente com base nos recursos de custos. Faturação é efetuada com base fora de um ou mais medidores que são utilizadas para controlar a utilização de um recurso ao longo da respetiva duração. Estes medidores, em seguida, são utilizados para calcular a fatura. Ver mais informações sobre a medição do Azure abaixo.

## <a name="how-does-azure-charge-metering-work"></a>Como o Azure cobra o trabalho medição?
Quando a rotação configurar um único recurso do Azure, tal como uma máquina virtual, terá uma ou várias instâncias de medidor criadas, bem como. Estes medidores são utilizadas para controlar a utilização do recurso ao longo do tempo. Cada medidor emite registos de utilização que são depois utilizados pelo Azure no nosso sistema de medição de custos para calcular a fatura. 

Por exemplo, uma única máquina virtual criada no Azure podem ter os seguintes medidores criadas para controlar a respetiva utilização:

- Horas de Computação
- Horas de endereço IP
- Transferência de dados
- Transferências de Dados de Saída
- Disco gerido Standard
- Operações de disco gerido Standard
- E/s padrão-disco
- Leitura de BLOBs de blocos de e/s Standard
- Escrita de BLOBs de blocos de e/s Standard
- Eliminação de BLOBs de blocos de e/s Standard

Uma vez criada a VM, cada um deles medidores acima irá começar a emitir os registos de utilização. Esta utilização, em seguida, será utilizada no sistema de medição do Azure, juntamente com o preço de medição para determinar a quantidade de um cliente é cobrado.

> [!Note]
> Os medidores de exemplo acima só podem ser um subconjunto de medidores criada uma VM que é criada.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>Qual é a diferença entre o Azure 1ª encargos de terceiros e custos do Azure Marketplace?
Azure 1ª encargos de terceiros são recursos que são desenvolvidas diretamente e oferecidas pelo Azure. Custos do Azure Marketplace são recursos que foram criadas por fornecedores de software de terceiros que estão disponíveis para utilização através do Azure marketplace. Por exemplo, uma Firewall Barracuda é um recurso do Azure marketplace oferecido por terceiros. Todos os custos de firewall e o respetivos medidores correspondentes serão apresentada como custos do marketplace. 

## <a name="tips-for-cost-management"></a>Sugestões para a gestão de custos
- Estimar os custos, utilizando o [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/) e [custo total da Calculadora de propriedade](https://aka.ms/azure-tco-calculator)e obter o [detalhadas informações sobre preços para cada serviço](https://azure.microsoft.com/pricing/).
- [Configurar alertas de faturação](billing-set-up-alerts.md).
- [Reveja a utilização e os custos regularmente no portal do Azure](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se ainda precisar de ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
