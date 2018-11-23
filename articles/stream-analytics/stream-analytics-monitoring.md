---
title: Compreender a tarefa de monitorização no Azure Stream Analytics
description: Este artigo descreve como monitorizar tarefas no Azure Stream Analytics
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: 200df7602f94f70f3fb9c62ad81a0710923184c7
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291424"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Compreender a monitorização de tarefa do Stream Analytics e como monitorizar consultas

## <a name="introduction-the-monitor-page"></a>Introdução: A página objeto da monitorização
O Azure portal ambos superfície métricas de chave de desempenho que podem ser utilizadas para monitorizar e resolver problemas de desempenho da sua consulta e a tarefa. Para ver estas métricas, navegue para a tarefa de Stream Analytics está interessado em ver as métricas para e ver que o **monitorização** secção na página de descrição geral.  

![Monitorização de ligação](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Será apresentada a janela conforme mostrado:

![Dashboard de tarefa de monitorização](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Métricas disponíveis para o Stream Analytics
| Métrica                 | Definição                               |
| ---------------------- | ---------------------------------------- |
| Eventos de Entrada Pendentes       | Número de eventos de entrada que são ' pendentes. |
| Erros de Conversão de Dados | Número de eventos de saída que não foi possível converter para o esquema de saída esperada. |
| Eventos de Entrada Antigos       | Número de eventos recebidos no início. |
| Falha no pedido de funções | Número de chamadas de função, com falhas do Azure Machine Learning (se presente). |
| Eventos de Função        | Número de eventos enviados para a função do Azure Machine Learning (se presente). |
| Pedidos de Função      | Número de chamadas para a função do Azure Machine Learning (se presente). |
| Erros de Desserialização de entrada       | Número de eventos que não foi possível anular a serialização.  |
| Bytes de Evento de Entrada      | Quantidade de dados recebidos pela tarefa de Stream Analytics, em bytes. Isto pode ser utilizado para validar que eventos estão a ser enviados para a origem de entrada. |
| Eventos de Entrada           | Quantidade de dados recebidos pela tarefa de Stream Analytics, no número de eventos. Isto pode ser utilizado para validar que eventos estão a ser enviados para a origem de entrada. |
| Origens de Entrada Recebidas       | Número de eventos provenientes de uma origem de entrada. |
| Eventos de Entrada atrasados      | Número de eventos que chegam mais tarde na origem que optar por ter sido removido ou seus timestamp foi ajustado, com base na configuração da política de ordenação de eventos da definição de janela de tolerância de chegada tardia. |
| Eventos de fora de ordem    | Número de eventos recebidos fora de ordem que foram removidos ou tendo em conta um período de tempo ajustado com base na política de ordenação de eventos. Isso pode ser afetado pela configuração da definição de janela de tolerância de fora de ordem. |
| Eventos de Saída          | Quantidade de dados enviados por tarefa do Stream Analytics para o destino de saída, no número de eventos. |
| Erros de Tempo de Execução         | Número total de erros relacionados com o processamento de consultas (excluindo detetados erros durante a ingestão de eventos ou resultados outputing) |
| % de utilização SU       | A utilização da unidade de transmissão em fluxo é atribuída a uma tarefa do separador escala da tarefa. Deve este indicador atingir 80% ou acima, há grande probabilidade de que o processamento de eventos poderá ser atrasado ou parado o progresso. |
| Atraso de Marca de Água       | O atraso de limite de tamanho máximo todas as partições de todos os resultados da tarefa. |

## <a name="customizing-monitoring-in-the-azure-portal"></a>Personalizar a monitorização no portal do Azure
Pode ajustar o tipo de gráfico, métricas apresentadas e intervalo nas definições de editar gráfico de tempo. Para obter detalhes, consulte [como personalizar monitorização](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Gráfico de Monitor de hora de consulta](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Saída mais recente
Outro ponto de dados interessantes para monitorizar a tarefa é a hora da última saída, mostrada na página de descrição geral.
Desta vez, é a hora de aplicação (ou seja, o tempo com o carimbo de hora a partir dos dados de eventos) de saída mais recentes do seu trabalho.

## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

