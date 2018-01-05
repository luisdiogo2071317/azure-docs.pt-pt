---
title: Teste de consulta do Stream Analytics do Azure | Microsoft Docs
description: Como testar as suas consultas tarefas do Stream Analytics.
keywords: testar a consulta, resolver problemas de consulta
documentation center: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 2898e3404dcfa3d75e3920f9c83e4efa7201998e
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Testar consultas do Azure Stream Analytics no portal do Azure

Com o Azure Stream Analytics, pode testar consultas no portal do Azure sem ser necessário iniciar ou parar uma tarefa.

## <a name="test-the-input"></a>A entrada de teste

1. Para testar com dados de entrada de exemplo, faça duplo clique de qualquer uma das entradas e, em seguida, selecione **carregar dados de exemplo do ficheiro**. Atualmente, pode carregar apenas dados de formatação JSON. Se os dados estão num formato diferente como CSV, deverá convertê-la JSON antes de carregar. Pode utilizar qualquer ferramenta de conversão opensource como [CSV para JSON convertor](http://www.convertcsv.com/csv-to-json.htm) para converter os dados em JSON.

    ![consulta de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Depois do carregamento estiver concluído, clique em **testar** para testar esta consulta contra os dados de exemplo que forneceu.

    ![dados de exemplo de teste do editor de consulta do Stream analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

A saída da sua consulta é apresentada no browser, com hiperligação de resultados da transferência deve que pretende guardar o resultado do teste para utilização posterior. Pode facilmente e iteratively modifique a consulta e testá-lo repetidamente para ver como o resultado é alterado.

![Saída de exemplo do editor de consulta do Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Com várias saídas utilizadas numa consulta, pode ver os resultados de ambas as saídas separadamente e facilmente alternar entre elas.

Depois de se satisfeito com os resultados apresentados no browser, pode guardar a consulta, iniciar a tarefa e permitem processar eventos sem erros.

## <a name="get-help"></a>Obter ajuda

Para obter mais assistência, experimente a nossa [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
