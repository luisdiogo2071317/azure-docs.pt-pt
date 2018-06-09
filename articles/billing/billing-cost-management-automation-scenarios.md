---
title: Cenários de automatização da gestão de custos e faturação do Azure | Microsoft Docs
description: Saiba como comuns faturação e de custo gestão cenários são APIs mapeadas para diferentes.
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
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: f84071577e9636e40d621093e2c57e3e9adf4913
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247671"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Cenários de automatização de gestão de faturação e de custo

Cenários comuns para o espaço de gestão de faturação e de custo são abaixo identificados e mapeados para diferentes APIs que pode ser utilizadas nesses cenários. É possível encontrar um resumo de todas as APIs disponíveis e a funcionalidade oferecem por baixo de cenário para mapeamento de API. 

## <a name="common-scenarios"></a>Cenários comuns 

Pode utilizar a faturação e APIs numa variedade de cenários de custo de resposta e utilização do custo de gestão relacionados com perguntas.  Uma descrição dos cenários comuns é fornecida abaixo.

- **Fatura reconciliação** -Microsoft foi cobram-me a quantidade certa?  O que é a minha fatura e pode posso calculá-la pessoalmente?

- **Entre os encargos** - agora que deve saber quanto posso estou a ser cobrado, quem é que na minha organização tem de pagar?

- **Otimização de custos** -poderei sabê-lo quanto posso já foi-lhe cobrado, no entanto, como posso obter mais informações sobre o dinheiro estou a dedicar no Azure?

- **Controlo de custos** – quero ver quanto posso estou a dedicar e utilizar o Azure ao longo do tempo. Quais são as tendências? Como foi posso fazer melhor?

- **Azure gastam durante o mês** – quantidade é meu mês atual do gastam até à data? É necessário fazer ajustamentos na minha os gastos com e/ou utilização do Azure? Quando durante o mês estou consigo consumir Azure mais?

- **Configure alertas** – gostaria de configurar o consumo de recursos ou monetário com base em alertas.

## <a name="scenario-to-api-mappings"></a>Cenário para mapeamentos de API

|         API/cenário        | Reconciliação da fatura    | Entre os encargos    | Otimização de custos    | Controlo de custos    | Mid mês gastam    | Alertas    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Orçamentos                     |                           |                  |           X          |                  |                    |     X     |
| Marketplaces                |             X             |         X        |           X          |         X        |          X         |     X     |
| Folha de Preços                 |             X             |         X        |           X          |         X        |          X         |           |
| Recomendações de reserva |                           |                  |           X          |                  |                    |           |
| Detalhes de reserva         |                           |                  |           X          |         X        |                    |           |
| Resumos de reserva       |                           |                  |           X          |         X        |                    |           |
| Detalhes de Utilização               |             X             |         X        |           X          |         X        |          X         |     X     |
| Períodos de faturação             |             X             |         X        |           X          |         X        |                    |           |
| Faturas                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Utilização não classificada               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> O cenário para mapeamentos de API abaixo não incluem as APIs de consumo de empresa. Sempre que possível,. utilize as APIs de consumo geral para abordar cenários de desenvolvimento de novas net daqui para a frente.

## <a name="api-summaries"></a>Resumos de API

### <a name="consumption"></a>Consumo
(*Direta web + aos clientes empresariais para todos os APIs exceto esses chamado terminar abaixo*)

-   **Orçamentos** (*que os clientes empresariais só*): Utilize o [orçamentos API](https://docs.microsoft.com/rest/api/consumption/budgets) criar orçamentos de custos ou a utilização de recursos, grupos de recursos ou faturação medidores.  Assim que tiver criado orçamentos, o alerta pode ser configurado para enviar notificações quando definidas pelo utilizador orçamento limiares forem ultrapassados. Também podem ser configuradas ações a adotar quando quantidades de orçamento são atingidas.
-   **Marketplaces**: Utilize o [Marketplace encargos API](https://docs.microsoft.com/rest/api/consumption/marketplaces) para obter dados de utilização e encargos em todos os recursos de mercado (Azure 3rd ofertas de terceiros). Estes dados podem ser utilizados para adicionar os custos de cópia de segurança em todos os recursos de mercado ou investigar os custos/utilização de recursos específicos.
-   **Folha de preços** (*que os clientes empresariais só*): os clientes empresariais podem utilizar o [API de folha de preços](https://docs.microsoft.com/rest/api/consumption/pricesheet) para obter os seus preços personalizada para todos os medidores. As empresas podem utilizar estes dados em combinação com informações de utilização de detalhes e marketplaces de utilização para efetuar cálculos de custo utilizando dados de utilização e marketplace. 
-   **Recomendações de reserva**: Utilize o [reserva recomendações API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) para obter recomendações de aquisição VM reservado instâncias. Recomendações foram concebidas para permitir que os clientes analisar reduções de custos esperado e adquirir quantidades.
-   **Detalhes de reserva**: Utilize o [reserva detalhes API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) para ver informações sobre anteriormente adquiridas reservas de VM, tais como a quantidade consumo foi reservado por oposição aos que está realmente a ser utilizado. Pode ver os dados em detalhe per-ao nível da VM.
-   **Reserva resumos**: Utilize o [reserva resumos API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) para ver informações de agregação no anteriormente adquiridas reservas de VM, tais como a quantidade consumo foi reservado por oposição aos que está realmente a ser utilizado no agregado. 
-   **Detalhes de utilização**: Utilize o [API de detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails) para obter gratuitamente e utilização no Azure todos os recursos de 1ª de terceiros. Informações estão no formato de registos de detalhes de utilização, atualmente são emitidos uma vez por medição por dia. Informações podem ser utilizadas para adicionar os custos de cópia de segurança em todos os recursos ou investigar os custos/utilização de recursos específicos.
-   **RateCard**: os clientes Web direto podem utilizar o [RateCard API](https://msdn.microsoft.com/library/azure/mt219005.aspx) para obter as taxas de medição. Em seguida, podem utilizar as informações devolvidas com as suas informações de utilização de recursos para calcular manualmente fatura esperada. 
-   **Não classificados utilização**: pode utilizar o [não classificados de utilização de API](https://msdn.microsoft.com/library/azure/mt219003.aspx) para obter informações de utilização não processados, antes de qualquer medição/charging efetuada pelo Azure.

### <a name="billing"></a>Faturação
-   **Períodos de faturação**: Utilize o [API de períodos de faturação](https://docs.microsoft.com/rest/api/billing/billingperiods) para determinar um período de faturação para analisar, juntamente com a fatura IDs durante esse período. Fatura IDs podem ser utilizadas com a API da fatura abaixo. 
-   **Faturas**: Utilize o [faturas API](https://docs.microsoft.com/rest/api/billing/invoices) para obter o URL de transferência para uma fatura para um determinado período de faturação no formato PDF.

### <a name="enterprise-consumption"></a>Consumo de Enterprise
*(Todas as APIs Enterprise apenas)*

-   **Saldo resumo**: Utilize o [API de resumo de saldo](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) para obter um mensal resumo das informações sobre saldos, compras novo, encargos de serviço do Azure Marketplace, ajustes e custos excedidos. Pode obter estas informações para o período de faturação atual ou qualquer período no passado. As empresas podem utilizar estes dados para executar uma comparação com encargos resumos manualmente calculadas. Esta API não fornece informações específicas do recurso ou uma vista de agregação dos custos.
-   **Detalhes de utilização**: Utilize o [API de detalhes de utilização](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) para obter informações de detalhe de utilização do Azure para o mês atual, um período específico de faturação ou um período de datas personalizada de terceiros 1ª. As empresas podem utilizar estes dados manualmente calcular fatura com base na velocidade e o consumo e também podem utilizar as informações de departamento/organização presentes para os custos de atributo entre organizações. Os dados fornece uma vista de recurso específicos de utilização/custo.
-   **Encargos de arquivo de Marketplace**: Utilize o [API de encargos de arquivo de Marketplace](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) para obter informações de detalhe de utilização do Azure para o mês atual, um período específico de faturação ou um período de datas personalizada de terceiros 3rd. As empresas podem utilizar estes dados manualmente calcular fatura com base na velocidade e o consumo e também podem utilizar as informações de departamento/organização presentes para os custos de atributo entre organizações. A taxa de arquivo de Marketplace API fornece uma vista de recurso específicos de utilização/custo.
-   **Folha de preços**: O [API de folha de preços](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) fornece a taxa de aplicável para cada medidor para a inscrição e a faturação período indicado. Estas informações de velocidade podem ser utilizadas em combinação com informações de utilização de detalhes e marketplaces de utilização para calcular manualmente fatura esperada.
-   **Períodos de faturação**: Utilize o [API de períodos de faturação](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) para obter uma lista de períodos juntamente com uma propriedade de apontar para a rota de API para os quatro conjuntos de dados de API de Enterprise que pertençam a esse período de faturação - BalanceSummary, de faturação UsageDetails, os encargos de Marketplace e folha de preços.
-   **Reservado recomendações de instância**: O [reservado API de recomendações de instância](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) analisa de 7 dias do cliente, 30 dias, ou 60 dias da utilização de máquina virtual e oferece recomendações único e compra partilhado. A instância reservada que API permite aos clientes analisar esperado reduções de custos e recomendado quantidades de compra.

## <a name="next-steps"></a>Próximos Passos

- Para obter informações sobre como utilizar as APIs de faturação do Azure para programaticamente conhecer a utilização do Azure, consulte [Azure faturação descrição geral da API](billing-usage-rate-card-overview.md).
