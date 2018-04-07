---
title: Como criar uma tarefa de processamento de análise de dados para o Azure Stream Analytics
description: Criar uma tarefa de processamento de análise de dados para o Stream Analytics | Learning segmento de caminho.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 349d99a003df4fd211842033a467aa3051b50c77
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Como criar uma tarefa de processamento de análise de dados para o Stream Analytics
O recurso de nível superior no Azure Stream Analytics é uma tarefa do Stream Analytics.  É constituída por uma ou mais origens de dados de entrada, uma consulta expressa a transformação de dados e um ou mais destinos de saída que os resultados são escritos para. Em conjunto estes permitem que o utilizador efetuar análise de dados de processamento para cenários de dados de transmissão em fluxo.

Para começar a utilizar o Stream Analytics, comece por criar uma nova tarefa de Stream Analytics.  Tenha em atenção de que esta ação não tem nenhum implicações de faturação até que a tarefa foi iniciada.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione **crie um recurso** > **dados + análise** > **tarefa do Stream Analytics**.
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
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

