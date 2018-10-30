---
title: Verifique o estado de funcionamento de um cluster do Explorador de dados do Azure
description: Este artigo descreve os passos para determinar se o seu cluster do Explorador de dados do Azure está em bom estado.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d07873b34a41ff20b5007a88743f6b150d4d8a3d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212833"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Verifique o estado de funcionamento de um cluster do Explorador de dados do Azure

Há vários fatores que afetam o estado de funcionamento de um cluster do Explorador de dados do Azure, incluindo a CPU, memória e o subsistema de disco. Este artigo mostra alguns passos básicos que pode tomar para avaliar o estado de funcionamento de um cluster.

1. Inicie sessão em [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. No painel esquerdo, selecione o cluster e execute o seguinte comando.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Uma saída de 1 está em bom estada; uma saída de 0 está danificada.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e navegue para o cluster.

1. Sob **monitorização**, selecione **métricas**, em seguida, selecione **manter ativo**, conforme mostrado na imagem seguinte. Uma saída próximas de 1 significa que um cluster de bom estado de funcionamento.

    ![Métrica de manter ativo do cluster](media/check-cluster-health/portal-metrics.png)

1. É possível adicionar outras métricas para o gráfico. Selecione o gráfico, em seguida, **adicionar métrica**. Selecione outra métrica - este exemplo mostra **CPU**.

    ![Adicionar métrica](media/check-cluster-health/add-metric.png)

1. Se precisar de assistência diagnosticar problemas com o estado de funcionamento de um cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).