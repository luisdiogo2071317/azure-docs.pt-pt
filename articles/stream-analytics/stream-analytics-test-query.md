---
title: A tarefa de Stream Analytics do Azure com dados de exemplo de teste | Microsoft Docs
description: Como testar as suas consultas tarefas do Stream Analytics.
keywords: uma tarefa de teste, amostragem de entrada, carregue a data de exemplo
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: stream-analytics
ms.workload: data-services
ms.date: 03/18/2018
ms.author: sngun
ms.openlocfilehash: c026a91fff5b8ef5774993b335f8d61877aa5d39
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="test-your-stream-analytics-query-with-sample-data"></a>Testar a sua consulta do Stream Analytics com dados de exemplo

Ao utilizar o Azure Stream Analytics, pode carregar consultas de teste e dados de exemplo no portal sem iniciar ou parar uma tarefa.

## <a name="upload-sample-data-and-test-the-query"></a>Carregar dados de exemplo e testar a consulta

1. Navegue até um dos seus tarefa do Stream Analytics existente > clique em **consulta** para abrir a janela do editor de consultas. 

2. Para testar a sua consulta com dados de entrada de exemplo, faça duplo clique em qualquer uma das entradas e, em seguida, selecione **carregar dados de exemplo do ficheiro**. Atualmente, pode carregar apenas dados de formatação JSON. Se os dados estão num formato diferente como CSV, deverá convertê-la JSON antes de carregar. Pode utilizar qualquer ferramenta de conversão opensource como [CSV para JSON convertor](http://www.convertcsv.com/csv-to-json.htm) para converter os dados em JSON.

    ![consulta de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

3. Depois do carregamento estiver concluído, clique em **testar** para testar esta consulta contra os dados de exemplo que forneceu.

    ![dados de exemplo de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

4. Se pretender guardar o teste de saída para utilização posterior, o resultado da sua consulta é apresentado no browser com uma ligação para os resultados da transferência. Pode facilmente e iteratively modifique a consulta e testá-lo repetidamente para ver como o resultado é alterado.

   ![Saída de exemplo do editor de consulta do Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Quando utilizar várias saídas numa consulta, pode ver os resultados para cada saída separadamente e facilmente alternar entre elas. Depois de verificar os resultados apresentados no browser, pode guardar a consulta, iniciar a tarefa e permita que processar eventos sem erros.
Para obter mais assistência, experimente a nossa [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
