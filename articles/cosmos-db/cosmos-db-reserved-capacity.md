---
title: Efetue o pré-pagamento do recursos do Azure Cosmos DB poupar dinheiro
description: Saiba como comprar capacidade do Azure Cosmos DB reservadas para poupar nos custos de computação.
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: f1582a54d305cbf2cc1b03b1a7fd2ee6079dd87e
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839233"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Efetue o pré-pagamento do recursos do Azure Cosmos DB com a capacidade de reserva

O Azure Cosmos DB reservado ajuda de capacidade que poupar dinheiro ao pagar previamente para recursos do Azure Cosmos DB para um ano ou três anos. Com capacidade de reservada do Azure Cosmos DB, pode obter um desconto no débito aprovisionado para recursos do Cosmos DB. Exemplos de recursos são bases de dados e contentores (tabelas, coleções e gráficos).

Capacidade de Cosmos DB reservada do Azure pode reduzir significativamente os custos de Cosmos DB&mdash;até 65% em preços regulares com um compromisso inicial de um ano ou três anos. Capacidade de reserva fornece um desconto de faturação e não afeta o estado de tempo de execução dos seus recursos do Azure Cosmos DB.

Capacidade de Cosmos DB reservada do Azure abrange o débito aprovisionado para os seus recursos. Não abrange a armazenamento e os encargos de rede. Assim que adquirir uma reserva, os custos de débito que correspondem a reserva atributos já não são cobrados a pay as you go taxas. Para obter mais informações sobre as reservas, consulte a [reservas Azure](../billing/billing-save-compute-costs-reservations.md) artigo. 

Pode comprar o Azure Cosmos DB reservado capacidade dos [portal do Azure](https://portal.azure.com). Comprar capacidade de reserva:

* Tem de estar na função de proprietário para, pelo menos, uma empresa ou uma subscrição pay as you go.  
* Para subscrições Enterprise, compras de reserva do Azure tem de estar ativadas no [portal EA](https://ea.azure.com/).  
* Para o programa de fornecedor de soluções Cloud (CSP), apenas os agentes de administrador ou agentes de vendas podem comprar capacidade reservada do Azure Cosmos DB.

## <a name="determine-the-required-throughput-before-purchase"></a>Determinar a taxa de transferência necessária antes da compra

O tamanho da reserva deve basear-se na quantidade total de débito que irão utilizar os recursos do Azure Cosmos DB em breve-em--implementado ou existentes. É possível determinar a taxa de transferência necessária das seguintes formas:

* Obtenha os dados históricos para o débito aprovisionado total em suas contas do Azure Cosmos DB, bases de dados e coleções em todas as regiões. Por exemplo, pode avaliar o débito aprovisionado médio diário baixando o seu extrato de utilização diária do `https://account.azure.com`.

* Se for um cliente Enterprise Agreement (EA), pode transferir o ficheiro de utilização para obter os detalhes de débito do Azure Cosmos DB. Consulte a **tipo de serviço** valor no **informações adicionais** secção do ficheiro de utilização.

* Pode somar o débito médio para todas as cargas de trabalho em suas contas do Azure Cosmos DB que espera executar em seguida um ou três anos. Em seguida, pode usar essa quantidade para a reserva.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Comprar capacidade reservada do Azure Cosmos DB

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  

2. Selecione **todos os serviços** > **reservas** > **adicionar**.  

3. Do **selecionar o tipo de produto** painel, escolha **do Azure Cosmos DB** > **selecionar** comprar uma reserva de novo.  

4. Preencha os campos obrigatórios, conforme descrito na tabela a seguir:

   ![Preencha o formulário de capacidade de reserva](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Campo  |Descrição  |
   |---------|---------|
   |Nome   |    Nome da reserva. Este campo é preenchido automaticamente com `CosmosDB_Reservation_<timeStamp>`. Pode fornecer um nome diferente ao criar a reserva. Ou pode alterá-lo Depois da reserva é criada.      |
   |Subscrição  |   Capacidade de reserva de subscrição que é utilizada para pagar o Azure Cosmos DB. O método de pagamento na subscrição selecionada é utilizado na cobrar os custos à cabeça. O tipo de subscrição tem de ser um dos seguintes procedimentos: <br/><br/>  [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (número da oferta: MS-AZR - 0017 P): para uma subscrição Enterprise, os encargos são deduzidos da saldo de alocação monetária a inscrição ou cobrados como utilização excedente. <br/><br/> [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) (número da oferta: MS-AZR - 0003p): para uma subscrição pay as you go, os custos são faturados para o método de pagamento do cartão de crédito ou por fatura na subscrição.    |
   |Âmbito   |   Opção que controla o número de subscrições, pode utilizar o benefício de faturação associado com a reserva. Este também controla a forma como a reserva se aplica às subscrições específicas.   <br/><br/>  Se selecionou **subscrição individual**, o desconto de reserva é aplicado às instâncias do Azure Cosmos DB na subscrição selecionada. <br/><br/>  Se selecionou **partilhado**, o desconto de reserva é aplicado às instâncias do Azure Cosmos DB, que são executados em qualquer subscrição com o contexto de faturação. O contexto de faturação é baseado na forma como se inscreveu no Azure. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.  <br/><br/> Pode alterar o âmbito de reserva depois de comprar a capacidade de reserva.  |
   |Tipo de capacidade de reserva   |  Débito aprovisionado como unidades de pedido.|
   |Unidades de capacidade de reserva  |      Quantidade de débito que pretende reservar. Pode calcular esse valor por determinar o débito necessário para todos os seus recursos de Cosmos DB (por exemplo, bancos de dados ou contentores) por região. , Em seguida, multiplique isso pelo número de regiões que irá associar-se a sua base de dados do Cosmos DB.  <br/><br/> Por exemplo: Se tiver cinco regiões com 1 milhão RU/seg em cada região, selecione 5 milhões de RU/seg para a compra de capacidade de reserva.    |
   |Termo  |   Um ano ou três anos.   |

5. Reveja o desconto e o preço da reserva no **custos** secção. Este preço de reserva se aplica aos recursos do Azure Cosmos DB com o débito aprovisionado em todas as regiões.  

6. Selecione **Comprar**. Quando a compra for concluída com êxito, verá a seguinte página: 

   ![Preencha o formulário de capacidade de reserva](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Depois de comprar uma reserva, ele é aplicado imediatamente a quaisquer recursos existentes do Azure Cosmos DB que corresponda aos termos da reserva. Se não tiver quaisquer recursos existentes do Azure Cosmos DB, a reserva aplicará ao implementar uma nova instância de Cosmos DB, que corresponde aos termos da reserva. Em ambos os casos, o período da reserva começa imediatamente após a compra ser efetuada com êxito. 

Quando a reserva expirar, as instâncias do Azure Cosmos DB continuam a ser executado e são faturadas às tarifas pay as you go normais.

## <a name="next-steps"></a>Passos Seguintes

O desconto de reserva é aplicado automaticamente para os recursos do Azure Cosmos DB que correspondem à âmbito de reserva e atributos. Pode atualizar o âmbito da reserva através do portal do Azure, PowerShell, CLI do Azure ou a API.

*  Para saber a capacidade de como reserva se aplica qualquer desconto ao Azure Cosmos DB, veja [compreender o desconto de reserva do Azure](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

   * [Quais são as reservas do Azure?](../billing/billing-save-compute-costs-reservations.md)  
   * [Gerir reservas do Azure](../billing/billing-manage-reserved-vm-instance.md)  
   * [Compreender a utilização de reserva para inscrição da sua empresa](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Compreender a utilização de reserva para a sua subscrição pay as you go](../billing/billing-understand-reserved-instance-usage.md)
   * [Reservas do Azure no programa CSP do Centro de parceiros](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Precisa de ajuda? Contacte-nos.

Se tiver alguma dúvida ou precisar de ajuda, [criar um pedido de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

