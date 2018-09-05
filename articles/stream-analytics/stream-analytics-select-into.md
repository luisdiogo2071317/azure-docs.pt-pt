---
title: Depurar consultas do Azure Stream Analytics com o SELECT INTO
description: Este artigo descreve como consulta médio de dados na tarefa Azure Stream Analytics de exemplo com instruções SELECT INTO na sintaxe de consulta.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/20/2017
ms.openlocfilehash: b056d4c29464451d3dc0ef62437f934535820489
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697997"
---
# <a name="debug-queries-by-using-select-into-statements"></a>Depurar consultas através de instruções SELECT INTO

No processamento de dados em tempo real, a saber o que aspeto dos dados no meio de consulta pode ser útil. Como entradas ou passos de uma tarefa do Azure Stream Analytics podem ser lido várias vezes, pode escrever instruções SELECT INTO extras. Se o fizer, produz dados intermediários para o armazenamento e permite-lhe inspecionar a correção dos dados, assim como acontece *assista variáveis* quando depura um programa.

## <a name="use-select-into-to-check-the-data-stream"></a>Utilizar SELECT INTO para verificar o fluxo de dados

A seguinte consulta de exemplo num trabalho do Azure Stream Analytics tem um fluxo de entrada, duas entradas de dados de referência e uma saída para o armazenamento de tabelas do Azure. A consulta associa os dados do hub de eventos e blobs de referência de dois para obter as informações de nome e categoria:

![Exemplo de consulta SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Tenha em atenção que a tarefa está em execução, mas não existem eventos estão a ser produzido na saída. Na **monitorização** mosaico, mostrado aqui, pode ver que a entrada produz dados, mas não sabe qual etapa da **associar** causado todos os eventos a ser removido.

![O mosaico de monitorização](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Nesta situação, pode adicionar alguns Extras em instruções SELECT INTO "iniciar sessão" os resultados intermediários de associação e os dados que são lidos a partir de entrada.

Neste exemplo, adicionámos duas novas "temporárias saídas." Eles podem ser qualquer coletor que desejar. Aqui usamos armazenamento do Azure como um exemplo:

![Adicionar Extras em instruções SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Em seguida, pode reescrever a consulta como esta:

![Reescrito consulta SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Agora, iniciar a tarefa novamente e deixar executar durante alguns minutos. Em seguida, consultar temp1 e temp2 com o Visual Studio Cloud Explorer para produzir as tabelas seguintes:

**tabela de temp1**
![SELECT INTO temp1 tabela](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**tabela de temp2**
![SELECT INTO temp2 tabela](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Como pode ver, temp1 e temp2 têm dados e a coluna de nome é preenchida corretamente no temp2. No entanto, uma vez que ainda não houver dados na saída, algo está errado:

![SELECT INTO output1 tabela sem dados](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Pelos dados de amostragem, pode ser quase certo que o problema é com a associação de segundo. Pode transferir os dados de referência de blob e dar uma olhada:

![Tabela de ref SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Como pode ver, o formato de GUID nestes dados de referência é diferente do formato da [coluna na temp2 from]. É por isso que os dados não chegam output1 conforme esperado.

Pode corrigir o formato de dados, carregue-o para o blob de referência e tente novamente:

![SELECT INTO tabela temporária](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Desta vez, os dados no resultado são formatados e preenchidos conforme esperado.

![SELECT INTO tabela final](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)


## <a name="get-help"></a>Obter ajuda

Para obter assistência, tente nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

