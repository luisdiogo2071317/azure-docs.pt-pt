---
title: Criar tarefas de transmissão em fluxo do Spark altamente disponíveis no YARN - Azure HDInsight | Microsoft Docs
description: Como configurar a transmissão em fluxo do Spark para um cenário de elevada disponibilidade.
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
ms.topic: article
ms.date: 01/26/2018
ms.author: ramoha
ms.openlocfilehash: bbb4da02cbe4b0685c715c4cd6bd7b15c6b5cce0
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="create-high-availability-spark-streaming-jobs-with-yarn"></a>Criar tarefas de transmissão em fluxo do Spark de elevada disponibilidade com YARN

Transmissão em fluxo do Spark permite-lhe implementar aplicações de dimensionáveis, débito elevado, com tolerância a falhas de processamento de fluxos de dados. Pode ligar a aplicações de transmissão em fluxo do Spark num cluster do HDInsight Spark para uma variedade de origens de dados, como a Event Hubs do Azure IoT Hub do Azure, Kafka, Flume, Twitter, ZeroMQ, sockets em bruto de TCP ou pela monitorização do sistema de ficheiros do HDFS para as alterações. Transmissão em fluxo do Spark suporta tolerância a falhas com a garantia que qualquer evento indicado é processado exatamente uma vez, mesmo com uma falha de nó.

Transmissão em fluxo do Spark cria tarefas de longa execução durante os quais é possível aplicar as transformações de dados e, em seguida, emitir os resultados para sistemas de ficheiros instalados, bases de dados, dashboards e a consola. Transmissão em fluxo do Spark processa micro-lotes de dados, recolhendo em primeiro um lote de eventos ao longo de um intervalo de tempo definido. Em seguida, esse batch é enviado para processamento e de saída. Intervalos de tempo do batch são definidos habitualmente em frações de um segundo.

![Transmissão em fluxo do Spark](./media/apache-spark-streaming-high-availability/spark-streaming.png)

## <a name="dstreams"></a>DStreams

Transmissão em fluxo do Spark representa um fluxo contínuo de dados com um *fluxo discretizado* (DStream). Este DStream pode ser criado a partir de origens de entrada como Event Hubs ou Kafka ou ao aplicar as transformações no DStream outro. Quando um evento chega à sua aplicação de transmissão em fluxo do Spark, o evento é armazenado de forma fiável. Ou seja, os dados do evento são replicados para que vários nós tenham uma cópia do mesmo. Isto garante que a falha de qualquer nó único não resultará na perda do evento.

O Spark core utiliza *resiliente distribuídas conjuntos de dados* (RDDs). RDDs distribuir dados em vários nós do cluster, onde cada nó, geralmente, mantém os dados completamente dentro da memória para um melhor desempenho. Cada RDD representa eventos recolhidos ao longo de um intervalo de lote. Quando o intervalo de lote decorrido, transmissão em fluxo do Spark produz um novo RDD que contém todos os dados desse intervalo. Este conjunto de RDDs é recolhido para uma DStream. Uma aplicação de transmissão em fluxo do Spark processa os dados armazenados em RDD cada lote.

![O Spark DStream](./media/apache-spark-streaming-high-availability/DStream.png)

## <a name="spark-structured-streaming-jobs"></a>Tarefas do Spark estruturados de transmissão em fluxo

O Spark estruturados de transmissão em fluxo foi introduzida no Spark 2.0 como um motor de análise para utilização na transmissão em fluxo de dados estruturados. O Spark estruturados de transmissão em fluxo utiliza SparkSQL motor APIs de criação de batches. Tal como acontece com de transmissão em fluxo do Spark, Spark estruturados de transmissão em fluxo é executado as computações ao longo do que chegam continuamente micro-lotes de dados. Spark estruturados de transmissão em fluxo representa um fluxo de dados como uma tabela de entrada com linhas ilimitadas. Ou seja, a tabela de entrada continua a crescer como novos dados são recebidos. Esta tabela de entrada continuamente é processada por uma consulta de execução longa, e os resultados são escritos para uma tabela de saída.

![Spark estruturado de transmissão em fluxo](./media/apache-spark-streaming-high-availability/structured-streaming.png)

Transmissão em fluxo estruturados, dados chega ao sistema em é ingeridos imediatamente para a tabela de entrada. Escrever consultas que executam operações em relação a esta tabela de entrada. O resultado de consulta gera outra tabela, denominada tabela de resultados. A tabela de resultados contém resultados da sua consulta, a partir da qual pode desenhar dados para enviar para um arquivo de dados externo essa base de dados relacional. O *intervalo acionador* define o período de tempo para quando os dados são processados da tabela de entrada. Por predefinição, a transmissão em fluxo estruturada processa os dados logo que chega. No entanto, também pode configurar o acionador para ser executada num intervalo de tempo, pelo que os dados de transmissão em fluxo são processados em lotes baseados no tempo. Os dados na tabela de resultados podem ser completamente atualizados sempre que há novos dados para que inclui todos os dados de saída, uma vez que iniciou a consulta de transmissão em fluxo (*modo concluído*), ou apenas pode conter apenas os dados que são novos desde o último tempo de processamento de consulta (*acrescentar modo*).

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Criar tarefas de transmissão em fluxo do Spark com tolerância a falhas

Para criar um ambiente altamente disponíveis para as tarefas de transmissão em fluxo do Spark, comece por codificação as tarefas individuais para recuperação em caso de falha. Estas tarefas de Self-recuperação são com tolerância a falhas.

RDDs tem várias propriedades ajudá-lo de elevada disponibilidade e tolerância a falhas tarefas de transmissão em fluxo do Spark:

* Lotes de dados de entrada armazenados no RDDs como um DStream são replicadas automaticamente na memória para tolerância a falhas.
* Podem ser recomputed dados perdidos devido a falha de trabalho de dados de entrada replicados na técnicos diferentes, desde que os nós de trabalho estão disponíveis.
* Recuperação rápida de falhas pode ocorrer dentro de um segundo, como a recuperação a partir de falhas/stragglers ocorre através de cálculo na memória.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Exatamente-uma vez semântica com transmissão em fluxo do Spark

Para criar uma aplicação que processa cada evento, uma vez (e apenas uma vez), considere como reiniciar todos os pontos de sistema de falha depois de ter um problema e como pode evitar perda de dados. Exatamente-assim que necessita de semântica que não existem dados perde-se em qualquer momento, e que o processamento da mensagem é reiniciável, independentemente de onde ocorre a falha. Consulte [criar Spark de transmissão em fluxo de tarefas com exatamente-uma vez eventos processamento](apache-spark-streaming-exactly-once.md).

## <a name="spark-streaming-and-yarn"></a>Transmissão em fluxo do Spark e o YARN

No HDInsight, o trabalho de cluster é coordenado pelo *ainda outro recurso Negotiator* (YARN). Conceber a elevada disponibilidade para transmissão em fluxo do Spark inclui técnicas para transmissão em fluxo do Spark bem como para os componentes YARN.  Um exemplo de configuração utilizando o YARN é mostrado abaixo. 

![Arquitetura YARN](./media/apache-spark-streaming-high-availability/yarn-arch.png)

As secções seguintes descrevem as considerações de design para esta configuração.

### <a name="plan-for-failures"></a>Plano para falhas

Para criar uma configuração de YARN para elevada disponibilidade, deve planear uma falha de executor ou controlador possíveis. Algumas tarefas de transmissão em fluxo do Spark também incluem requisitos de garantia de dados que necessitam de configuração adicional e a configuração. Por exemplo, uma aplicação de transmissão em fluxo pode ter um requisito comercial para uma perda de dados de zero garantir apesar de erro que ocorre no sistema de transmissão em fluxo de anfitrião ou cluster do HDInsight.

Se um **executor** falhar, respetivas tarefas e os recetores são reiniciados por Spark automaticamente, pelo que não existe nenhuma alteração de configuração necessária.

No entanto, se um **controlador** falhar, em seguida, todos os respetivos executores associadas falharem e blocos de todos os recebido e resultados de computação são perdidos. Para recuperar a partir de uma falha de controlador, utilize *DStream pontos de verificação* conforme descrito em [criar Spark de transmissão em fluxo de tarefas com exatamente-uma vez eventos processamento](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers). Ponto de verificação DStream periodicamente guarda o *direcionado acíclicos* (DAG) de DStreams para o armazenamento com tolerância a falhas, tais como o Storage do Azure.  Ponto de verificação permite Spark estruturados de transmissão em fluxo reiniciar o controlador de falha das informações de ponto de verificação.  Este reinício controlador inicia executores novo e reinicia também recetores.

Para recuperar controladores com DStream pontos de verificação:

* Configurar o reinício automático de controladores no YARN com a definição de configuração `yarn.resourcemanager.am.max-attempts`.
* Definir um diretório do ponto de verificação num sistema de ficheiros compatível com HDFS com `streamingContext.checkpoint(hdfsDirectory)`.
* Reestruturar o código de origem a utilizar pontos de verificação para recuperação, por exemplo:

    ```scala
        def creatingFunc() : StreamingContext = {
            val context = new StreamingContext(...)
            val lines = KafkaUtils.createStream(...)
            val words = lines.flatMap(...)
            ...
            context.checkpoint(hdfsDir)
        }

        val context = StreamingContext.getOrCreate(hdfsDir, creatingFunc)
        context.start()
    ```

* Configurar a recuperação de perda de dados, permitindo o registo de escrita antecipada (WLAN) com `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`e desativar a replicação de dentro da memória para a entrada DStreams com `StorageLevel.MEMORY_AND_DISK_SER`.

Para resumir, utilizando o ponto de verificação + Quisição + recetores fiáveis, será capaz de fornecer ", pelo menos, uma vez" recuperação de dados:

* Exatamente uma vez, desde que os dados recebidos não são perdidos e as saídas são ambos idempotent ou transacional.
* Exatamente uma vez, com a nova abordagem Kafka direta que utiliza Kafka um registo replicado, em vez de utilizarem recetores ou WALs.

### <a name="typical-concerns-for-high-availability"></a>Problemas típicos para elevada disponibilidade

* É mais difícil monitorizar as tarefas de transmissão em fluxo de tarefas do batch. As tarefas de transmissão em fluxo do Spark estão normalmente de longa execução e YARN não agrega os registos de até que seja uma tarefa concluída.  Pontos de verificação do Spark são perdidos durante a aplicação ou atualizações de Spark e terá de limpar o diretório de ponto de verificação durante a atualização.

* Configure o seu modo de cluster YARN para executar controladores, mesmo se um cliente falhar. Para configurar o reinício automático de controladores:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Spark e a IU de transmissão em fluxo do Spark têm um sistema de métricas configurável. Também pode utilizar bibliotecas adicionais, tais como Graphite/Grafana para transferir as métricas de dashboard, tais como 'registos de núm processados', 'Utilização de memória/GC no controlador de & executores', 'atraso total', 'utilização do cluster de', etc. Em estruturados transmissão em fluxo versão 2.1 ou superior, pode utilizar `StreamingQueryListener` para recolher métricas adicionais.

* Deve segmentar as tarefas de longa execução.  Quando uma aplicação de transmissão em fluxo do Spark for submetida para o cluster, tem de ser definida a fila YARN onde a tarefa é executada. Pode utilizar um [YARN capacidade programador](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) para submeter tarefas de longa execução ao separar as filas.

* Encerre corretamente a aplicação de transmissão em fluxo. Se os desvios são conhecidos e todos os Estado da aplicação é armazenado externamente, pode parar programaticamente a aplicação de transmissão em fluxo em lugar adequado. Uma técnica consiste em utilizar "thread hooks" no Spark, verificando para um sinalizador externo cada *n* segundos. Também pode utilizar um *ficheiro marcador* que é criada no HDFS ao iniciar a aplicação, em seguida, remover quando que pretende parar. Uma abordagem de ficheiro de marcador, utilize um thread separado na sua aplicação de Spark que chama o código semelhante a isto:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral de transmissão em fluxo do Spark](apache-spark-streaming-overview.md)
* [Criar tarefas de transmissão em fluxo do Spark com exatamente-uma vez eventos de processamento](apache-spark-streaming-exactly-once.md)
* [Spark de execução longa tarefas no YARN de transmissão em fluxo](http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/) 
* [Transmissão em fluxo estruturada: Tolerância a semântica de falhas](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Fluxos discretizados: Um modelo com tolerância a falhas de processamento de fluxo dimensionável](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
