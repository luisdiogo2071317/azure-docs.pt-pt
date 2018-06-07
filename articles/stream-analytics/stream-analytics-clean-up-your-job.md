---
title: Limpar a sua tarefa do Azure Stream Analytics
description: Este artigo é um guia para saber como eliminar tarefas do Azure Stream Analytics.
services: stream-analytics
author: mamccrea
manager: kfile
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 4773f542f12ae1773e881106bc8948c663bfd1e3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661081"
---
# <a name="clean-up-your-azure-stream-analytics-job"></a>Limpar a sua tarefa do Azure Stream Analytics

Tarefas do Stream Analytics do Azure podem ser facilmente eliminadas através do portal do Azure, Azure PowerShell, Azure SDK para .net ou a REST API.

>[!NOTE] 
>Quando parar a tarefa de Stream Analytics, mantém os dados apenas no armazenamento entrado e de saída, tais como os Event Hubs ou SQL Database do Azure. Se forem necessárias para remover dados do Azure, lembre-se de que siga o processo de remoção dos recursos de entrada e saída da tarefa do Stream Analytics.

## <a name="stop-a-job-in-azure-portal"></a>Parar uma tarefa no portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 

2. Localize a tarefa de Stream Analytics em execução e selecione-o.

3. Na página de tarefa do Stream Analytics, selecione **parar** para parar a tarefa. 

   ![Parar a tarefa](./media/stream-analytics-clean-up-your-job/stop-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Eliminar uma tarefa no portal do Azure

1. Inicie sessão no Portal do Azure. 

2. Localize a tarefa de Stream Analytics existente e selecione-o.

3. Na página de tarefa do Stream Analytics, selecione **eliminar** para eliminar a tarefa. 

   ![Eliminar Tarefa](./media/stream-analytics-clean-up-your-job/delete-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>Parar ou eliminar uma tarefa com o PowerShell

Para parar uma tarefa com o PowerShell, utilize o [Stop-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob?view=azurermps-5.7.0) cmdlet. Para eliminar uma tarefa com o PowerShell, utilize o [remover AzureRmStreamAnalyticsJob](https://docs.microsoft.com/en-us/powershell/module/azurerm.streamanalytics/Remove-AzureRmStreamAnalyticsJob?view=azurermps-5.7.0) cmdlet.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>Parar ou eliminar uma tarefa com o Azure SDK para .NET

Para parar uma tarefa com o Azure SDK para .NET, utilize o [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) método. Para eliminar uma tarefa com o Azure SDK para .NET, [StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) método.

## <a name="stop-or-delete-a-job-using-rest-api"></a>Parar ou eliminar uma tarefa utilizando a REST API

Para parar uma tarefa utilizando a REST API, consulte o [parar](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#stop) método. Para eliminar uma tarefa utilizando a REST API, consulte o [eliminar](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-job#delete) método.