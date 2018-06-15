---
title: Problemas comuns para resolver problemas no Azure Stream Analytics
description: Este artigo descreve alguns problemas comuns do Azure Stream Analytics e passos para resolver esses problemas.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: e04d1072acee635235b0a5bd8465ca38c861017b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31523528"
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Problemas comuns do Stream Analytics e passos para resolver problemas

## <a name="troubleshoot-malformed-input-events"></a>Resolver problemas de eventos de entrada com formato incorreto

 Problemas de serialização são causados quando o fluxo de entrada da tarefa do Stream Analytics contém mensagens mal formadas. Por exemplo, uma mensagem incorretamente formada pode ser causado por um parêntesis em falta ou uma chaveta num objeto JSON em falta ou incorretos hora em formato de carimbo de data / no campo de hora. 
 
 Quando uma tarefa de Stream Analytics recebe uma mensagem incorretamente formada de entrada, ignora a mensagem e notifica o utilizador com um aviso. Um símbolo de aviso é apresentado no **entradas** mosaico da tarefa do Stream Analytics (esta sessão de aviso existe, desde que a tarefa está em estado de execução):

![Entradas do mosaico](media/stream-analytics-malformed-events/inputs_tile.png)

Para obter mais informações, ative os registos de diagnóstico para ver os detalhes sobre o aviso. Para eventos de entrada com formato incorreto, os registos de execução contém uma entrada com a mensagem que se assemelha: "mensagem: não foi possível anular a serialização do evento de entrada do recurso <blob URI> como json". 

### <a name="troubleshooting-steps"></a>Passos de resolução de problemas

1. Navegue para o mosaico de entrada e clique para ver os avisos.

2. O mosaico de entrada de detalhes apresenta um conjunto de avisos com detalhes sobre o problema. Segue-se uma mensagem de aviso de exemplo, a mensagem de aviso mostra a partição, o desvio e números de sequência em que contém dados JSON com formato incorreto. 

   ![Mensagem de aviso com desvio igual a](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Para obter os dados JSON tem um formato incorreto, execute o código de CheckMalformedEvents.cs. Este exemplo está disponível no [repositório do GitHub exemplos](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Este código leituras o ID de partição, deslocamento e impressões os dados que estão localizados em que o desvio. 

4. Depois de ler os dados, pode analisar e corrigir o formato de serialização. 

## <a name="delayed-output"></a>Saída atrasada

### <a name="first-output-is-delayed"></a>Saída primeiro está atrasada
Quando é iniciada uma tarefa de Stream Analytics, os eventos de entrada são lidas, mas pode haver um atraso na saída que está a ser produzida em determinadas circunstâncias.

Valores de tempo em elementos de consulta temporal podem contribuir para o atraso de saída. Para produzir o resultado correto através do windows de tempo, a tarefa de transmissão em fluxo é iniciado através da leitura de dados de possíveis de tempo mais recente (há até sete dias) para preencher a janela de tempo. Durante este período, nenhuma saída é produzida até que a leitura dos eventos de entrada pendentes catch-up esteja concluída. Este problema pode superfície quando o sistema atualiza as tarefas de transmissão em fluxo, deste modo, reiniciar a tarefa. Essas atualizações ocorrerem, geralmente, uma vez cada alguns meses. 

Por conseguinte, utilize o critério ao conceber a sua consulta do Stream Analytics. Se utilizar uma janela de tempo (mais de várias horas, até sete dias) para elementos temporais na sintaxe de consulta da tarefa, pode levar a um atraso na primeira saída quando a tarefa é iniciada ou reiniciada.  

Uma mitigação para este tipo de primeiro atraso de saída é utilize técnicas de parallelization de consulta (os dados de criação de partições) ou adicione mais unidades de transmissão em fluxo de mensagens em fila para melhorar o débito até intercete a tarefa de cópia de segurança.  Para obter mais informações, consulte [considerações ao criar tarefas do Stream Analytics](stream-analytics-concepts-checkpoint-replay.md)

Estes fatores afetam a pontualidade de saída primeiro que é gerada:

1. Utilização de agregados Next (grupo através de em cascata, salto e a deslizante windows)
   - Para em cascata ou hopping agregados de janela, os resultados são gerados no fim do período de tempo janela. 
   - Para uma janela deslizante, os resultados são gerados quando um evento entra ou sai da janela deslizante. 
   - Se estiver a planear utilizar o tamanho da janela elevado (> 1 hora), é melhor escolher a janela de salto ou deslizante, para que possa ver o resultado com mais frequência.

2. Utilização das associações temporais (associação com DATEDIFF)
   - Correspondências são geradas, assim como quando chegam ambos os lados dos eventos correspondentes.
   - Dados que não possui uma correspondência (associação externa à esquerda) são gerados no fim da janela DATEDIFF relativamente a cada evento no lado esquerdo.

3. Utilização de funções analíticas temporais (ISFIRST, LAST e desfasamento com LIMIT DURATION)
   - Para as funções analíticas, o resultado é gerado para cada evento, não existe nenhum atraso.

### <a name="output-falls-behind"></a>Saída se situe atrás
Durante o funcionamento normal da tarefa, se encontrar o que resultado da tarefa é baixar atrás (latência maior e mais), pode identificar as causas raiz, examinando estes fatores:
- Indica se o sink a jusante está limitado
- Se está limitada a origem a montante
- Indica se a lógica de processamento da consulta está intensivas de computação

Para ver os detalhes, no portal do Azure, selecione a tarefa de transmissão em fluxo e selecione o **diagrama de tarefa**. Para cada entrada, há um por métrica de eventos de registo de segurança de partição. Se a métrica de eventos de registo de segurança mantém aumentar, é um indicador de que os recursos do sistema são restringidos. Potencialmente que é devido a limitação de sink de saída ou elevada da CPU. Para obter mais informações sobre como utilizar o diagrama de tarefa, consulte [condicionada por dados depuração utilizando o diagrama de tarefa](stream-analytics-job-diagram-with-metrics.md).

## <a name="handle-duplicate-records-in-azure-sql-database-output"></a>Processar registos duplicados na saída da SQL Database do Azure

Quando configura a base de dados SQL do Azure como saída para uma tarefa de Stream Analytics, mesmo em massa insere registos na tabela de destino. Em geral, do Azure stream analytics garante que [entrega, pelo menos, uma vez]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) para o sink de saída, um pode ainda [alcançar exatamente-uma vez entrega]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) para SQL Server de saída quando a tabela SQL tem uma restrição exclusiva definida. 

Depois de restrições de chave exclusivas são configuradas na tabela de SQL e existem registos duplicados, que está a ser inseridos na tabela SQL, o Azure Stream Analytics remove o registo duplicado. -Divide os dados em lotes e recursivamente inserir os lotes até é encontrado um único registo duplicado. Se a tarefa de transmissão em fluxo tem um número de linhas duplicadas, esta divisão considerável e inserir o processo tem de ignorar os duplicados um de cada, que é menos eficiente e morosa. Se vir várias mensagens de aviso de violação da chave no registo de atividades dentro da hora anterior, é provável que a saída do SQL Server é abrandamento a tarefa de toda. 

Para resolver este problema, deve [configurar o índice]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) que está a causar a violação de chave ao ativar a opção IGNORE_DUP_KEY. A ativação desta opção permite especificar valores duplicados e ignorados pelo SQL durante inserções em massa e SQL Azure simplesmente produz uma mensagem de aviso em vez de um erro. O Azure Stream Analytics não produz já erros de violação da chave primária.

Quando configurar IGNORE_DUP_KEY para vários tipos de índices, tenha em atenção as de observações seguintes:

* Não é possível definir IGNORE_DUP_KEY numa chave primária ou uma restrição exclusiva que utilize ALTER INDEX, terá de remover e recriar o índice.  
* Pode definir a opção IGNORE_DUP_KEY utilizando ALTER INDEX para um índice exclusivo, que é diferente da restrição de chave primária/exclusivos e criado com a definição de índice ou CREATE INDEX.  
* IGNORE_DUP_KEY não se aplicar aos índices de arquivo de colunas não é possível impor a exclusividade nessas índices.  

## <a name="next-steps"></a>Passos Seguintes
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
