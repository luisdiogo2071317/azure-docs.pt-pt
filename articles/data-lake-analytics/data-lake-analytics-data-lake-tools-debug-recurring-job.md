---
title: "Resolver problemas de uma tarefa periódica anormal | Microsoft Docs"
description: "Saiba como utilizar o Azure Data Lake Tools para Visual Studio para depurar uma tarefa periódica anormal."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: 9b60c861810d6577b33aa0cdf14f26dc2cfc0e4d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Resolver problemas de uma tarefa periódica anormal

Este artigo mostra como utilizar [ferramentas do Azure Data Lake para Visual Studio](http://aka.ms/adltoolsvs) para resolver problemas relacionados com tarefas periódicas. Saiba mais sobre o pipeline e tarefas periódicas a partir do [blogue do Azure Data Lake e o Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Trabalhos recorrentes, normalmente, partilham a mesma consulta lógica e os dados de entrada semelhantes. Por exemplo, imagine que tem uma tarefa periódica que cada segunda-feira de manhã 8 A.M. a contagem de utilizador de Active Directory semanal última semana. Os scripts para estas tarefas partilham um modelo de script que contém a lógica de consulta. As entradas para estas tarefas são os dados de utilização da última semana. Partilhar a mesma lógica de consulta e a entrada semelhante, normalmente, significa que o desempenho destas tarefas é semelhante e estável. Se um dos seus trabalhos recorrentes subitamente efetua anormalmente, falha ou atrasar muito, pode querer:

- Consulte os relatórios de estatísticas para a execução anterior da tarefa periódica para ver o que aconteceu.
- Compare o trabalho anormal com um normal para descobrir o que foi alterado.

**Relacionadas com a vista de tarefas** nas ferramentas do Azure Data Lake para Visual Studio ajuda pode acelerar o progresso de resolução de problemas com ambos os casos.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Passo 1: Localize trabalhos recorrentes e abra a vista de tarefas relacionadas

Para utilizar a vista de tarefas relacionadas para resolver um problema de tarefa periódica, terá de primeiro localizar a tarefa periódica no Visual Studio e, em seguida, abra a vista de tarefas relacionadas.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Cenário 1: Tem o URL para a tarefa periódica

Através de **ferramentas** > **Data Lake** > **tarefas vista**, pode colar o URL de tarefa para abrir a vista de tarefa no Visual Studio. Selecione **ver tarefas relacionadas** para abrir a vista de tarefas relacionadas.

![Ver tarefas relacionadas ligação nas ferramentas do Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Caso 2: Tem o pipeline para a tarefa periódica, mas não o URL

No Visual Studio, pode abrir o Browser de Pipeline através do Explorador de servidores > sua conta do Azure Data Lake Analytics > **Pipelines**. (Se não é possível localizar este nó no Explorador de servidores, [transfira o Plug mais recente-in](http://aka.ms/adltoolsvs).) 

![Selecionar o nó de Pipelines](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Num Browser de Pipeline, são listados todos os pipelines para a conta de Data Lake Analytics no lado esquerdo. Pode expandir pipelines para localizar todos os trabalhos recorrentes e, em seguida, selecione de que tem problemas. Vista de tarefas relacionadas abre no lado direito.

![Selecionar um pipeline e ao abrir a vista de trabalho relacionados](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Passo 2: Analisar um relatório de estatísticas

Um resumo e um relatório de estatísticas são apresentados na parte superior da vista de tarefa relacionados. Aqui, pode encontrar a potencial causa do problema. 

1.  No relatório, o eixo x mostra o tempo de submissão da tarefa. Utilizá-la para localizar a tarefa anormal.
2.  Utilize o processo no diagrama seguinte para verificar as estatísticas e obter informações sobre o problema e possíveis soluções.

![Diagrama do processo para a verificação das estatísticas](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Passo 3: Comparar a tarefa a um trabalho normal anormal

Pode encontrar submetido todos os trabalhos recorrentes através da lista de tarefas na parte inferior da vista de tarefa relacionados. Para obter mais informações e possíveis soluções, clique com botão direito a tarefa anormal. Utilize a vista de tarefa Diff para comparar a tarefa anormal com um normal anterior.

![Menu de atalho para comparar as tarefas](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Preste atenção para as diferenças entre estas duas tarefas grandes. Dessas diferenças provavelmente estão a causar problemas de desempenho. Para obter mais verificar, utilize os passos no diagrama seguinte:

![Diagrama do processo para verificar as diferenças entre tarefas](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Passos Seguintes

* [Resolver problemas de desfasamento de dados](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Depurar definido pelo utilizador código c# para as tarefas que falharam U-SQL](data-lake-analytics-debug-u-sql-jobs.md)