---
title: Efetue o pré-pagamento do recursos do Azure Cosmos DB poupar dinheiro | Documentos da Microsoft
description: Saiba como comprar capacidade do Azure Cosmos DB reservadas para poupar nos custos de computação.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 1be2d67d8a1ee51c4883ae1f50b80ad3a9691c2d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981972"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Efetue o pré-pagamento do recursos do Azure Cosmos DB com a capacidade de reserva

O Azure Cosmos DB reservado ajuda de capacidade que poupar dinheiro ao pagar previamente para recursos do Azure Cosmos DB durante um período de um ano ou três anos. Capacidade de Cosmos DB reservada do Azure permite-lhe obter um desconto sobre o débito aprovisionado para recursos do Cosmos DB, por exemplo, bancos de dados, contentores (coleções/tabelas/gráficos). Capacidade de Cosmos DB reservada do Azure pode reduzir significativamente os custos de Cosmos DB — até 65% em preços regulares – compromisso inicial de um ano ou três anos. Capacidade de reserva fornece um desconto de faturação e não afeta o estado de tempo de execução dos seus recursos do Cosmos DB.

Capacidade de Cosmos DB reservada do Azure abrange o débito aprovisionado para os seus recursos, não abrange os custos de armazenamento e rede. Assim que adquirir uma reserva, os custos de débito que correspondem a reserva atributos já não são cobrados a pay as you go taxas. Para obter mais informações sobre as reservas, consulte [reservas Azure](../billing/billing-save-compute-costs-reservations.md) artigo. 

Pode comprar o Azure Cosmos DB reservado capacidade dos [portal do Azure](https://portal.azure.com). Para comprar uma capacidade de reserva:

* Tem de estar na função de proprietário para, pelo menos, uma empresa ou uma subscrição pay as you go.  
* Para subscrições Enterprise, compras de reserva do Azure tem de estar ativadas no [portal EA.](https://ea.azure.com/)  
* Para o programa de fornecedor de soluções Cloud (CSP), apenas os agentes de administrador ou agentes de vendas podem comprar capacidade reservada do Azure Cosmos DB.

## <a name="determine-the-required-throughput-before-purchase"></a>Determinar a taxa de transferência necessária antes da compra

O tamanho da reserva deve basear-se na quantidade total de débito utilizado pelos recursos de Azure Cosmos DB existentes ou em breve-em--implementado (por exemplo, bancos de dados ou de contentores - coleções, tabelas, gráficos). É possível determinar a taxa de transferência necessária das seguintes formas:

* Navegue para [Portal do Azure](https://portal.azure.com), localize a conta do Azure Cosmos DB, abra o painel de métricas e obtenha os detalhes de média de débito por segundo da **débito** separador durante um período de 3 a 6 meses. Forneça este tamanho como as unidades de capacidade de reserva ao comprar.

Em alternativa, se for um Enterprise Agreement (EA), pode transferir o ficheiro de utilização e consultar **tipo de serviço** valor no **informações adicionais** secção do ficheiro de utilização para obter o Azure Cosmos DB detalhes de débito.

Também pode somar o débito médio para todas as cargas de trabalho em suas contas do Azure Cosmos DB, que prevê em execução para os em seguida um ou três anos e utilizar essa quantidade de reserva.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Comprar capacidade reservada do Azure Cosmos DB

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  

2. Selecione **todos os serviços** > **reservas** > **adicionar**.  

3. Do **selecionar o tipo de produto** painel, escolha **do Azure Cosmos DB**e, em seguida **selecionar** para comprar uma reserva nova.  

4. Preencha os campos obrigatórios, conforme descrito na tabela abaixo:

   ![Preencha o formulário de capacidade de reserva](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Campo  |Descrição  |
   |---------|---------|
   |Nome   |    Nome da reserva. Este campo é preenchido automaticamente com o `CosmosDB_Reservation_<timeStamp>`. Pode fornecer um nome diferente ao criar a reserva ou renomeá-lo Depois da reserva é criada.      |
   |Subscrição  |   A subscrição utilizada para pagar a capacidade de reservada do Azure Cosmos DB. O método de pagamento na subscrição selecionada é utilizado na cobrar os custos à cabeça. O tipo de subscrição tem de ser um dos seguintes procedimentos: <br/><br/>  [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (número da oferta: MS-AZR - 0017 P) – para uma subscrição enterprise, os encargos são deduzidos da saldo de alocação monetária a inscrição ou cobrados como utilização excedente. <br/><br/> [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) (número da oferta: MS-AZR - 0003p) – para a subscrição pay as you go, os custos são faturados para o método de pagamento do cartão de crédito ou por fatura na subscrição.    |
   |Âmbito   |   Âmbito da reserva controla o número de subscrições pode tirar partido do benefício de faturação com a reserva e controlam como a reserva é aplicada às subscrições específicas. Uma reserva tem um âmbito da subscrição única ou partilhado. Se selecionar:   <br/><br/>  **Subscrição individual** -o desconto de reserva é aplicado às instâncias do Azure Cosmos DB na subscrição selecionada. <br/><br/>  **Partilhado** – o desconto de reserva é aplicado às instâncias do Azure Cosmos DB com qualquer subscrição dentro de seu contexto de faturação. O contexto de faturação é determinado com base na forma como se inscreveu no Azure. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.  <br/><br/> Pode alterar o âmbito de reserva após a compra a capacidade de reserva.  |
   |Tipo de capacidade reservada   |  Tipo de capacidade de reserva é o débito aprovisionado em termos de unidades de pedido.|
   |Unidades de capacidade reservadas  |      A quantidade de débito que pretende reservar. Pode calcular esse valor por determinar o débito necessário para todos os seus recursos do Cosmos DB (por exemplo, bancos de dados ou contentores) por região e, em seguida, multiplique isso pelo número de regiões que vai associar à sua base de dados do Cosmos DB.  <br/> Por exemplo: Se tiver cinco regiões com 1 milhão RU/seg em cada região, em seguida, selecione 5 milhões de RU/seg para compra de capacidade de reserva.    |
   |Termo  |   Um ano ou três anos.   |

5. Reveja o desconto e o preço da reserva no **custos** secção. Este preço de reserva se aplica aos recursos do Azure Cosmos DB com o débito aprovisionado em todas as regiões.  

6. Selecione **Comprar**. Pode ver a captura de ecrã seguinte quando a compra for concluída com êxito. 

   ![Preencha o formulário de capacidade de reserva](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Depois de comprar uma reserva, ele será aplicado imediatamente a quaisquer recursos existentes do Azure Cosmos DB que corresponda aos termos da reserva. Se não tiver quaisquer recursos existentes do Azure Cosmos DB, a reserva aplicará ao implementar uma nova instância de Cosmos DB, que corresponde aos termos da reserva. Em ambos os casos, o período da reserva começa imediatamente após a compra ser efetuada com êxito. 

Quando a reserva expirar, as instâncias do Azure Cosmos DB continuarão a ser executado e são faturadas às tarifas pay as you go normais.

## <a name="next-steps"></a>Passos Seguintes

O desconto de reserva é aplicado automaticamente para os recursos do Azure Cosmos DB que correspondem à âmbito de reserva e atributos. Pode atualizar o âmbito da reserva através do portal do Azure, PowerShell, CLI, ou por meio da API.

*  Para saber a capacidade de como reserva se aplica qualquer desconto ao Azure Cosmos DB, veja [compreender o Azure reservas de desconto](../billing/billing-understand-cosmosdb-reservation-charges.md)

* Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

   * [Quais são as reservas do Azure?](../billing/billing-save-compute-costs-reservations.md)  
   * [Gerir reservas Azure](../billing/billing-manage-reserved-vm-instance.md).  
   * [Compreender a utilização de reserva para inscrição da sua empresa](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Compreender a utilização de reserva para a sua subscrição pay as you go](../billing/billing-understand-reserved-instance-usage.md)
   * [Reservas do Azure no programa de fornecedor de soluções (CSP) do parceiro Center na nuvem](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.

