---
title: "Como criar uma tarefa de processamento de análise de dados para o Stream Analytics | Microsoft Docs"
description: "Criar uma tarefa de processamento de análise de dados para o Stream Analytics | Learning segmento de caminho."
keywords: "processamento de análise de dados"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 82b9c861fee820cdb0ca1891e0e62436f422d838
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Como criar uma tarefa de processamento de análise de dados para o Stream Analytics
O recurso de nível superior no Azure Stream Analytics é uma tarefa do Stream Analytics.  É constituída por uma ou mais origens de dados de entrada, uma consulta expressa a transformação de dados e um ou mais destinos de saída que os resultados são escritos para. Em conjunto estes permitem que o utilizador efetuar análise de dados de processamento para cenários de dados de transmissão em fluxo.

Para começar a utilizar o Stream Analytics, comece por criar uma nova tarefa de Stream Analytics.  Tenha em atenção de que esta ação não tem nenhum implicações de faturação até que a tarefa foi iniciada.

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. Selecione **novo** > **dados + análise** > **tarefa do Stream Analytics**.
3. Selecione **Criar**.
   
3. Especifique a configuração pretendida para a tarefa de Stream Analytics.
   
   * No **nome da tarefa** caixa, introduza um nome para identificar a tarefa de Stream Analytics. Quando o **nome da tarefa** é validado, aparece uma marca de verificação verde na caixa de nome de tarefa. O **nome da tarefa** pode conter apenas carateres alfanuméricos e '-' carateres e tem de ter entre 3 e 63 carateres.
   * Utilize **localização** para especificar a localização geográfica onde pretende executar a tarefa.
   * Especifique um novo ou existente **grupo de recursos** para conter recursos relacionados para a sua aplicação.
4. Selecione **Criar**.
Pode demorar alguns minutos para que a tarefa de Stream Analytics ser criada. Para verificar o estado, pode monitorizar o progresso no hub de notificações.
    
   ![Criar tarefa de análise de dados de portal do Azure de processamento da tarefa](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. A nova tarefa mostrará um Estado de **criado**. Tenha em atenção que o **iniciar** botão está desativado. Configure a tarefa entrada, consultas e de saída antes de iniciar a tarefa.

   
   ![Análise de dados de portal do Azure processar o estado da tarefa](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

