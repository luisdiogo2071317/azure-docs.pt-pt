---
title: Criar tarefas de transmissão em fluxo do Spark com exatamente-uma vez eventos processamento - Azure HDInsight
description: Como configurar a transmissão em fluxo do Spark para processar um evento, uma vez e apenas uma vez.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/26/2018
ms.openlocfilehash: ae170e90cede26bd6a43fcc10b93fcd7490d838f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618826"
---
# <a name="create-spark-streaming-jobs-with-exactly-once-event-processing"></a>Criar tarefas de transmissão em fluxo do Spark com exatamente-uma vez o evento de processamento

Aplicações de processamento de Stream tomar diferentes abordagens para como eles lidam com mensagens de processamento novamente após alguns falha no sistema:

* Pelo menos uma vez: cada mensagem é garantida a ser processada, mas podem obter processado mais de uma vez.
* No máximo uma vez: cada mensagem pode ou não podem ser processada. Se uma mensagem é processada, ele é apenas processado uma vez.
* Exatamente uma vez: é garantida que cada mensagem a ser processado uma vez e apenas uma vez.

Este artigo mostra-lhe como configurar a transmissão em fluxo do Spark para obter exatamente-processamento uma vez.

## <a name="exactly-once-semantics-with-spark-streaming"></a>Exatamente-uma vez semântica com transmissão em fluxo do Spark

Em primeiro lugar, considere como a todos os pontos de falha de sistema reiniciar depois de ter um problema e como pode evitar a perda de dados. Uma aplicação de transmissão em fluxo do Spark tem:

* Uma origem de entrada
* Um ou mais processos de destinatário que efetuar pull dos dados da origem de entrada
* Tarefas que processam os dados
* Um sink de saída
* Um processo de driver que gere a tarefa de execução longa

Exatamente-assim que a semântica de exige que não se perdem a qualquer momento e esse processamento de mensagens é reinicializável, independentemente de onde a falha ocorre.

### <a name="replayable-sources"></a>Origens replayable

A aplicação de transmissão em fluxo do Spark está a ler os eventos de origem tem de ser *replayable*. Isso significa que em casos em que a mensagem foi obtida, em seguida, o sistema falha antes da mensagem pode ser persistente ou processada, mas, a origem tem de fornecer a mesma mensagem novamente.

No Azure, os Hubs de eventos do Azure e o Kafka no HDInsight fornecem replayable origens. Outro exemplo de uma origem replayable é um sistema de ficheiros tolerante a falhas, como o HDFS, blobs de armazenamento do Azure, ou do Azure Data Lake Store, onde todos os dados são mantidos para sempre e em qualquer momento pode ser lidos novamente os dados em sua totalidade.

### <a name="reliable-receivers"></a>Recetores fiáveis

No Spark Streaming, origens, como os Hubs de eventos e Kafka têm *recetores fiáveis*, onde cada destinatário mantém o controle do seu progresso a origem de leitura. Um recetor fiável persiste o estado para o armazenamento tolerante a falhas, dentro do ZooKeeper ou em pontos de verificação Spark Streaming escritos no HDFS. Se um destinatário tal falhar e posterior reiniciado, pode escolher onde parou.

### <a name="use-the-write-ahead-log"></a>Utilizar o registo de escrita-Ahead

Transmissão em fluxo do Spark suporta a utilização de um registo de escrita-Ahead, em que cada evento recebido é escrito pela primeira vez ao diretório de ponto de verificação do Spark no armazenamento tolerante a falhas e, em seguida, armazenado num Resiliente distribuído conjunto de dados (RDD). No Azure, o armazenamento tolerante a falhas é HDFS apoiadas pelo armazenamento do Azure ou do Azure Data Lake Store. Na sua aplicação de transmissão em fluxo do Spark, o registo de escrita-Ahead está ativado para todos os recetores definindo a `spark.streaming.receiver.writeAheadLog.enable` definição de configuração para `true`. O registo de escrita-Ahead fornece tolerância a falhas para falhas do driver e o executor.

Para trabalhadores de tarefas em execução com os dados de eventos, cada RDD é por definição, ambos replicados e distribuídos em várias funções de trabalho. Se uma tarefa falhar porque a função de trabalho em execução que falhou, a tarefa será reiniciada na outra função de trabalho que tenha uma réplica dos dados do evento, portanto, o evento não é perdido.

### <a name="use-checkpoints-for-drivers"></a>Utilizar pontos de verificação para drivers

Os controladores de tarefa tem de ser reinicializável. No caso de falha de driver de execução da aplicação Spark Streaming, demora para baixo com o mesmo todos os recetores em execução, tarefas e qualquer RDDs armazenar dados de eventos. Neste caso, terá de ser capazes de guardar o progresso da tarefa para que pode continuá-lo mais tarde. Isto é conseguido ao ponto de verificação a direcionado acíclicos Graph (DAG) de DStream periodicamente para armazenamento tolerante a falhas. Os metadados do DAG incluem a configuração usada para criar a aplicação de transmissão em fluxo, as operações que definem o aplicativo e qualquer lotes que são colocados em fila, mas ainda não foi concluídas. Estes metadados permite que um controlador com falha ser reiniciado a partir das informações de ponto de verificação. Quando o driver é reiniciado, ele iniciará recetores novos próprios de recuperar os dados do evento no RDDs do Log de escrita-Ahead.

Pontos de verificação estão ativados em transmissão em fluxo do Spark em duas etapas. 

1. O objeto de StreamingContext, configure o caminho de armazenamento para os pontos de verificação:

    val ssc = new StreamingContext(spark, Seconds(1))  ssc.checkpoint("/path/to/checkpoints")

    No HDInsight, esses pontos de verificação deverá ser guardados para o armazenamento de predefinido ligado ao seu cluster, o armazenamento do Azure ou do Azure Data Lake Store.

2. Em seguida, especifica um intervalo de ponto de verificação (em segundos) a DStream. A cada intervalo, derivados do evento de entrada de dados de estado são mantidos no armazenamento. Dados de estado persistente podem reduzir a computação necessária ao reconstruir o estado do evento de origem.

    val lines = ssc.socketTextStream("hostname", 9999)  lines.checkpoint(30)  ssc.start()  ssc.awaitTermination()

### <a name="use-idempotent-sinks"></a>Utilize os sinks de idempotentes

O coletor de destino ao qual a tarefa escreve resultados tem de ser capaz de lidar com a situação em que é fornecido o mesmo resultado mais de uma vez. O coletor tem de ser capaz de detectar esses resultados duplicados e ignorá-las. Uma *idempotentes* coletor pode ser chamado várias vezes com os mesmos dados sem alteração de estado.

Pode criar idempotentes sinks através da implementação de lógica que começa por verificar a existência do resultado da entrada no arquivo de dados. Se o resultado já existir, a gravação deve aparecer com êxito a partir da perspetiva de sua tarefa do Spark, mas na realidade, seu arquivo de dados ignorados os dados duplicados. Se o resultado não existir, o coletor deverão inserir este resultado de novo para o seu armazenamento. 

Por exemplo, poderia usar um procedimento armazenado com a base de dados do Azure SQL que insere eventos numa tabela. Este procedimento armazenado procura primeiro o evento por campos de chave, e somente quando nenhum evento correspondente encontrado é o registo inserido na tabela.

Outro exemplo é usar um sistema de arquivos particionada, como blobs de armazenamento do Azure ou o Azure Data Lake store. Neste caso sua lógica de sink não é necessário verificar a existência de um ficheiro. Se o ficheiro que representa o evento existir, é simplesmente substituído com os mesmos dados. Caso contrário, é criado um novo ficheiro no caminho calculado.

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral da transmissão em fluxo do spark](apache-spark-streaming-overview.md)
* [Criar tarefas de transmissão em fluxo do Spark altamente disponíveis no YARN](apache-spark-streaming-high-availability.md)
