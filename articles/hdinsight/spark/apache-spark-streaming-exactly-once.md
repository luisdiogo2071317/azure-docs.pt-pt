---
title: Criar tarefas de transmissão em fluxo do Spark com exatamente-uma vez eventos processamento - Azure HDInsight | Microsoft Docs
description: Como configurar a transmissão em fluxo do Spark para processar um evento, uma vez e apenas uma vez.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ramoha
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: 61162a845f6620d7c6e5662baedfdbfae9b9653a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Criar tarefas de transmissão em fluxo do Spark com exatamente-uma vez eventos de processamento

Aplicações de processamento de fluxo efetuar diferentes abordagens para como estes processam mensagens processamento novamente após alguns falha no sistema:

* Pelo menos uma vez: cada mensagem é garantida a ser processado, mas podem obter processado mais do que uma vez.
* No máximo uma vez: cada mensagem pode ou não podem ser processada. Se uma mensagem é processada, este é apenas processado uma vez.
* Exatamente uma vez: cada mensagem é garantida a ser processado uma vez e apenas uma vez.

Este artigo mostra como configurar a transmissão em fluxo do Spark para alcançar exatamente-processamento de uma vez.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Exatamente-uma vez semântica com transmissão em fluxo do Spark

Em primeiro lugar, considere como reiniciar todos os pontos de sistema de falha depois de ter um problema e como pode evitar perda de dados. Tem uma aplicação de transmissão em fluxo do Spark:

* Uma origem de entrada
* Um ou mais processos de recetor solicitar dados da origem de entrada
* Tarefas que processam os dados
* Um sink de saída
* Um processo de controladores que gere a tarefa de longa execução

Exatamente-assim que necessita de semântica que não existem dados perde-se em qualquer momento, e que o processamento da mensagem é reiniciável, independentemente de onde ocorre a falha.

### <a name="replayable-sources"></a>Replayable origens

Tem de ser a origem da aplicação de transmissão em fluxo do Spark está a ler os eventos de *replayable*. Isto significa que em casos onde a mensagem foi obtida, mas, em seguida, o sistema falha antes da mensagem não foi continuada ou processada, a origem tem de fornecer a mesma mensagem novamente.

No Azure, Event Hubs do Azure e Kafka no HDInsight fornecem replayable origens. Outro exemplo de uma origem replayable é um sistema de ficheiros com tolerância a falhas, como o HDFS, blobs de armazenamento do Azure, ou do Azure Data Lake Store, onde todos os dados são mantidos indefinidamente e em qualquer momento, pode voltar ler os dados na íntegra.

### <a name="reliable-receivers"></a>Recetores fiáveis

De transmissão em fluxo do Spark, origens, como Kafka e Hubs de eventos tem *recetores fiáveis*, onde cada recetor mantém um registo do progresso da leitura de origem. Um recetor fiável mantém o estado para o armazenamento com tolerância a falhas, dentro de ZooKeeper ou na transmissão em fluxo do Spark os pontos de verificação escritos HDFS. Se esses um recetor falha e é posterior reiniciado, pode escolher em que parou.

### <a name="use-the-write-ahead-log"></a>Utilizar o registo de escrita antecipada

Transmissão em fluxo do Spark suporta a utilização de um registo de escrita antecipada, em que cada evento recebido é primeiro escrito ao diretório de ponto de verificação do Spark no armazenamento com tolerância a falhas e, em seguida, armazenado num Resiliente distribuídas conjunto de dados (RDD). No Azure, o armazenamento com tolerância a falhas é HDFS segurança efetuada pelo armazenamento do Azure ou do Azure Data Lake Store. A aplicação de transmissão em fluxo do Spark, o registo de escrita antecipada está ativado para todos os recetores definindo a `spark.streaming.receiver.writeAheadLog.enable` definição de configuração para `true`. O registo de escrita antecipada fornece tolerância a falhas de falhas de controlador e o executor.

Para trabalhadores de tarefas em execução contra os dados do evento, cada RDD é pela definição ambos replicados e distribuídos por vários workers. Se uma tarefa falhar porque o trabalho em execução que falhou, a tarefa será reiniciada no outro worker que tenha uma réplica dos dados de eventos, por isso, o evento não é perdido.

### <a name="use-checkpoints-for-drivers"></a>Utilizar pontos de verificação para controladores

Os controladores de tarefa têm de ser reiniciados. Se o controlador de execução da sua aplicação de transmissão em fluxo do Spark falhar, demora com o mesmo todos os recetores em execução, tarefas e qualquer RDDs armazenar dados de eventos. Neste caso, terá de poder guardar o progresso da tarefa para que possa retomar mais tarde. Isto é conseguido ao ponto de verificação a direcionado Acíclico gráfico (DAG) de DStream periodicamente para o armazenamento com tolerância a falhas. Os metadados do DAG incluem a configuração utilizada para criar a aplicação de transmissão em fluxo, as operações que definem a aplicação e quaisquer lotes que são colocados em fila, mas ainda não foi concluídas. Estes metadados permite que um controlador de falha para reiniciada desde as informações de ponto de verificação. Quando reinicia o controlador, iniciará recetores novo próprios de recuperar os dados do evento no RDDs do registo de escrita antecipada.

Pontos de verificação estão ativados no Spark de transmissão em fluxo em dois passos. 

1. O objeto de StreamingContext, configure o caminho de armazenamento para os pontos de verificação:

    val ssc = new StreamingContext(spark, Seconds(1))  ssc.checkpoint("/path/to/checkpoints")

    No HDInsight, estes pontos de verificação deverá ser guardados para o armazenamento de predefinido ligado ao seu cluster de armazenamento do Azure ou do Azure Data Lake Store.

2. Em seguida, especifique um intervalo de ponto de verificação (em segundos) a DStream. A cada intervalo, derivados do evento de entrada de dados de estado são transferidos para o armazenamento. Dados de estado persistente podem reduzir a computação necessária ao reconstruir o estado a partir de eventos de origem.

    val lines = ssc.socketTextStream("hostname", 9999)  lines.checkpoint(30)  ssc.start()  ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Utilizar idempotent sinks

O sink de destino para o qual a tarefa escreve resultados tem de ser capaz de lidar com a situação em que é dado o mesmo resultado mais do que uma vez. O sink tem de ser capaz de detetar esses resultados duplicados e ignorá-las. Um *idempotent* sink pode ser chamado várias vezes com os mesmos dados sem qualquer alteração de estado.

Pode criar idempotent sinks implementando lógica primeiro verifica a existência do resultado da entrada no arquivo de dados. Se o resultado já existir, deverá aparecer a operação de escrita com êxito a partir da perspetiva da tarefa Spark, mas na realidade, o arquivo de dados ignorados de dados duplicados. Se o resultado não existir, em seguida, o sink deverão inserir este resultado de novo para o respetivo armazenamento. 

Por exemplo, poderia utilizar um procedimento armazenado com base de dados do SQL do Azure que insere eventos numa tabela. Este procedimento armazenado procura primeiro o evento para campos de chave e apenas quando nenhum evento correspondente foi encontrado o registo inserido na tabela.

Outro exemplo consiste em utilizar um sistema de ficheiros particionadas, como o blobs Storage do Azure ou do Azure Data Lake store. Neste caso, a lógica de sink não é necessário verificar a existência de um ficheiro. Se o ficheiro que representa o evento existir, é simplesmente substituído com os mesmos dados. Caso contrário, é criado um novo ficheiro no caminho calculado.

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral de transmissão em fluxo do Spark](apache-spark-streaming-overview.md)
* [Criar tarefas de transmissão em fluxo do Spark altamente disponíveis no YARN](apache-spark-streaming-high-availability.md)
