---
title: Descrição geral de consumo do Azure API | Microsoft Docs
description: Saiba como APIs de consumo do Azure dão-lhe acesso programático para custo e os dados de utilização para os seus recursos do Azure.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 68825e85-de64-466d-b11a-8baffde836b5
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: 69e148089f48ebe04faceff87b08eb3f7127f8a3
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248372"
---
# <a name="azure-consumption-api-overview"></a>Descrição geral de consumo do Azure API 

As APIs de consumo do Azure dão-lhe acesso programático para dados e de utilização para os seus recursos do Azure. Estas APIs atualmente apenas suportam inscrições de Enterprise e subscrições direta Web (com algumas exceções). As APIs são continuamente atualizadas para suportar outros tipos de subscrições do Azure.

APIs de consumo do Azure fornecem acesso a:
- Enterprise e Web Direcionam clientes 
    - Detalhes de Utilização 
    - Custos do Marketplace 
    - Recomendações de reserva 
    - Detalhes de reserva 
    - Resumos de reserva 
- Apenas a clientes empresariais 
    - Folha de preços 
    - Orçamentos 
    - Equilibra 

## <a name="usage-details-api"></a>Detalhes de utilização API

Utilizar a API de detalhes de utilização para obter dados de encargos e a utilização do Azure todos os recursos de 1ª de terceiros. Informações estão no formato de registos de detalhes de utilização que atualmente são emitidos uma vez por medição por recurso por dia. Informações podem ser utilizadas para adicionar os custos de cópia de segurança em todos os recursos ou investigar os custos / utilização de recursos específicos. 

A API inclui:

-   **Os dados de consumo de nível de medidor** - os dados de ver, incluindo a utilização de custos, o medidor emitir encargos, e os recursos do Azure a taxa diz respeito ao. Todos os registos de detalhes de utilização mapeiam para um registo diariamente.
-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Filtragem** -compactar o conjunto para baixo para um conjunto de registos de detalhes de utilização utilizando os seguintes filtros mais pequeno de resultados de API:
    - Fim de utilização / iniciar de utilização
    - Grupo de Recursos
    - Nome do Recurso
-   **Agregação de dados** -OData de utilização para aplicar as expressões para detalhes de utilização de agregação por etiquetas ou filtrar as propriedades
-   **Utilização de tipos de oferta diferentes** -informações de detalhe de utilização estão atualmente disponíveis para os clientes empresariais e Web direta.

Para obter mais informações, consulte a especificação técnica para o [API de detalhes de utilização](https://docs.microsoft.com/rest/api/consumption/usagedetails).

## <a name="marketplace-charges-api"></a>Custos de Marketplace API

Utilizar a API de encargos de Marketplace para obter dados de utilização e encargos em todos os recursos de mercado (Azure 3rd ofertas de terceiros). Estes dados podem ser utilizados para adicionar os custos de cópia de segurança em todos os recursos de mercado ou investigar os custos / utilização de recursos específicos. 

A API inclui:

-   **Os dados de consumo de nível de medidor** - os dados de ver, incluindo a utilização de marketplace custo, a medição emitir encargos, e que recursos os encargos diz respeito ao. Todos os registos de detalhes de utilização mapeiam para um registo diariamente.
-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Filtragem** -compactar o conjunto para baixo para um conjunto de registos do marketplace utilizando os seguintes filtros mais pequeno de resultados de API:
    - Início de utilização / fim de utilização
    - Grupo de Recursos
    - Nome do Recurso
-   **Utilização de tipos de oferta diferentes** -informações de Marketplace estão atualmente disponíveis para os clientes empresariais e Web direta.

Para obter mais informações, consulte a especificação técnica para o [Marketplace encargos API](https://docs.microsoft.com/rest/api/consumption/marketplaces).

## <a name="balances-api"></a>API de saldos

Os clientes empresariais podem utilizar a API equilibra para obter um mensal resumo das informações sobre saldos, compras novo, encargos de serviço do Azure Marketplace, ajustes e excedidos encargos. Pode obter estas informações para o período de faturação atual ou qualquer período no passado. As empresas podem utilizar estes dados para executar uma comparação com encargos resumos manualmente calculadas. Esta API não fornece informações específicas de recursos e uma vista de agregação dos custos. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Apenas os clientes de Enterprise** esta API está apenas disponíveis EA os clientes. 
    - Os clientes têm de ter permissões de administrador de empresa para chamar esta API 

Para obter mais informações, consulte a especificação técnica para o [saldos API](https://docs.microsoft.com/rest/api/consumption/getbalancesbybillingaccount).

## <a name="budgets-api"></a>Orçamentos API

Os clientes empresariais podem utilizar esta API para criar orçamentos de custos ou a utilização de recursos, grupos de recursos ou faturação medidores. Depois desta informação foi considerada, alertas podem ser configuradas a serem notificadas quando o orçamento definido pelo utilizador limiares forem ultrapassados. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Apenas os clientes de Enterprise** -esta API está apenas disponíveis EA os clientes.
-   **Notificações configuráveis** -especificar es para ser notificado quando a atribuição é tripped.
-   **Utilização ou custo com base orçamentos** -criar a sua atribuição com base no consumo ou custo conforme necessário ao seu cenário.
-   **Filtragem** -filtrar a atribuição a um subconjunto mais pequeno de recursos com os seguintes filtros configuráveis
    - Grupo de Recursos
    - Nome do Recurso
    - Medidor
-   **Períodos de tempo configurável orçamento** -especifique a frequência deve repor o orçamento e quanto o orçamento é válido para.

Para obter mais informações, consulte a especificação técnica para o [orçamentos API](https://docs.microsoft.com/rest/api/consumption/budgets).

## <a name="reservation-recommendations-api"></a>Recomendações de reserva API

Utilize esta API para obter recomendações de aquisição VM reservado instâncias. Recomendações foram concebidas para permitir que os clientes analisar reduções de custos esperado e adquirir quantidades. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Filtragem** -personalizar os resultados de recomendação utilizando os seguintes filtros:
    - Âmbito
    - Período de Lookback
-   **As informações de reserva para tipos de oferta diferentes** -informações de reserva estão atualmente disponíveis para os clientes empresariais e Web direta.

Para obter mais informações, consulte a especificação técnica para o [reserva recomendações API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations).

## <a name="reservation-details-api"></a>Detalhes de reserva API

Utilize a API de detalhes de reserva para ver informações sobre anteriormente adquiridas reservas de VM, tais como a quantidade consumo foi reservado por oposição aos que, na verdade, está a ser utilizado. Pode ver dados num por VM de nível de detalhe. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Filtragem** -compactar o resultado da API definido para baixo para um conjunto mais pequeno de reservas utilizando o seguinte filtro:
    - Intervalo de datas
-   **As informações de reserva para tipos de oferta diferentes** -informações de reserva estão atualmente disponíveis para os clientes empresariais e Web direta.

Para obter mais informações, consulte a especificação técnica para o [reserva detalhes API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails).

## <a name="reservation-summaries-api"></a>Reserva resumos API

Utilize esta API para ver informações de agregação no anteriormente adquiridas reservas de VM, tais como a quantidade consumo foi reservado versus quantidade realmente a ser utilizada no aggregate. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Filtragem** -personalizar os resultados ao utilizar a base diária com o seguinte filtro:
    - Data de Utilização
-   **As informações de reserva para tipos de oferta diferentes** -informações de reserva estão atualmente disponíveis para os clientes empresariais e Web direta.
-   **As agregações diárias ou mensais** – os chamadores podem especificar se pretendem os seus dados de resumo de reserva no intervalo de diário ou mensal.

Para obter mais informações, consulte a especificação técnica para o [reserva resumos API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries).

## <a name="price-sheet-api"></a>API de folha de preços
Cliente de empresa pode utilizar esta API para obter os seus preços personalizada para todos os medidores. As empresas podem utilizar esta em combinação com informações de utilização de detalhes e marketplaces de utilização para efetuar cálculos de custo utilizando dados de utilização e marketplace. 

A API inclui:

-   **Controlo de acesso baseado em funções do Azure** -configurar políticas de acesso no [portal do Azure](https://portal.azure.com), a [CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) ou [cmdlets Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) para especificar que os utilizadores ou aplicações podem obter acesso a dados de utilização da subscrição. Os chamadores tem de utilizar tokens padrão do Azure Active Directory para autenticação. Adicione o autor da chamada à função ou o leitor de faturação, leitor, proprietário ou contribuinte obter acesso aos dados de utilização de uma subscrição do Azure específica. 
-   **Apenas os clientes de Enterprise** -esta API está apenas disponíveis EA os clientes. Clientes direta Web devem utilizar a API de RateCard para obter preços. 

Para obter mais informações, consulte a especificação técnica para o [API de folha de preços](https://docs.microsoft.com/rest/api/consumption/pricesheet).

## <a name="scenarios"></a>Cenários

Seguem-se alguns dos cenários que são efetuados possíveis através de consumo de APIs:

-   **Fatura reconciliação** -Microsoft foi cobram-me a quantidade certa?  O que é a minha fatura e pode posso calculá-la pessoalmente?
-   **Entre os encargos** - agora que deve saber quanto posso estou a ser cobrado, quem é que na minha organização tem de pagar?
-   **Otimização de custos** -poderei sabê-lo quanto posso já foi-lhe cobrado... como posso obter mais informações sobre o dinheiro que estou a dedicar no Azure?
-   **Controlo de custos** -Quero ver quanto posso estou a dedicar e utilizar o Azure ao longo do tempo. Quais são as tendências? Como foi posso fazer melhor?
-   **Azure gastam durante o mês** -quantidade é meu mês atual do gastam até à data? É necessário fazer ajustamentos na minha os gastos com e/ou utilização do Azure? Quando durante o mês estou consigo consumir Azure mais?
-   **Configure alertas** - gostaria de configurar o consumo de recursos ou monetário com base em alertas com base num orçamento.

## <a name="next-steps"></a>Próximos Passos

- Para obter informações sobre como utilizar as APIs de faturação do Azure para programaticamente conhecer a utilização do Azure, consulte [Azure faturação descrição geral da API](billing-usage-rate-card-overview.md).



