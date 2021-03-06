---
title: Limpar a tarefa do Azure Stream Analytics
description: Este artigo mostra-lhe diferentes métodos para eliminar as tarefas de Azure Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 85db38fef5e69c4de855f8cb6d54151496faebbe
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090249"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Limpar a tarefa do Azure Stream Analytics

Tarefas do Azure Stream Analytics podem ser facilmente eliminadas através do portal do Azure, Azure PowerShell, Azure SDK para .net ou a REST API.

>[!NOTE] 
>Quando parar a tarefa de Stream Analytics, os dados presentes apenas no armazenamento de entrada e saído, como os Hubs de eventos ou a base de dados do Azure SQL. Se forem necessárias para remover os dados do Azure, certifique-se de que siga o processo de remoção para os recursos de entrada e saídos da sua tarefa do Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Parar uma tarefa no portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 

2. Localize a tarefa de Stream Analytics em execução e selecioná-lo.

3. Na página de tarefa do Stream Analytics, selecione **parar** para parar a tarefa. 

   ![Parar a tarefa do Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminar uma tarefa no portal do Azure

1. Inicie sessão no Portal do Azure. 

2. Localize a tarefa de Stream Analytics existente e selecioná-lo.

3. Na página de tarefa do Stream Analytics, selecione **eliminar** para eliminar a tarefa. 

   ![Eliminar tarefa do Azure Stream Analytics](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Parar ou eliminar uma tarefa com o PowerShell

Para parar uma tarefa com o PowerShell, utilize o [Stop-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0) cmdlet. Para eliminar uma tarefa com o PowerShell, utilize o [Remove-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Parar ou eliminar uma tarefa com o Azure SDK para .NET

Para parar uma tarefa com o Azure SDK para .NET, utilize o [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) método. Para eliminar uma tarefa com o Azure SDK para .NET, [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) método.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Parar ou eliminar uma tarefa com a REST API

Para parar uma tarefa com a REST API, consulte a [parar](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) método. Para eliminar uma tarefa com a REST API, consulte a [eliminar](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) método.
