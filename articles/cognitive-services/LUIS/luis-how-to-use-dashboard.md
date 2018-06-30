---
title: Dashboard de aplicações para aplicações de LUIS | Microsoft Docs
description: Saiba mais sobre o dashboard de aplicações, uma ferramenta de criação de relatórios visualizada que lhe permite monitorizar as suas aplicações num instante único.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: c7ef38e2f2edaf795d3d76706afd4aa09b3b6959
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110053"
---
# <a name="application-dashboard"></a>Dashboard de aplicações
O dashboard de aplicações permite-lhe monitorizar a aplicação num instante único. O **Dashboard** apresenta quando abrir uma aplicação ao clicar no nome de aplicação no **aplicações My** em seguida, selecione de página **Dashboard** do painel superior. 

> [!CAUTION]
> Se pretender que as métricas mais atualizadas à sua para LUIS, tem de:
> * Utilize um LUIS [chave de ponto final](luis-how-to-azure-subscription.md) criada no Azure
> * Chave de ponto final de LUIS de utilização para todos os pedidos de ponto final, incluindo LUIS [API](https://aka.ms/luis-endpoint-apis) e bot
> * Chave de outro ponto final de utilização para cada aplicação LUIS. Não utilize uma chave de ponto final único para todas as aplicações. A chave de ponto final é controlada ao nível da chave, não ao nível da aplicação.  

O **Dashboard** página dá-lhe uma descrição geral da aplicação, incluindo o modelo atual LUIS Estado, bem como [endpoint](luis-glossary.md#endpoint) utilização ao longo do tempo. <!--The following image shows the **Dashboard** page.-->

<!-- TBD: Get a working screen shot
![The Dashboard](./media/luis-how-to-use-dashboard/dashboard.png)
-->

<!-- TBD: IS THIS STILL TRUE?
At the top of the **Dashboard** page, a contextual notification bar constantly displays notifications to update you on the required or recommended actions appropriate for the current state of your app. It also provides useful tips and alerts as needed. A detailed description of the data reported on the **Dashboard** page follows.
-->
  
## <a name="app-status"></a>Estado da aplicação
O dashboard apresenta a preparação da aplicação e da publicação de estado, incluindo a data e hora quando a aplicação foi efetuada a última preparado e publicado.  

![Dashboard - estado da aplicação](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Estatísticas de dados do modelo
O dashboard apresenta o número total de pendentes, entidades e utterances identificados existente na aplicação. 

![Estatísticas de dados da aplicação](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Pedidos de ponto final
O dashboard apresenta os pedidos de ponto final total que a aplicação de LUIS recebe e permite-lhe apresentar pedidos com êxito durante um período que especificar. O número total de pedidos apresentada é a soma de pedidos de ponto final que utilizam um [chave de ponto final](./luis-concept-keys.md#endpoint-key) e o ponto final de pedidos com êxito que utilizam um [chave de criação de conteúdos](./luis-concept-keys.md#authoring-key).

<!-- TBD: this image is old but I don't have a new one based on usage -->
![Pedidos de ponto final](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> O ponto final atual atingiu o número está no portal do Azure na descrição geral de serviço de LUIS. 
 
### <a name="total-endpoint-hits"></a>Ponto final de total de pedidos
O número total de pedidos de ponto final recebida para a sua aplicação desde a criação da aplicação até à data atual.

### <a name="endpoint-hits-per-period"></a>Pedidos de ponto final por período
O número de pedidos recebidos durante um período anterior, apresentados por dia. Os pontos de entre as datas de início e de fim representam os dias baixar neste período. Coloque o ponteiro do rato sobre cada ponto para ver os pedidos com êxito na contagem de cada dia dentro do período de. 

Para selecionar um período de ver no gráfico:
 
1. Clique em **definições adicionais** ![botão definições adicionais](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para aceder à lista de pontos finais. Pode selecionar períodos vão de uma semana cópias de segurança para um ano. 

    ![Pedidos de ponto final por período](./media/luis-how-to-use-dashboard/timerange.png)

2. Selecione um período da lista e, em seguida, clique na seta de fundo ![Fazer uma cópia de seta](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para apresentar o gráfico.

### <a name="key-usage"></a>Utilização de chaves
O número de pedidos consumido da chave de ponto final da aplicação. Para obter mais informações sobre chaves de ponto final, consulte [chaves na LUIS](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Divisão intenção
O **intenção repartição** apresenta uma divisão de pendentes com base no utterances identificados ou pedidos de ponto final. Este gráfico de resumo mostra a importância relativa de cada intenção de na aplicação. Quando paira o rato o ponteiro do rato sobre um setor, consulte o nome da intenção e a percentagem representa da contagem total de pedidos de identificados utterances/ponto final. 

![Divisão intenção](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Para controlar se a repartição baseia-se no utterances identificados ou pedidos de ponto final:

1. Clique em **definições adicionais** ![botão definições adicionais](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para aceder à lista de como a imagem seguinte:

    ![Lista de divisão intenção](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Selecione um valor na lista e, em seguida, clique na seta de fundo ![Fazer uma cópia de seta](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para apresentar o gráfico.

## <a name="entity-breakdown"></a>Divisão de entidade
O dashboard apresenta uma divisão de entidades com base no utterances identificados ou pedidos de ponto final. Este gráfico de resumo mostra a importância relativa da cada entidade na aplicação. Quando paira o rato o ponteiro do rato sobre um setor, consulte o nome da entidade e a percentagem de pedidos de identificados utterances/ponto final. 

![Divisão de entidade](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Para controlar se a repartição baseia-se no utterances identificados ou pedidos de ponto final:

1. Clique em **definições adicionais** ![botão definições adicionais](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para aceder à lista de como a imagem seguinte:

    ![Lista de divisão de entidade](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Selecione um valor na lista e, em seguida, clique na seta de fundo ![Fazer uma cópia de seta](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para apresentar o gráfico em conformidade.
