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
ms.openlocfilehash: 0746247d2c912ba66e81b95f45b168e32b522130
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988432"
---
# <a name="check-the-health-of-an-azure-data-explorer-cluster"></a>Verifique o estado de funcionamento de um cluster do Explorador de dados do Azure

Há vários fatores que afetam o estado de funcionamento de um cluster do Explorador de dados do Azure, incluindo a CPU, memória e o subsistema de disco. Este artigo mostra alguns passos básicos que pode tomar para avaliar o estado de funcionamento de um cluster.

1. Inicie sessão no [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. No painel esquerdo, selecione o cluster e execute o seguinte comando.

    ```Kusto
    .show diagnostics
    | project IsHealthy
    ```
    Uma saída de 1 está em bom estada; uma saída de 0 está danificada.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e navegue para o cluster.

1. Sob **monitorização**, selecione **métricas**, em seguida, selecione **manter ativo**, conforme mostrado na imagem seguinte. Uma saída próximas de 1 significa que um cluster de bom estado de funcionamento.

    ![Métrica de manter ativo do cluster](media/check-cluster-health/portal-metrics.png)

1. Adicione outras métricas, como CPU e memória de colocação em cache para medir a utilização de recursos para o cluster.

1. Se precisar de assistência diagnosticar problemas com o estado de funcionamento de um cluster, abra um pedido de suporte no [portal do Azure](https://portal.azure.com).