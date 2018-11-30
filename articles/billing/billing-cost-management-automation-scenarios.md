---
title: A faturação do Azure e cenários de automação de gestão de custos | Documentos da Microsoft
description: Saiba como comuns faturação e são de cenários de gestão de custos APIs mapeadas para diferentes.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/13/2018
ms.author: erikre
ms.openlocfilehash: 09ecd46837a12f5f1a8278b1644dc099701bcd00
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584728"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Cenários de automação de gestão de faturação e de custo

Cenários comuns para o espaço de gestão de faturação e de custo são identificados abaixo e mapeados para diferentes APIs que podem ser usadas nesses cenários. Um resumo de todas as APIs disponíveis e a funcionalidade que oferecem pode ser encontrado abaixo o cenário para o mapeamento de API. 

## <a name="common-scenarios"></a>Cenários comuns

Pode usar a faturação e gestão de custos APIs numa variedade de cenários para os custos de resposta e utilização relacionados com a perguntas.  Uma descrição dos cenários comuns é fornecida abaixo.

- **Reconciliação de nota fiscal** -Microsoft fez-me cobrada a quantidade certa?  O que é a minha fatura e pode eu calculo-lo eu mesmo?

- **Cross custos** - agora que sei quanto eu estou a ser cobrado, quem na minha organização tem de pagar?

- **Otimização de custos** -sei quanto eu tenha sido cobrado, no entanto, como posso obter mais o dinheiro estou a passar no Azure?

- **Controlo de custos** – quero ver quanto eu estou gastos e a utilizar o Azure ao longo do tempo. Quais são as tendências? Como foi que estar fazendo melhor?

- **Gastos do Azure durante o mês** – quanto meu mês atual é de gastar até à data? É necessário fazer os ajustes na minha gastos e/ou utilização do Azure? Quando durante o mês estou que consumindo Azure mais?

- **Configure alertas** – eu gostaria de configurar a recursos com base no consumo ou monetário com base em alertas.

## <a name="scenario-to-api-mappings"></a>Cenário para mapeamentos de API

|         API/cenário        | Reconciliação de nota fiscal    | Entre os custos    | Otimização de custos    | Controlo de custos    | Mid mês gastar    | Alertas    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Orçamentos                     |                           |                  |           X          |                  |                    |     X     |
| Mercados                |             X             |         X        |           X          |         X        |          X         |     X     |
| Folha de Preços                 |             X             |         X        |           X          |         X        |          X         |           |
| Recomendações de reserva |                           |                  |           X          |                  |                    |           |
| Detalhes da reserva         |                           |                  |           X          |         X        |                    |           |
| Resumos de reserva       |                           |                  |           X          |         X        |                    |           |
| Detalhes de utilização               |             X             |         X        |           X          |         X        |          X         |     X     |
| Períodos de faturação             |             X             |         X        |           X          |         X        |                    |           |
| Faturas                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Utilização não classificada               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> O cenário para mapeamentos de API abaixo não incluem as APIs de consumo da empresa. Sempre que possível, use a opção as APIs de consumo geral para abordar cenários de desenvolvimento de novos net no futuro.

## <a name="api-summaries"></a>Resumos de API

### <a name="consumption"></a>Consumo
(*Web Direct + os clientes empresariais para todas as APIs exceto essas chamadas fora abaixo*)

-   **Orçamentos** (*apenas a clientes empresariais*): Utilize o [orçamentos API](https://docs.microsoft.com/rest/api/consumption/budgets) para criar os orçamentos de custos ou utilização de recursos, grupos de recursos ou medidores de faturação.  Depois de criar orçamentos, alertas podem ser configurados para notificar quando definidas pelo utilizador orçamento limiares forem ultrapassados. Ações também podem ser configuradas para ocorrer quando são atingidas quantidades de orçamento.
-   **Mercados**: Utilize o [API de encargos do Marketplace](https://docs.microsoft.com/rest/api/consumption/marketplaces) para obter dados de utilização e custos em todos os recursos do Marketplace (Azure 3º ofertas de terceiros). Estes dados podem ser utilizados para aumentar os custos em todos os recursos do Marketplace ou investigar os custos/utilização em recursos específicos.
-   **Folha de preços** (*apenas a clientes empresariais*): os clientes empresariais podem utilizar os [API da folha de preços](https://docs.microsoft.com/rest/api/consumption/pricesheet) para recuperar seus preços personalizados para todos os medidores. As empresas podem utilizar estes dados em combinação com informações de utilização de detalhes e mercados de utilização para realizar cálculos de custo com dados de utilização e o marketplace. 
-   **Recomendações de reserva**: Utilize o [API de recomendações de reserva](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) obter recomendações para a compra de instâncias de VM reservadas. Recomendações destinam-se para permite aos clientes analisar economias de custo previsto e quantidades de compra.
-   **Detalhes da reserva**: Utilize o [API de detalhes de reserva](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) para ver informações sobre anteriormente adquiridas reservas de VM, como a quantidade de consumo foi reservado versus quanto está realmente a ser utilizado. Pode ver dados per-VM-nível de detalhe.
-   **Resumos de reserva**: Utilize o [API de resumos de reserva](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) para ver informações agregadas sobre anteriormente adquiridas reservas de VM, como a quantidade de consumo foi reservado versus quanto está realmente a ser utilizado no agregado. 
-   **Detalhes de utilização**: Utilize o [API de detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails) para obter o custo e a utilização no Azure todos os recursos de terceiros 1. Informações estão sob a forma de registos de detalhes de utilização, que atualmente são emitidos uma vez por medidor por dia. Informações podem ser utilizadas para aumentar os custos em todos os recursos ou investigar os custos/utilização em recursos específicos.
-   **RateCard**: os clientes Web Direct podem utilizar o [API de RateCard](https://msdn.microsoft.com/library/azure/mt219005.aspx) para obter suas taxas de medidor. Em seguida, podem utilizar as informações retornadas com suas informações de utilização de recursos para calcular manualmente fatura. 
-   **Não classificados de utilização**: pode utilizar o [API de utilização não classificados](https://msdn.microsoft.com/library/azure/mt219003.aspx) para obter informações de utilização não processados, antes de qualquer medição/cobrar efetuada pelo Azure.

### <a name="billing"></a>Faturação
-   **Períodos de faturação**: Utilize o [API de períodos de faturação](https://docs.microsoft.com/rest/api/billing/billingperiods) para determinar um período de faturação para analisar, juntamente com a nota fiscal IDs durante esse período. Nota fiscal IDs podem ser utilizadas com a API de nota fiscal abaixo. 
-   **Notas fiscais**: Utilize o [API de notas fiscais](https://docs.microsoft.com/rest/api/billing/invoices) para obter o URL de transferência para uma nota fiscal de um determinado período de faturação no formato PDF.

### <a name="enterprise-consumption"></a>Consumo de Enterprise
*(Todas as APIs de empresas apenas)*

-   **Resumo do saldo**: Utilize o [API de resumo do saldo](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) para obter um mensal resumo de informações sobre os saldos, novas compras, os encargos de serviços do Azure Marketplace, ajustes e custos de utilização excedida. Pode obter estas informações para o período de faturação atual ou qualquer período no passado. As empresas podem utilizar estes dados para executar uma comparação com custos de resumos manualmente calculadas. Esta API não fornece informações específicas de recurso ou uma visualização agregada dos custos.
-   **Detalhes de utilização**: Utilize o [API de detalhes de utilização](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) para obter informações de detalhes de utilização do Azure para o mês atual, de um determinado período de faturação ou de um período de datas personalizadas de terceiros 1. As empresas podem utilizar estes dados para calcular manualmente fatura com base numa taxa de e no consumo e também podem utilizar as informações de organização/departamento presentes para os custos de atributo entre organizações. Os dados fornecem uma exibição de recursos específicos de utilização/custo.
-   **Custos de armazenamento do Marketplace**: Utilize o [Marketplace Store custos API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) para obter informações de detalhes de utilização do Azure para o mês atual, de um determinado período de faturação ou de um período de datas personalizadas de terceiros 3. As empresas podem utilizar estes dados para calcular manualmente fatura com base numa taxa de e no consumo e também podem utilizar as informações de organização/departamento presentes para os custos de atributo entre organizações. O custo de armazenamento do Marketplace API fornece uma exibição de recursos específicos de utilização/custo.
-   **Folha de preços**: A [API da folha de preços](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) fornece a taxa aplicável para cada contador para a inscrição e o período de faturação determinado. Estas informações de taxa podem ser utilizadas em combinação com informações de utilização de detalhes e mercados de utilização para manualmente calcular fatura.
-   **Períodos de faturação**: Utilize o [API de períodos de faturação](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) para obter uma lista de períodos, juntamente com uma propriedade que aponta para a rota de API para os quatro conjuntos de dados empresariais de API que dizem respeito a nesse período de faturação - BalanceSummary, de faturação UsageDetails encargos do Marketplace e folha de preços.
-   **Recomendações de reserva do Azure**: A [API de recomendações de instância reservada](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) aborda 7 dias do cliente, 30 dias, ou de 60 dias de utilização da máquina virtual e oferece recomendações de compra partilhados e únicas. A instância reservada que API permite aos clientes analisar esperado poupanças de custos e recomendado quantidades de compra.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-is-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>O que é a diferença entre as APIs de relatórios empresariais e as APIs de consumo? Quando devo utilizar cada?
Essas APIs têm um conjunto de funcionalidades semelhantes e podem responder o mesmo conjunto amplo de perguntas no espaço de gestão de faturação e de custo. No entanto, cada API destina-se a públicos diferentes: 

- **APIs de relatórios empresariais**: essas APIs estão disponíveis para os clientes que iniciaram um Enterprise Agreement com a Microsoft que lhe concede acesso a preços personalizados e de compromissos monetários negociados. As APIs requerem uma chave para utilizar que pode ser obtida através da [Enterprise Portal](https://ea.azure.com). Para obter uma descrição destas APIs, veja [descrição geral de APIs de relatórios para os clientes empresariais](billing-enterprise-api.md).

- **Consumo de APIs**: essas APIs estão disponíveis para todos os clientes, com algumas exceções. Para obter mais informações, consulte [descrição geral da API de consumo do Azure](billing-consumption-api-overview.md) e o [referência da API de consumo do Azure](https://docs.microsoft.com/rest/api/consumption/). As APIs fornecidas são a solução recomendada para os cenários de desenvolvimento mais recentes. 

### <a name="what-is-the-difference-between-the-usage-details-api-and-the-usage-api"></a>O que é a diferença entre a API de detalhes de utilização e a API de utilização?
Estas APIs fornecem dados fundamentalmente diferentes:

- **Detalhes de utilização**: A [API de detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails) fornece Azure informações de utilização e custo por instância de medidor. Os dados fornecidos já transmitido através do custo do Azure, sistema de medição e tinha Custo aplicado a ele, juntamente com outras possíveis alterações:

    - Alterações para levar em conta a utilização de compromissos monetários pré-paga
    - Alterações para levar em conta as discrepâncias de utilização detetadas pelo Azure

- **Utilização**: A [API de utilização](https://msdn.microsoft.com/library/Mt219003.aspx) fornece informações de utilização do Azure não processados antes de ele passa através do Azure do sistema de medição de custos. Estes dados podem não ter qualquer correlação com a quantidade de utilização e/ou custo que é vista depois do Azure cobra o sistema de medição.

### <a name="what-is-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>O que é a diferença entre a API de nota fiscal e a API de detalhes de utilização?
Essas APIs proporcionam uma exibição diferente dos mesmos dados. O [API de nota fiscal](https://docs.microsoft.com/rest/api/billing/invoices) destina-se somente a clientes Web Direct e fornece uma implementação mensal cópia de segurança da fatura com base nos custos de agregação para cada tipo de medidor. O [API de detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails) fornece uma vista granular dos registos de utilização/custo para cada dia e pode ser utilizado por clientes empresariais e de Web Direct.

### <a name="what-is-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>O que é a diferença entre a API da folha de preços e a API de RateCard?
Estas APIs fornecem semelhante conjuntos de dados, mas têm a públicos diferentes. Informações abaixo.

- API da folha de preços: Os [API da folha de preços](https://docs.microsoft.com/rest/api/consumption/pricesheet) fornece o personalizado preços que tem sido negociado para um cliente empresarial.
- API de RateCard: Os [RateCard API](https://msdn.microsoft.com/library/mt219005.aspx) fornece o destinados ao público preços que se aplica aos clientes Web Direct.

## <a name="next-steps"></a>Próximos Passos

- Para obter informações sobre como utilizar as APIs do Azure para obter informações sobre sua utilização do Azure programaticamente, consulte [descrição geral da API de consumo do Azure](billing-consumption-api-overview.md) e [descrição geral da API de faturação do Azure](billing-usage-rate-card-overview.md).
- Para comparar a fatura com o ficheiro de utilização diária detalhadas e os relatórios de gestão de custos no portal do Azure, consulte [compreender a sua fatura do Microsoft Azure](billing-understand-your-bill.md)
- Se tiver alguma dúvida ou precisar de ajuda, [criar um pedido de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
