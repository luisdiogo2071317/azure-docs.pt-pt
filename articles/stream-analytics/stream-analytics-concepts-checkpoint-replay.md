---
title: Ponto de verificação e de reprodução de tarefa recuperação os conceitos do Azure Stream Analytics
description: Este artigo descreve o ponto de verificação e de reprodução de tarefa recuperação os conceitos do Azure Stream Analytics.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: 32970ff37d202cc73e7ab7aa1bf3d737dae895c1
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936722"
---
# <a name="checkpoint-and-replay-concepts-in-azure-stream-analytics-jobs"></a>Conceitos de ponto de verificação e repetição nas tarefas do Azure Stream Analytics
Este artigo descreve os internos ponto de verificação e repetição conceitos no Azure Stream Analytics e o impacto que das tem na recuperação de tarefa. Sempre que é executada uma tarefa de Stream Analytics, informações de estado são mantidas internamente. Essas informações de estado são guardadas num ponto de verificação periodicamente. Em alguns cenários, as informações de ponto de verificação são utilizadas para recuperação da tarefa se ocorrer uma falha de tarefa ou a atualização. Noutras circunstâncias, o ponto de verificação não pode ser utilizado para recuperação, não sendo uma repetição necessária.

## <a name="stateful-query-logic-in-temporal-elements"></a>Lógica de monitorização de estado de consulta em elementos temporais
Um da capacidade exclusiva da tarefa do Azure Stream Analytics consiste em efetuar o processamento de monitorização de estado, como as agregações, associações temporais e as funções analíticas temporais. Cada um destes operadores mantém as informações de estado quando a tarefa é executada. O tamanho da janela máximo para estes elementos de consulta é sete dias. 

O conceito de janela temporal aparece no vários elementos de consulta do Stream Analytics:
1. As agregações (grupo através de em cascata, salto e a deslizante windows)

2. Associações temporais (associação com DATEDIFF)

3. Funções analíticas temporais (ISFIRST, LAST e desfasamento com LIMIT DURATION)


## <a name="job-recovery-from-node-failure-including-os-upgrade"></a>Recuperação de tarefa de falha de nó, incluindo a atualização do SO
Sempre que é executada uma tarefa de Stream Analytics, internamente é ampliar a trabalhar entre vários nós de trabalho. Estado de cada nó de trabalho é checkpointed cada alguns minutos, que ajuda o recuperar se ocorrer uma falha de sistema.

Por vezes, poderá falhar um nó de trabalho especificado ou uma atualização do sistema operativo pode ocorrer para esse nó de trabalho. Para recuperar automaticamente, o Stream Analytics adquire um novo nó de bom estado de funcionamento e o estado do nó de trabalho anterior é restaurado a partir de verificação mais recente disponível. Para retomar o trabalho, é necessária restaurar o estado da hora quando o ponto de verificação foi efetuado uma pequena quantidade de repetição. Normalmente, o intervalo de restauro é apenas de alguns minutos. Quando suficiente unidades de transmissão em fluxo estão selecionadas para a tarefa, a reprodução deve ser concluída rapidamente. 

Uma consulta paralela completamente, é proporcional ao tempo que demora a acompanhar após uma falha de nó de trabalho:

[a taxa de eventos de entrada] x [o comprimento do intervalo] / [número de partições a processar]

Se alguma vez observar o atraso de processamento significativas devido a falha de nó e SO atualizar, considere efetuar a consulta totalmente paralela e dimensionar a tarefa alocar mais unidades de transmissão em fluxo. Para obter mais informações, consulte [dimensionar uma tarefa de Stream Analytics do Azure para aumentar o débito](stream-analytics-scale-jobs.md).

Atual do Stream Analytics não mostrar um relatório quando este tipo de processo de recuperação está a decorrer.

## <a name="job-recovery-from-a-service-upgrade"></a>Recuperação de tarefa de uma atualização de serviço 
Microsoft ocasionalmente atualiza os binários que executam as tarefas do Stream Analytics no serviço do Azure. Estes tempos tarefas em execução dos utilizadores sejam atualizadas para a versão mais recente e a tarefa é reiniciado automaticamente. 

Atualmente, o formato de ponto de verificação de recuperação não é preservado entre as atualizações. Como resultado, o estado da consulta de transmissão em fluxo deve ser restaurado utilizando inteiramente técnica de reprodução. Para permitir que as tarefas do Stream Analytics reproduzir o exato mesmo de entrada do antes, é importante definir, pelo menos, a política de retenção para a origem de dados para a janela tamanhos na sua consulta. Conseguir fazê-lo poderá resultar em resultados incorretos ou parciais durante a atualização do serviço, uma vez que a origem de dados não pode ser mantidas suficiente novamente para incluir o tamanho da janela completa.

Em geral, a quantidade de repetição necessária é proporcional ao tamanho da janela multiplicado pela taxa de eventos média. Por exemplo, para uma tarefa com uma taxa de entrada de 1000 eventos por segundo, é considerado um tamanho de janela maior do que uma hora para ter um tamanho de reprodução de grandes dimensões. Até uma hora de dados poderá ter de ser novamente processados para inicializar o estado para que este possa produzir completa e os resultados corretos, o que podem causar um atraso de saída (nenhuma saída) para um determinado período expandido. As consultas com sem windows ou outros operadores temporais, como `JOIN` ou `LAG`, teria de reprodução de zero.

## <a name="estimate-replay-catch-up-time"></a>Tempo de catch-up de reprodução de estimativa
Para estimar o período de atraso devido a uma atualização de serviço, pode seguir esta técnica:

1. Carregar o Hub de eventos entrada com dados suficientes para cobrir o maior tamanho da janela da consulta, a taxa de evento esperado. Timestamp dos eventos deve ser próximo da hora de relógio lateral durante esse período de tempo, como se fosse um feed de entrada em direto. Por exemplo, se tiver uma janela de dia 3 na sua consulta, enviar eventos para o Hub de eventos para três dias e continuar a enviar eventos. 

2. Começar a utilizar a tarefa **agora** como a hora de início. 

3. Medir o tempo entre a hora de início e quando é gerado o resultado primeiro. É a hora aproximada quanto atraso a tarefa será cobrado durante uma atualização de serviço.

4. Se o atraso é demasiado longo, tente a tarefa de partição e aumentar o número de SUs, para distribuir a carga a mais nós. Em alternativa, considere reduzir os tamanhos de janela da consulta e efetuar mais agregação ou outra com monitorização de estado de processamento na saída produzida pela tarefa de Stream Analytics no sink a jusante (por exemplo, utilizando a base de dados SQL do Azure).

Para a preocupação de estabilidade do serviço geral durante a atualização das tarefas de crítico de missão, considere executar tarefas duplicadas em regiões do Azure emparelhadas. Para obter mais informações, consulte [fiabilidade de tarefa de Stream Analytics da garantia durante as atualizações de serviço](stream-analytics-job-reliability.md).

## <a name="job-recovery-from-a-user-initiated-stop-and-start"></a>Recuperação de tarefa a partir de um utilizador iniciada parar e iniciar
Para editar a sintaxe de consulta numa tarefa de transmissão em fluxo ou ajustar entradas e saídas, a tarefa tem de ser parado para efetuar as alterações e atualizar a estrutura de tarefa. Tais cenários, quando um utilizador para a tarefa de transmissão em fluxo e inicia-la novamente, o cenário de recuperação é semelhante a atualização do serviço. 

Dados de ponto de verificação não podem ser utilizados para um reinício de tarefa iniciada pelo utilizador. Para estimar o atraso de saída durante um reinício deste tipo, utilize o mesmo procedimento conforme descrito na secção anterior e aplicar mitigação semelhante, se o atraso é demasiado longo.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre escalabilidade e fiabilidade, consulte estes artigos:
- [Tutorial: Configurar alertas para tarefas do Azure Stream Analytics](stream-analytics-set-up-alerts.md)
- [Uma tarefa de Stream Analytics do Azure para aumentar o débito de escala](stream-analytics-scale-jobs.md)
- [Garante fiabilidade de tarefa do Stream Analytics durante as atualizações de serviço](stream-analytics-job-reliability.md)
