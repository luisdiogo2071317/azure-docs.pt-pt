---
title: Monitorizar tarefas no Azure Data Lake Analytics no portal do Azure
description: Este artigo descreve como utilizar o portal do Azure para resolver problemas relacionados com tarefas do Azure Data Lake Analytics.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 8c61b8736dfb13f0c2c2520f22979ac51646e05f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623660"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Monitorizar tarefas no Azure Data Lake Analytics com o Portal do Azure

**Para ver todas as tarefas**

1. No portal do Azure, clique em **Microsoft Azure** no canto superior esquerdo.
2. Clique no mosaico com o nome da sua conta de Data Lake Analytics.  A tarefa de resumo é apresentada no **de tarefas gestão** mosaico.

    ![Gestão de tarefas do Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    A tarefa de gestão dá-lhe relance o estado da tarefa. Tenha em atenção de que não há uma tarefa falhada.
3. Clique em de **de tarefas gestão** mosaico para obter as tarefas. As tarefas são categorizadas em **executar**, **em fila**, e **terminado**. Deverá ver a tarefa falhada no **terminado** secção. Este deverá ser primeiro na lista. Quando tiver um grande número de tarefas, pode clicar em **filtro** para ajudar a localizar tarefas.

    ![Tarefas de filtro do Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Clique na tarefa falhada a partir da lista para abrir os detalhes da tarefa:

    ![Tarefa de falha do Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Tenha em atenção o **reenviar** botão. Depois de corrigir o problema, pode submeter novamente a tarefa.
5. Clique em parte realçado na captura de ecrã anterior para abrir os detalhes do erro.  Deverá ver algo semelhante ao seguinte:

    ![Detalhes da tarefa de falha do Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Indica que a pasta de origem não foi encontrada.
6. Clique em **duplicado Script**.
7. Atualização do **FROM** caminho:

    "/ Samples/Data/SearchLog.tsv"
8. Clique em **Submeter Tarefa**.

## <a name="see-also"></a>Consulte também
* [Descrição geral da análise do Azure Data Lake](data-lake-analytics-overview.md)
* [Introdução ao Azure Data Lake Analytics com o Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Gerir o Azure Data Lake Analytics com o portal do Azure](data-lake-analytics-manage-use-portal.md)
