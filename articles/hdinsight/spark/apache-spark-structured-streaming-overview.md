---
title: O Spark estruturados de transmissão em fluxo no Azure HDInsight | Microsoft Docs
description: Como utilizar aplicações do Spark estruturados de transmissão em fluxo em clusters do HDInsight Spark.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: maxluk
ms.openlocfilehash: f3eccfb63b127f3cbb8f9d95ec2fa802861d8c9d
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="overview-of-spark-structured-streaming"></a>Descrição geral do Spark estruturado de transmissão em fluxo

O Spark estruturados de transmissão em fluxo permite-lhe implementar aplicações de dimensionáveis, débito elevado, com tolerância a falhas de processamento de fluxos de dados. Transmissão em fluxo estruturada baseia-se após o motor de Spark SQL e melhora após as construções de pacotes de dados do Spark SQL e conjuntos de dados para que possa escrever a transmissão em fluxo as consultas da mesma forma teriam de escrever consultas de batch.  

Aplicações de transmissão em fluxo estruturadas executam em clusters do HDInsight Spark e ligar a transmissão em fluxo de dados de Kafka, um socket (para fins de depuração) TCP, armazenamento do Azure ou do Azure Data Lake Store. As duas última opções, que dependem dos serviços de armazenamento externo, permitem-lhe ver para novos ficheiros adicionados para o armazenamento e processar os respetivos conteúdos, como se se foram transmitidas em sequência. 

Transmissão em fluxo estruturada cria uma consulta de longa execução durante o qual aplicar operações para os dados de entrada, tais como seleção, projeção, agregação, modos de janela e associar a transmissão em fluxo DataFrame com referência DataFrames. Em seguida, enviar os resultados ao armazenamento de ficheiros (Blobs Storage do Azure ou Data Lake Store) ou para qualquer arquivo de dados utilizando código personalizado (como base de dados do SQL Server ou o Power BI). Transmissão em fluxo estruturada também fornece a saída para a consola de depuração localmente e uma tabela em memória para que possa ver os dados gerados para depuração no HDInsight. 

![Com o HDInsight e Spark de processamento de fluxo estruturada de transmissão em fluxo ](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]
> O Spark estruturados de transmissão em fluxo está a substituir o Spark transmissão em fluxo (DStreams). Doravante, transmissão em fluxo estruturada receberá melhoramentos e manutenção, ao DStreams será em modo de manutenção apenas. Transmissão em fluxo estruturada não está atualmente como funcionalidade-concluir como DStreams para as origens e sinks que suporta a Box, por isso, avaliar os requisitos para escolher o Spark adequado a opção de processamento de fluxo. 

## <a name="streams-as-tables"></a>Fluxos como tabelas

O Spark estruturados de transmissão em fluxo representa um fluxo de dados como uma tabela que é unbounded em profundidade, ou seja, a tabela continua a crescer, conforme novos dados são recebidos. Isto *tabela de entrada* continuamente é processado por uma consulta de longa execução e os resultados enviados para um *tabela de saída*:

![Estruturados conceito de transmissão em fluxo](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

Transmissão em fluxo estruturados, dados chega ao sistema em é imediatamente ingeridos para uma tabela de entrada. Escrever consultas (utilizando as APIs de conjunto de dados e DataFrame) que executam operações em relação a esta tabela de entrada. O resultado de consulta gera outra tabela, o *tabela de resultados*. A tabela de resultados contém os resultados da sua consulta, a partir da qual pode desenhar dados para um arquivo de dados externo, essa base de dados relacional. A temporização de quando os dados são processados da tabela de entrada é controlada pelo *intervalo acionador*. Por predefinição, o intervalo de Acionador é zero, para que transmissão em fluxo estruturada tenta processar os dados logo que chega. Na prática, isto significa que, assim que a transmissão em fluxo estruturada é feita processar a execução da consulta anterior, inicia outro processamento executar em relação a quaisquer dados recentemente recebidos. Pode configurar o acionador para ser executada com intervalo, para que os dados de transmissão em fluxo são processados em lotes baseados no tempo. 

Os dados nos resultados de tabelas podem conter apenas os dados que são novos desde o último tempo a consulta foi processados (*acrescentar modo*), ou a tabela estar completamente atualizada sempre que há novos dados para a tabela inclui todos os dados de saída uma vez que iniciou a consulta de transmissão em fluxo (*modo concluído*).

### <a name="append-mode"></a>Acrescentar modo

No acrescente modo, apenas as linhas adicionadas à tabela de resultados, uma vez que a última execução de consulta estão presentes na tabela de resultados e escritos para armazenamento externo. Por exemplo, a consulta mais simples apenas copia todos os dados da tabela de entrada para a tabela de resultados será alterada. Sempre que um intervalo de Acionador decorrido, os novos dados são processados e as linhas que representam os dados novos são apresentados na tabela de resultados. 

Considere um cenário em que são de processamento a telemetria de sensores de temperatura, tais como um thermostat. Assumir que o primeiro acionador processados um evento no tempo 00:01 para dispositivo 1 com uma leitura de temperatura de 95 graus. O primeiro acionador da consulta, apenas a linha com tempo 00:01 é apresentado na tabela de resultados. Em vez 00:02 quando chega a outro evento, apenas nova linha é a linha com tempo 00:02 e, por isso, a tabela de resultados iria conter apenas que uma linha.

![A transmissão em fluxo estruturada acrescentar modo](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Quando utilizar acrescentar modo, a consulta deverá ser projeções aplicar (selecionar as colunas que cares sobre), filtragem (diretriz apenas as linhas com correspondência determinadas condições) ou associar (aumentar os dados com dados de uma tabela de pesquisa estático). Acrescente torna de modo mais fácil emitir apenas novos dados relevantes aponta para armazenamento externo.

### <a name="complete-mode"></a>Modo concluído

Considere o cenário mesmo, desta vez utilizando o modo de conclusão. No modo concluído, a tabela de saída completa é atualizada cada acionador para que a tabela inclui dados não apenas do acionador mais recente executar, mas da executa todos os. Pode utilizar o modo de conclusão para copiar os dados será alterados da tabela de entrada para a tabela de resultados. Em cada execução accionada, novas linhas de resultado são apresentados juntamente com todas as linhas anteriores. A tabela de resultados de saída irá armazenar todos os dados recolhidos, uma vez que iniciou a consulta e, eventualmente, executaria memória esgotada. Modo concluído destina-se com as consultas de agregação que resumir os dados de entrada de alguma forma e outros cada tabela de resultados de Acionador é atualizado com um resumo de novo. 

Assuma até ao momento existem visão de cinco segundos de dados já processados e está na altura de processar os dados para o segundo sexto. A tabela de entrada tem eventos para o tempo 00:01 e tempo 00:03. O objetivo desta consulta de exemplo é dar a temperatura média do dispositivo a cada cinco segundos. A implementação desta consulta aplica-se um agregado que demora a todos os valores que enquadra-se cada período de 5 segundos, averages a temperatura e produz uma linha para a temperatura média ao longo dessa intervalo. No final da primeira janela de 5 segundos, existem duas cadeias de identificação: (00:01, 1, 95) e (00:03, 1, 98). Por isso, para a janela 00:00-00:05 a agregação produz uma cadeia de identificação com a temperatura média de 96.5 graus. Na janela de 5-segundo seguinte, há apenas um ponto de dados em tempo 00:06, pelo que a temperatura média resultante é 98 graus. Na hora 00:10, utilizando o modo concluído, a tabela de resultados tem as linhas para ambos os windows 00:00-00:05 e 00:05-00:10 porque a consulta devolve todas as linhas agregadas, não apenas os novos. Por conseguinte, a tabela de resultados continua a crescer à medida que são adicionadas novas janelas.    

![Estruturados concluído modo de transmissão em fluxo](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Nem todas as consultas utilizando o modo concluído fará com que a tabela a crescer sem limites.  Considere o exemplo anterior, que em vez de média, menos a temperatura pela janela de tempo que uma média em vez disso, por ID de dispositivo. A tabela de resultados contém um número fixo de linhas (uma por dispositivo) com a temperatura média para o dispositivo em todos os pontos de dados recebidos a partir desse dispositivo. Como são recebidas temperatures novo, a tabela de resultados for atualizada para que o médias na tabela são sempre atuais. 

## <a name="components-of-a-spark-structured-streaming-application"></a>Componentes de uma aplicação Spark estruturados de transmissão em fluxo

Uma consulta de exemplo simples pode resumem o leituras de temperatura pelo hour-long windows. Neste caso, os dados são armazenados nos ficheiros JSON no armazenamento do Azure (anexado como armazenamento de predefinido para o cluster de HDInsight):

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Estes ficheiros JSON são armazenados no `temps` subpasta por baixo do contentor de cluster do HDInsight. 

### <a name="define-the-input-source"></a>Definir a origem de entrada

Configure primeiro uma DataFrame que descreve a origem dos dados e quaisquer definições requeridas dessa origem. Este exemplo desenha a partir dos ficheiros JSON no armazenamento do Azure e aplica-se um esquema aos mesmos durante a leitura.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>Aplicar a consulta

Em seguida, aplica uma consulta que contém as operações pretendidas contra DataFrame de transmissão em fluxo. Neste caso, uma agregação agrupa todas as linhas no windows de 1 hora e, em seguida, calcula os temperatures máxima, mínimas e média numa janela que 1 hora.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>Definir o sink de saída

Em seguida, defina o destino para as linhas que são adicionadas à tabela de resultados dentro de cada intervalo de Acionador. Neste exemplo produz apenas todas as linhas para uma tabela em memória `temps` que mais tarde pode consultar com SparkSQL. Modo de saída completa assegura que todas as linhas para todos os windows são saída sempre.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>Iniciar a consulta

Inicie a consulta de transmissão em fluxo e execute até que é recebido um sinal de terminação. 

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>Ver os resultados

Enquanto a consulta está em execução, no SparkSession mesma, pode executar uma consulta de SparkSQL contra o `temps` onde estão armazenados os resultados da consulta de tabela. 

    select * from temps

Esta consulta gera resultados semelhantes ao seguinte:


| janela |  min(Temp) | AVG(Temp) | Max(Temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Para obter detalhes sobre a API de fluxo estruturada do Spark, juntamente com os dados de entrada origens, de operações e de saída sinks-suporta, consulte [Spark estruturados de transmissão em fluxo guia de programação](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html).

## <a name="checkpointing-and-write-ahead-logs"></a>Pontos de verificação e registos de escrita antecipada

Para fornecer resiliência e tolerância a falhas, transmissão em fluxo estruturada depende *pontos de verificação* para garantir que esse fluxo pode continuar sem interrupção, mesmo com falhas de nó o processamento. No HDInsight, o Spark cria pontos de verificação para o armazenamento durável, o Storage do Azure ou para o Data Lake Store. Estes pontos de verificação armazenam as informações de progresso sobre a consulta de transmissão em fluxo. Além disso, a transmissão em fluxo estruturada utiliza um *registo escrita antecipada* (WLAN). O WLAN captura dados de transmissões em que tem sido recebidos, mas ainda não processados por uma consulta. Se ocorrer uma falha e é reiniciado processamento do WLAN, quaisquer eventos recebidos de origem não foram perdidos.

## <a name="deploying-spark-streaming-applications"></a>Implementar aplicações de transmissão em fluxo do Spark

Normalmente, criar uma aplicação de transmissão em fluxo do Spark localmente num ficheiro JAR e, em seguida, implementá-la para o Spark no HDInsight ao copiar o ficheiro JAR no armazenamento de predefinição ligado ao cluster do HDInsight. Pode começar a sua aplicação com as APIs REST do LIVY disponíveis do seu cluster utilizando uma operação POST. O corpo da publicação inclui um documento JSON que fornece o caminho para o seu JAR, o nome da classe cujo método principal define e executa a aplicação de transmissão em fluxo e, opcionalmente, os requisitos de recursos da tarefa (por exemplo, o número de executor, memória e núcleos) , e as definições de configuração necessita de código da aplicação.

![Implementar uma aplicação de transmissão em fluxo do Spark](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Também pode ser verificado o estado de todas as aplicações com um pedido GET relativamente um ponto final LIVY. Por fim, pode terminar uma aplicação em execução ao emitir um pedido de eliminação relativamente ao ponto final LIVY. Para obter detalhes sobre a API de LIVY, consulte [tarefas remotas com o LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Passos Seguintes

* [Criar um cluster do Apache Spark no HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [O Spark estruturados guia de programação de transmissão em fluxo](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Iniciar tarefas do Spark remotamente com o LIVY](apache-spark-livy-rest-interface.md)
