---
title: Testar uma tarefa de Stream Analytics do Azure com dados de exemplo
description: Como testar as suas consultas tarefas do Stream Analytics.
keywords: Este artigo descreve como utilizar o portal do Azure para testar uma tarefa do Azure Stream Analytics, a entrada de exemplo e carregar dados de exemplo.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/27/2018
ms.openlocfilehash: 3dc9091934f3db8ededc13f74d2f302eccace4d6
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Testar uma consulta do Stream Analytics com dados de exemplo

Ao utilizar o Azure Stream Analytics, pode carregar consultas de teste e dados de exemplo no portal do Azure sem iniciar ou parar uma tarefa.

## <a name="upload-sample-data-and-test-the-query"></a>Carregar dados de exemplo e testar a consulta

1. Inicie sessão no Portal do Azure. 

2. Localize a tarefa de Stream Analytics existente e selecione-o.

3. No Stream Analytics tarefa página, sob o **topologia de tarefa** cabeçalho, selecione **consulta** para abrir a janela do editor de consultas. 

4. Para testar a sua consulta com dados de entrada de exemplo, faça duplo clique em qualquer uma das entradas.  Em seguida, selecione **carregar dados de exemplo do ficheiro**.

   Os dados tem de ser apenas dados de formatação JSON. Se os dados estão num formato diferente como CSV, deverá convertê-la JSON antes de carregar. Pode utilizar qualquer ferramenta de conversão opensource como [CSV para JSON convertor](http://www.convertcsv.com/csv-to-json.htm) para converter os dados em JSON.

    ![consulta de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Depois do carregamento estiver concluído, selecione **testar** para testar esta consulta contra os dados de exemplo que forneceu.

    ![dados de exemplo de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Se precisar do resultado do teste para utilização posterior, o resultado da sua consulta é apresentado no browser com uma ligação para os resultados da transferência. 

7. Iteratively modifique a consulta e testá-lo novamente para ver como o resultado é alterado.

   ![Saída de exemplo do editor de consulta do Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Quando utilizar várias saídas numa consulta, os resultados são apresentados no separadores separadas e facilmente pode alternar entre elas.

8. Depois de verificar os resultados apresentados no browser, **guardar** sua consulta. Em seguida, **iniciar** a tarefa e permitir que o mesmo processar os eventos de entrada.

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
