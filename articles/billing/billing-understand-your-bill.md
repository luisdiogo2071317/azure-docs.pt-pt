---
title: Compreender a sua fatura do Azure | Documentos da Microsoft
description: Saiba como ler e compreender a utilização e faturação para a sua subscrição do Azure
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
ms.author: cwatson
ms.openlocfilehash: f7ec113a7fa51fc6d3684c2e7ee9379bd1534e30
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392419"
---
# <a name="understand-your-bill-for-microsoft-azure"></a>Compreender a sua fatura do Microsoft Azure
Para compreender a fatura do Azure, compare a fatura com o ficheiro de utilização diária detalhadas e os relatórios de gestão de custos no portal do Azure.

>[!NOTE]
>Este artigo não é aplicável a clientes Enterprise Agreement (EA). Se for um cliente EA, [pode encontrar a documentação de nota fiscal no Enterprise Portal.](https://ea.azure.com/helpdocs/understandingYourInvoice)  

Para obter um PDF da sua fatura e uma cópia do seu detalhada diária utilização CSV transferência de ficheiros, consulte [obter a faturação da nota fiscal e diário de dados de utilização do Azure](billing-download-azure-invoice-daily-usage-date.md). 

Para termos detalhados e as descrições de sua fatura e o ficheiro de utilização diária detalhadas, consulte [compreender os termos na sua fatura do Microsoft Azure](billing-understand-your-invoice.md) e [Compreender termos no seu Microsoft Azure detalhadas utilização](billing-understand-your-usage.md). 

Para obter detalhes sobre os relatórios de gestão de custos, veja [gestão de custos do portal do Azure](https://docs.microsoft.com/azure/billing/billing-getting-started).

> [!div class="nextstepaction"]
> [Ajude a melhorar os documentos de faturação do Azure](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="charges"></a>Como posso Certifique-se de que os encargos na minha fatura estão corretos?

>[!VIDEO https://www.youtube.com/embed/3YegFD769Pk]

Se existe uma cobrança na sua fatura que pretende obter mais detalhes nos, existem algumas opções.

### <a name="option-1-review-your-invoice-and-compare-the-usage-and-costs-with-the-detailed-usage-csv-file"></a>Opção 1: Rever a sua fatura e compare a utilização e os custos com o ficheiro CSV de utilização detalhada

O ficheiro CSV de utilização detalhada mostra os encargos por período de faturação e utilização diária. Para obter o ficheiro CSV de utilização detalhada, consulte [obter a faturação da nota fiscal e diário de dados de utilização do Azure](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date).

As cobranças de utilização são apresentadas no nível do medidor. Os seguintes termos significa que a mesma coisa de nota fiscal e o ficheiro de utilização detalhada. Por exemplo, o ciclo de faturação da nota fiscal é equivalente do período de faturação mostrado no ficheiro de utilização detalhada.

 | Nota fiscal (PDF) | Utilização detalhada (CSV)|
 | --- | --- |
|Ciclo de faturação | Período de Faturação |
 |Nome |Categoria do Medidor |
 |Tipo |Subcategoria do medidor |
 |Recurso |Nome do Medidor |
 |Região |Região do Medidor |
 |Consumido |Quantidade Consumida |
 |Incluída |Quantidade Incluída |
 |A Cobrar |Quantidade de Utilização Excedente |

O **os custos de utilização** secção da sua fatura tem o valor total para cada contador que foi consumida durante seu período de faturação. Por exemplo, a captura de ecrã seguinte mostra um custo de utilização para o serviço de agendador do Azure.

![Custos de utilização de nota fiscal](./media/billing-understand-your-bill/1.png)

O **instrução** secção da sua utilização detalhada CSV mostra o valor cobrado pelo mesmo. Ambas as *consumida* quantidade e *valor* corresponder a nota fiscal.

![Custos de utilização CSV](./media/billing-understand-your-bill/2.png)

Para ver uma análise detalhada desta cobrança diariamente, vá para o **utilização diária** secção de CSV. Filtrar por "Scheduler" em *categoria do medidor* e pode ver quais os dias foi utilizado o medidor e qual a quantidade consumida. O *Resource* e *grupo de recursos* informações também estão listadas para comparação. O *consumida* valores devem adicionar a cópia de segurança para o que é apresentado da nota fiscal.

![Secção utilização diária o CSV](./media/billing-understand-your-bill/3.png)

Para obter o custo por dia, multiplique os *consumida* resumem-se com o *taxa* partir do **instrução** secção.

Para saber mais sobre a nota fiscal, veja [compreender a fatura do Azure](billing-understand-your-invoice.md).

Para saber mais sobre cada uma das colunas no CSV, consulte [compreender a utilização detalhada do Azure](billing-understand-your-invoice.md).

### <a name="option-2-review-your-invoice-and-compare-with-the-usage-and-costs-in-the-azure-portal"></a>Opção 2: Rever a sua fatura e compare com a utilização e custos no portal do Azure

O portal do Azure pode também ajudar a verificar os encargos. O portal do Azure fornece gráficos de gestão de custos para uma visão geral da sua utilização e os custos na sua fatura.

Para continuar com o exemplo acima, visite o [página subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), selecione a sua subscrição e, em seguida, escolha **análise de custo**. A partir daí, pode especificar o intervalo de tempo e ver os custos de utilização para o serviço de agendador do Azure.

![Vista de análise de custo no portal do Azure](./media/billing-understand-your-bill/4.png)

Para ver a divisão de custo diário na **histórico dos custos**, clique na linha.

![Vista de histórico de custos no portal do Azure](./media/billing-understand-your-bill/5.png)

Para obter mais informações, consulte [evitar custos inesperados com a faturação do Azure e a gestão de custos](billing-getting-started.md#costs).

## <a name="external"></a>E encargos de serviços externos?
Serviços externos (também conhecido como o Azure Marketplace ordena) são fornecidos pelos fornecedores de serviço independentes e são faturados separadamente. Os encargos não aparecem na sua fatura do Azure. Para obter mais informações, consulte [compreender os encargos de serviços externos do Azure](billing-understand-your-azure-marketplace-charges.md).

## <a name="payment"></a>Como faço para que um pagamento?

Se configurar um cartão de crédito ou um cartão de débito como o método de pagamento, o pagamento é cobrado automaticamente dentro de 10 dias após terminar o período de faturação. Na sua declaração de cartão de crédito, o item de linha diria **do Azure da MSFT**.

Se [pagar por fatura](billing-how-to-pay-by-invoice.md), envie o pagamento para a localização listado na parte inferior da sua fatura. Para obter mais ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="how-do-i-check-the-status-of-a-payment-made-by-credit-card"></a>Como posso ver o estado de um pagamento feito com cartão de crédito?

[Criar um pedido de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para pedir o estado de seu pagamento. 

## <a name="are-there-different-azure-customer-types-how-do-i-know-what-customer-type-i-am"></a>Existem tipos diferentes de cliente do Azure? Como posso saber que tipo de cliente, eu estou?
Existem diferentes tipos de clientes do Azure. Para compreender melhor a preços e faturação, consulte as seguintes descrições de tipo de cliente.

- **Enterprise**: os clientes empresariais têm de assinar um contrato de Enterprise com o Azure para tornar negociados compromissos monetários e obter acesso aos preços personalizados para recursos do Azure.
- **Web Direct**: os clientes Web Direct não tem entrado qualquer contrato personalizado com o Azure. Estes clientes se inscreveram no Azure através de azure.com e recebem os preços de destinado ao públicos para todos os recursos do Azure.
- **Fornecedor de serviços cloud**: fornecedor de serviços Cloud são, normalmente, as empresas que foi contratadas por um cliente final para criar soluções baseadas no Azure.

## <a name="why-dont-i-see-the-cost-the-resource-i-have-created-in-my-bill"></a>Por que motivo não vejo o custo do recurso que criei na minha fatura?
Faz do Azure não fatura diretamente com base no custo do recurso. A faturação é feita com base desativar um ou mais medidores que são utilizadas para controlar a utilização de um recurso durante seu ciclo de vida. Destes medidores, em seguida, são utilizados para calcular a fatura. Ver mais informações sobre a medição do Azure abaixo.

## <a name="how-does-azure-charge-metering-work"></a>Como o Azure cobra o trabalho medição?
Quando acelerar um único recurso do Azure, como uma máquina virtual, terá uma ou várias instâncias de medidor criadas também. Estes medidores são usados para controlar a utilização do recurso ao longo do tempo. Cada medidor emite os registos de utilização que são, em seguida, utilizados pelo Azure no nosso sistema de medição de custo para calcular a fatura. 

Por exemplo, uma única máquina virtual criada no Azure pode ter os seguintes medidores criados para controlar a sua utilização:

- Horas de Computação
- Horas de endereço IP
- Transferência de dados de
- Transferências de Dados de Saída
- Disco gerido Standard
- Operações de disco gerido Standard
- E/s padrão-disco
- Leitura de BLOBs de blocos de e/s padrão
- Escrita de BLOBs de blocos de e/s padrão
- Eliminação de BLOBs de blocos de e/s de padrão

Assim que a VM é criada, cada um dos medidores acima irá começar a emitir os registos de utilização. Esta utilização, em seguida, será utilizada no sistema de medição do Azure, juntamente com o preço do medidor para determinar quanto um cliente é cobrado.

> [!Note]
> Os medidores de exemplo acima só podem ser um subconjunto de medidores criada uma VM que é criada.

## <a name="what-is-the-difference-between-azure-1st-party-charges-and-azure-marketplace-charges"></a>Qual é a diferença entre o Azure 1º de encargos de terceiros e os encargos do Azure Marketplace?
Azure 1º de custos com terceiros às são recursos que são desenvolvidos diretamente e disponibilizadas pelo Azure. Encargos do Azure Marketplace são para os recursos que foram criados por fornecedores de software de terceiros que estão disponíveis para utilização através do Azure marketplace. Por exemplo, uma Firewall de Barracuda é um recurso do Azure marketplace oferecido por terceiros. Todas as cobranças para o firewall e de seu correspondentes medidores serão apresentados individualmente como encargos do marketplace. 

## <a name="tips-for-cost-management"></a>Dicas para a gestão de custos
- Estimar os custos ao utilizar o [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/) e [custo total da Calculadora de propriedade](https://aka.ms/azure-tco-calculator)e obtenha o [detalhes dos preços informações para cada serviço](https://azure.microsoft.com/pricing/).
- [Configurar alertas de faturação](billing-set-up-alerts.md).
- [Reveja a utilização e custos regularmente no portal do Azure](billing-getting-started.md#costs).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
