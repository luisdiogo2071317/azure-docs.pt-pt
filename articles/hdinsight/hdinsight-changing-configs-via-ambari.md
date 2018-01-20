---
title: "Otimizar as configurações de cluster com o Ambari - Azure HDInsight | Microsoft Docs"
description: Utilize a IU da web do Ambari para configurar e otimizar os clusters do HDInsight.
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: ashish
ms.openlocfilehash: 74c1b3298cd7b6ffd5b4a60e2fa78ed733232f92
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="use-ambari-to-optimize-hdinsight-cluster-configurations"></a>Utilizar Ambari para otimizar a configurações de cluster do HDInsight

O HDInsight fornece clusters do Apache Hadoop para aplicações de processamento de dados em grande escala. Gestão, monitorização e otimizar estas clusters com vários nós complexos podem ser um desafio. [Apache Ambari](http://ambari.apache.org/) é uma interface web para gerir e monitorizar clusters do HDInsight com Linux.  Para os clusters do Windows, utilize o Ambari [REST API](hdinsight-hadoop-manage-ambari-rest-api.md).

Para uma introdução ao utilizar a IU da Web do Ambari, consulte [gerir clusters do HDInsight utilizando a IU da Web do Ambari](hdinsight-hadoop-manage-ambari.md)

Inicie sessão no Ambari em `https://CLUSTERNAME.azurehdidnsight.net` com as suas credenciais de cluster. O ecrã inicial apresenta uma descrição geral do dashboard.

![Dashboard do Ambari](./media/hdinsight-changing-configs-via-ambari/ambari-dashboard.png)

A IU da web do Ambari pode ser utilizado para gerir anfitriões, serviços, alertas, configurações e vistas. Não é possível utilizar Ambari para criar um cluster do HDInsight, atualizar serviços, gerir pilhas e versões, desativar ou recommission anfitriões ou adicionar serviços ao cluster.

## <a name="manage-your-clusters-configuration"></a>Gerir a configuração do cluster

Definições de configuração ajudam a otimizar um serviço específico. Para modificar as definições de configuração de um serviço, selecione o serviço do **serviços** barra lateral (no lado esquerdo) e, em seguida, navegue para o **folhas** separador na página de detalhes do serviço.

![Serviços de barra lateral](./media/hdinsight-changing-configs-via-ambari/services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>Modificar tamanho da pilha NameNode Java

O tamanho da pilha de NameNode Java depende de vários fatores, tais como a carga no cluster, o número de ficheiros e os números de blocos. O tamanho predefinido de 1 GB funciona bem com a maior parte dos clusters, apesar de algumas cargas de trabalho podem exigir mais ou menos memória. 

Para modificar o tamanho da pilha NameNode Java:

1. Selecione **HDFS** da barra lateral serviços e navegue para o **folhas** separador.

    ![Configuração do HDFS](./media/hdinsight-changing-configs-via-ambari/hdfs-config.png)

2. Localizar a definição **tamanho da pilha de NameNode Java**. Também pode utilizar o **filtro** caixa de texto para o tipo e encontrar uma definição específica. Selecione o **caneta** ícone junto do nome da definição.

    ![Tamanho da pilha de NameNode Java](./media/hdinsight-changing-configs-via-ambari/java-heap-size.png)

3. Escreva o novo valor na caixa de texto e, em seguida, prima **Enter** para guardar a alteração.

    ![Editar o tamanho da pilha de NameNode Java](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit.png)

4. O tamanho da pilha de NameNode Java é alterado para 2 GB de 1 GB.

    ![Editar o tamanho da pilha de NameNode Java](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

5. Guardar as alterações ao clicar na verde **guardar** botão na parte superior do ecrã de configuração.

    ![Guardar alterações](./media/hdinsight-changing-configs-via-ambari/save-changes.png)

## <a name="hive-optimization"></a>Otimização de ramo de registo

As secções seguintes descrevem as opções de configuração para otimizar o desempenho global do ramo de registo.

1. Para modificar os parâmetros de configuração do ramo de registo, selecione **Hive** da barra lateral serviços.
2. Navegue para o **folhas** separador.

### <a name="set-the-hive-execution-engine"></a>Definir o motor de execução do Hive

Ramo de registo fornece dois motores de execução: MapReduce e Tez. Tez é mais rápido do que MapReduce. Clusters do HDInsight com Linux tem Tez, como o motor de execução de predefinição. Para alterar o motor de execução:

1. No ramo **folhas** separador, escreva **motor de execução** na caixa Filtro.

    ![Motor de execução de pesquisa](./media/hdinsight-changing-configs-via-ambari/search-execution.png)

2. O **otimização** valor de predefinido da propriedade é **Tez**.

    ![Otimização - Tez](./media/hdinsight-changing-configs-via-ambari/optimization-tez.png)

### <a name="tune-mappers"></a>Otimizar mappers

Hadoop tenta dividir (*mapa*) num único ficheiro em vários ficheiros e processo resultante ficheiros em paralelo. O número de mappers depende o número de divisões. Os seguintes parâmetros de configuração de dois unidade o número de divisões para o motor de execução Tez:

* `tez.grouping.min-size`: Inferior limite no tamanho de uma divisão agrupada com um valor predefinido de 16 MB (16,777,216 bytes).
* `tez.grouping.max-size`: Limite superior de tamanho de uma divisão agrupada com um valor predefinido de 1 GB (1,073,741,824 bytes).

Como um desempenho regra geral, diminua ambos estes parâmetros para melhorar a latência, aumentar o débito mais.

Por exemplo, para definir quatro mapeador tarefas para um tamanho de dados de 128 MB, deverá definir ambos os parâmetros para 32 MB cada (33,554,432 bytes).

1. Para modificar os parâmetros de limite, navegue para o **folhas** separador do serviço Tez. Expanda o **geral** painel e localize o `tez.grouping.max-size` e `tez.grouping.min-size` parâmetros.

2. Defina os dois parâmetros para **33,554,432** bytes (32 MB).

    ![Tamanhos de agrupamento no Tez](./media/hdinsight-changing-configs-via-ambari/tez-grouping-size.png)
 
Estas alterações afetam todas as tarefas de Tez entre o servidor. Para obter um resultado ideal, escolha os valores de parâmetros adequados.

### <a name="tune-reducers"></a>Otimizar reducers

ORC e Snappy oferecem elevado desempenho. No entanto, ramo de registo pode ter poucos reducers por predefinição, fazendo com que congestionamentos.

Por exemplo, imagine que tem um tamanho de dados de entrada de 50 GB. Que dados ORC formatar com compressão Snappy é de 1 GB. O número de reducers necessário como estimativas de ramo de registo: (número de entrada de bytes para mappers / `hive.exec.reducers.bytes.per.reducer`).

Com as predefinições, neste exemplo é 4 reducers.

O `hive.exec.reducers.bytes.per.reducer` parâmetro especifica o número de bytes processadas por reducer. O valor predefinido é 64 MB. Este valor de otimização para baixo aumenta paralelismo e poderá melhorar o desempenho. Otimização demasiado baixo pode também produzir reducers demasiados, afetar negativamente potencialmente o desempenho. Este parâmetro é baseado nos seus requisitos de dados específica, definições de compressão e outros fatores ambientais.

1. Para modificar o parâmetro, navegue para o ramo do **folhas** separador e localize o **dados por Reducer** parâmetro na página de definições.

    ![Dados por Reducer](./media/hdinsight-changing-configs-via-ambari/data-per-reducer.png)
 
2. Selecione **editar** para modificar o valor a 128 MB (134,217,728 bytes) e, em seguida, prima **Enter** para guardar.

    ![Dados por Reducer - editado](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    Atribuído um tamanho de entrada de 1.024 MB, com 128 MB de dados por reducer, existem 8 reducers (1024/128).

3. Um valor incorreto para a **dados por Reducer** parâmetro poderá resultar num grande número de reducers, afetar negativamente o desempenho das consultas. Para limitar o número máximo de reducers, defina `hive.exec.reducers.max` para um valor adequado. O valor predefinido é 1009.

### <a name="enable-parallel-execution"></a>Ativar a execução paralela

Uma consulta do Hive é executada nas fases de um ou mais. Se as fases independentes podem ser executadas em paralelo, que irá aumentar o desempenho das consultas.

1.  Para ativar a execução paralela da consulta, navegue para o ramo do **configuração** separador e procure o `hive.exec.parallel` propriedade. O valor predefinido é falso. Altere o valor true e, em seguida, prima **Enter** para guardar o valor.
 
2.  Para limitar o número de tarefas a ser executadas em paralelo, modifique o `hive.exec.parallel.thread.number` propriedade. O valor predefinido é de 8.

    ![Paralelo exec de ramo de registo](./media/hdinsight-changing-configs-via-ambari/hive-exec-parallel.png)


### <a name="enable-vectorization"></a>Ativar vectorization

Ramo de registo processa dados linha por linha. Vectorization direciona o ramo de registo para processar dados em blocos de 1.024 linhas em vez de uma linha numa altura. Vectorization só é aplicável para o formato de ficheiro ORC.

1. Para ativar uma execução de consulta vectorized, navegue para o ramo do **folhas** separador e procure o `hive.vectorized.execution.enabled` parâmetro. O valor predefinido é verdadeiro para Hive 0.13.0 ou posterior.
 
2. Para ativar a execução vectorized para o lado reduza da consulta, defina o `hive.vectorized.execution.reduce.enabled` parâmetro como true. O valor predefinido é falso.

    ![Execução do Hive vectorized](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Ativar a otimização baseada no custo (CBO)

Por predefinição, o Hive segue um conjunto de regras para localizar um plano de execução de consultas. Otimização baseada no custo (CBO) avalia vários esquemas para executar uma consulta e atribui um custo para cada plano, em seguida, determina o plano cheapest para executar uma consulta.

Para ativar CBO, navegue para o ramo do **folhas** separador e procure `parameter hive.cbo.enable`, em seguida, mude o botão de alternar para **no**.

![Configuração CBO](./media/hdinsight-changing-configs-via-ambari/cbo.png)

Os seguintes parâmetros de configuração adicionais aumentam o desempenho das consultas do Hive quando CBO está ativado:

* `hive.compute.query.using.stats`

    Quando definido como true, o ramo de registo utiliza estatísticas armazenadas no respetivo metastore responder às consultas simples como `count(*)`.

    ![Estatísticas CBO](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    Estatísticas de coluna são criadas quando CBO está ativada. Ramo de registo utiliza estatísticas de coluna, o que são armazenadas na metastore, otimizar as consultas. A obter estatísticas de coluna para cada coluna demora mais tempo quando o número de colunas é elevado. Quando definido em false, esta definição desativa a obter estatísticas de coluna do metastore.

    ![Estatísticas de coluna de conjunto de estatísticas de ramo de registo](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    As estatísticas da partição básica, tais como o número de linhas, o tamanho de dados e o tamanho de ficheiro são armazenadas no metastore. Quando definido como VERDADEIRO, a partição estatísticas são recuperadas do metastore. Se for FALSO, o tamanho do ficheiro é obtido a partir do sistema de ficheiros e é obtido o número de linhas do esquema de linha.

    ![Estatísticas de partição de conjunto de estatísticas de ramo de registo](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Ativar a compressão intermédio

Tarefas de mapa criar intermédios ficheiros que são utilizados pelas tarefas reducer. Intermédio compressão diminui o tamanho de ficheiro intermédio.

Tarefas do Hadoop são, normalmente, e/s condicionada apoio. A compressão de dados pode acelerar a e/s e transferência de rede global.

Os tipos de compressão disponíveis são:

| Formato | Ferramenta | Algoritmo | Extensão de ficheiro | Divisíveis? |
| -- | -- | -- | -- | -- |
| Gzip | Gzip | DEFLATE | .gz | Não |
| Bzip2 | Bzip2 | Bzip2 |.bz2 | Sim |
| LZO | Lzop | LZO | .lzo | Sim, se indexada |
| snappy | N/A | snappy | snappy | Não |

Regra geral, o método de compressão divisíveis é ter importante, caso contrário, serão criadas mappers muito poucos. Se os dados de entrada são o texto, `bzip2` é a melhor opção. Para o formato ORC, Snappy é a opção de compressão mais rápida.

1. Para ativar a compressão intermédio, navegue para o ramo do **folhas** separador e, em seguida, defina o `hive.exec.compress.intermediate` parâmetro como true. O valor predefinido é falso.

    ![Exec comprimir intermédio de ramo de registo](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]
    > Para comprimir ficheiros intermédios, escolha um codec de compressão com inferior da CPU de custos, mesmo que o codec não tem uma saída de compressão elevada.

2. Para definir o codec de compressão intermédio, adicione a propriedade personalizada `mapred.map.output.compression.codec` para o `hive-site.xml` ou `mapred-site.xml` ficheiro.

3. Para adicionar uma definição personalizada:

    a. Navegue para o ramo do **folhas** separador e selecione o **avançadas** separador.

    b. Sob o **avançadas** separador, localize e expanda o **personalizado. o ramo de registo-site** painel.

    c. Clique na ligação **Adicionar propriedade** na parte inferior do painel de site do ramo de registo personalizado.

    d. Na janela Adicionar propriedade, introduza `mapred.map.output.compression.codec` como a chave e `org.apache.hadoop.io.compress.SnappyCodec` como o valor.

    e. Clique em **Adicionar**.

    ![Ramo de registo de propriedade personalizada](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Isto irá comprimir o ficheiro intermédio utilizando Snappy compressão. Assim que a propriedade é adicionada, aparece no painel de site do ramo de registo personalizado.

    > [!NOTE]
    > Este procedimento modifica o `$HADOOP_HOME/conf/hive-site.xml` ficheiro.

### <a name="compress-final-output"></a>Comprimir resultado final

O resultado final do ramo de registo também pode ser comprimido.

1. Para comprimir o resultado final do ramo de registo, navegue para o ramo do **folhas** separador e, em seguida, defina o `hive.exec.compress.output` parâmetro como true. O valor predefinido é falso.

2. Para escolher o codec de compressão de saída, adicione o `mapred.output.compression.codec` propriedade personalizada para o painel de site do ramo de registo personalizado, conforme descrito no passo 3 da secção anterior.

    ![Ramo de registo de propriedade personalizada](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Ativar a execução speculative

Execução speculative inicia um determinado número de tarefas duplicados para detetar e a lista de proibições o controlador de tarefas em execução lenta, ao melhorar a execução da tarefa geral por otimizar os resultados de tarefas individuais.

Execução speculative não deve ser ativada para execução longa tarefas de MapReduce com grandes quantidades de entrada.

* Para ativar a execução speculative, navegue para o ramo do **folhas** separador e, em seguida, defina o `hive.mapred.reduce.tasks.speculative.execution` parâmetro como true. O valor predefinido é falso.

    ![Ramo de registo mapred reduzir execução speculative tarefas](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Otimizar partições dinâmicas

Ramo de registo permite a criação de partições dinâmicas ao inserir registos numa tabela, sem predefining todas as partições. Esta é uma funcionalidade poderosa, embora o poderá resultar na criação de um grande número de partições e de um grande número de ficheiros para cada partição.

1. Para o ramo de registo para partições dinâmicas, a `hive.exec.dynamic.partition` valor do parâmetro deve ser verdadeiro (predefinição).

2. Alterar o modo de partição dinâmico para *strict*. No modo restrito, pelo menos uma partição tem de ser estáticos. Isto impede que as consultas sem o filtro de partição na cláusula WHERE, ou seja, *strict* impede que as consultas que todas as partições de análise. Navegue para o ramo do **folhas** separador e, em seguida, defina `hive.exec.dynamic.partition.mode` para **strict**. O valor predefinido é **nonstrict**.
 
3. Para limitar o número de partições dinâmicas a criar, modificar o "parâmetro dos hive.exec.max.dynamic.partitions. O valor predefinido é 5 000.
 
4. Para limitar o número total de partições dinâmicas por nó, modifique `hive.exec.max.dynamic.partitions.pernode`. O valor predefinido é 2000.

### <a name="enable-local-mode"></a>Ativar o modo de local

Modo de local permite que o ramo de registo efetuar todas as tarefas de uma tarefa num único computador ou, por vezes, num único processo. Isto melhora o desempenho das consultas se os dados de entrada são pequenos e a sobrecarga de iniciar tarefas para consultas consome uma percentagem significativa da execução de consulta global.

Para ativar o modo de local, adicione o `hive.exec.mode.local.auto` parâmetro para o painel de site do ramo de registo personalizado, conforme explicado no passo 3 do [ativar a compressão intermédio](#enable-intermediate-compression) secção.

![Ramo de registo exec modo local automática](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Conjunto único MapReduce MultiGROUP por

Quando esta propriedade está definida como true, uma consulta MultiGROUP por com as chaves de grupo por comum gera uma única tarefa de MapReduce.  

Para ativar este comportamento, adicione o `hive.multigroupby.singlereducer` parâmetro para o painel de site do ramo de registo personalizado, conforme explicado no passo 3 do [ativar a compressão intermédio](#enable-intermediate-compression) secção.

![Definir o ramo de registo único MapReduce MultiGROUP através da](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Otimizações de ramo de registo adicionais

As secções seguintes descrevem otimizações adicionais relacionados com o ramo de registo que pode definir.

#### <a name="join-optimizations"></a>Otimizações de associação

O tipo de associação predefinido no ramo de registo é um *shuffle associação*. No ramo de registo, mappers especiais a entrada de leitura em emitir um associação par chave/valor para um ficheiro intermédio. Hadoop ordena e intercala estes pares numa fase de shuffle. Nesta fase shuffle é dispendiosa. Selecionar a associação correta com base nos seus dados pode melhorar significativamente o desempenho.

| Tipo de União | Quando | Como | Definições de ramo de registo | Comentários |
| -- | -- | -- | -- | -- |
| Shuffle associação | <ul><li>Escolha predefinida</li><li>Sempre funciona</li></ul> | <ul><li>Lê a partir da parte de uma das tabelas</li><li>Registos e ordena na chave de associação</li><li>Envia um registo para cada reduce</li><li>A associação é efetuada no lado Reduza</li></ul> | Nenhum Hive significativa definição necessários | Funciona sempre |
| Associação do mapa | <ul><li>Uma tabela pode caber na memória</li></ul> | <ul><li>Lê a tabela pequena na tabela hash de memória</li><li>Fluxos através de parte dos ficheiros grandes</li><li>Associa cada registo da tabela hash</li><li>Associações são o mapeador de individualmente</li></ul> | `hive.auto.confvert.join=true` | Muito rápida, mas limitado |
| Registo de intercalação de ordenação | Se ambas as tabelas são: <ul><li>Ordenar as mesmas</li><li>Bucketed o mesmo</li><li>Associar a coluna ordenada/agrupados</li></ul> | Cada processo: <ul><li>Lê um registo a partir de cada tabela</li><li>Processa a linha com o valor mais baixo</li></ul> | `hive.auto.convert.sortmerge.join=true` | Muito eficiente |

#### <a name="execution-engine-optimizations"></a>Otimizações de motor de execução

Recomendações adicionais para otimizar o motor de execução do Hive:

| Definição | Recomendado | HDInsight predefinido |
| -- | -- | -- |
| `hive.mapjoin.hybridgrace.hashtable` | = TRUE mais segura, mais lenta; FALSE = mais rápida | falso |
| `tez.am.resource.memory.mb` | Limite superior de 4 GB para a maioria dos | Otimizados automaticamente |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="pig-optimization"></a>Otimização do PIg

Propriedades de PIg podem ser modificadas da web do Ambari da IU para otimizar as consultas de Pig. Modificar propriedades de Pig do Ambari diretamente modifica as propriedades de Pig no `/etc/pig/2.4.2.0-258.0/pig.properties` ficheiro.

1. Para modificar as propriedades de Pig, navegue para o Pig **folhas** separador e, em seguida, expanda o **pig propriedades avançadas** painel.

2. Localizar, anule os comentários e altere o valor da propriedade que pretende modificar.

3. Selecione **guardar** no lado direito superior da janela para guardar o novo valor. Algumas propriedades podem exigir um reinício do serviço.

    ![Propriedades avançadas do pig](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)
 
> [!NOTE]
> As definições de nível de sessão substituem os valores de propriedade no `pig.properties` ficheiro.

### <a name="tune-execution-engine"></a>Otimizar o motor de execução

Dois motores de execução estão disponíveis para executar Pig scripts: MapReduce e Tez. Tez é um motor otimizado e é muito mais rápido do que MapReduce.

1. Para modificar o motor de execução, do **pig propriedades avançadas** painel, localizar a propriedade `exectype`.

2. O valor predefinido é **MapReduce**. Altere-o para **Tez**.


### <a name="enable-local-mode"></a>Ativar o modo de local

É semelhante ao ramo, de modo local é utilizada para acelerar as tarefas com relativamente mais pequenas quantidades de dados.

1. Para ativar o modo de local, defina `pig.auto.local.enabled` para **verdadeiro**. O valor predefinido é falso.

2. As tarefas com um tamanho de dados de entrada inferior ao `pig.auto.local.input.maxbytes` valor da propriedade são consideradas como sendo tarefas pequenas. O valor predefinido é de 1 GB.


### <a name="copy-user-jar-cache"></a>Copie a cache de jar do utilizador

PIg copia os ficheiros JAR necessários para UDFs para uma cache distribuída, para que fiquem disponíveis para nós de tarefas. Estes v7 são alterados com frequência. Se estiver ativada, o `pig.user.cache.enabled` v7 sejam colocadas em cache para reutilize-as para as tarefas executadas pelo mesmo utilizador permite a definição. Isto resulta num aumento secundário no desempenho da tarefa.

1. Para ativar, defina `pig.user.cache.enabled` como true. A predefinição é falsa.

2. Para configurar o caminho de base do V7 em cache, definir `pig.user.cache.location` para o caminho de base. A predefinição é `/tmp`.


### <a name="optimize-performance-with-memory-settings"></a>Otimizar o desempenho com as definições da memória

As seguintes definições de memória podem ajudá-lo a otimizar o desempenho de scripts de Pig.

* `pig.cachedbag.memusage`: A quantidade de memória atribuída a uma matriz. Uma matriz é o conjunto de cadeias de identificação. Uma cadeia de identificação é um conjunto ordenado de campos e um campo é um conjunto de dados. Se os dados de uma matriz de ultrapassa a memória alocada, é spilled para disco. O valor predefinido é 0,2, que representa as 20 por cento da memória disponível. Esta memória é partilhada entre todas as matrizes de uma aplicação.

* `pig.spill.size.threshold`: São spilled matrizes excede este limite de tamanho transbordam (em bytes) para o disco. O valor predefinido é 5 MB.


### <a name="compress-temporary-files"></a>Comprimir ficheiros temporários

PIg gera ficheiros temporários durante a execução da tarefa. A compressão os ficheiros temporários origina um aumento do desempenho ao ler ou escrever ficheiros no disco. As seguintes definições podem ser utilizadas para comprimir ficheiros temporários.

* `pig.tmpfilecompression`: Quando verdadeiro, ativa a compressão do ficheiro temporário. O valor predefinido é falso.

* `pig.tmpfilecompression.codec`: O codec de compressão a utilizar para a compressão os ficheiros temporários. Os codecs compressão recomendada são LZO e Snappy inferior utilização da CPU.

### <a name="enable-split-combining"></a>Ativar a divisão combinar

Quando ativada, os ficheiros pequenos são combinados para menos de mapa de tarefas. Isto melhora a eficiência de tarefas com muitos ficheiros pequenos. Para ativar, defina `pig.noSplitCombination` como true. O valor predefinido é falso.


### <a name="tune-mappers"></a>Otimizar mappers

O número de mappers é controlado ao modificar a propriedade `pig.maxCombinedSplitSize`. Esta ação Especifica o tamanho dos dados a ser processado por uma tarefa de mapa único. O valor predefinido é o tamanho do bloco predefinido do sistema de ficheiros. Aumentar este valor resulta num decréscimo do número de tarefas de mapeador de pontos.


### <a name="tune-reducers"></a>Otimizar reducers

O número de reducers é calculado com base no parâmetro `pig.exec.reducers.bytes.per.reducer`. O parâmetro especifica o número de bytes processadas por reducer, por predefinição 1 GB. Para limitar o número máximo de reducers, defina o `pig.exec.reducers.max` propriedade, por predefinição 999.


## <a name="hbase-optimization-with-the-ambari-web-ui"></a>Otimização do HBase com a IU da web do Ambari

Configuração de HBase é modificada do **folhas de HBase** separador. As secções seguintes descrevem algumas das definições de configuração importantes que afetam o desempenho de HBase.

### <a name="set-hbaseheapsize"></a>Definir HBASE_HEAPSIZE

O tamanho da pilha de HBase Especifica a quantidade máxima de área dinâmica para dados a ser utilizada em megabytes por *região* e *mestre* servidores. O valor predefinido é de 1000 MB. Isto deve ser otimizado para a carga de trabalho de cluster.

1. Para modificar, navegue para o **avançada HBase-env** painel no HBase **folhas** separador e, em seguida, localize o `HBASE_HEAPSIZE` definição.

2. Altere o valor predefinido para 5000 MB.

    ![HBASE_HEAPSIZE](./media/hdinsight-changing-configs-via-ambari/hbase-heapsize.png)


### <a name="optimize-read-heavy-workloads"></a>Otimizar cargas de trabalho pesado de leitura

As seguintes configurações são importantes para melhorar o desempenho das cargas de trabalho pesado de leitura.

#### <a name="block-cache-size"></a>Tamanho da cache do bloco

A cache do bloco é a cache de leitura. O tamanho é controlado pelo `hfile.block.cache.size` parâmetro. O valor predefinido é 0.4, que é 40 por cento da memória de servidor de região total. Quanto maior for o tamanho de cache de blocos, será o mais rápidas leituras aleatórias.

1. Para modificar este parâmetro, navegue para o **definições** separador o HBase **folhas** separador e, em seguida, localize **% dos RegionServer alocado para memórias intermédias de leitura**.

    ![Tamanho de cache do bloco de HBase](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)
 
2. Para alterar o valor, selecione o **editar** ícone.


#### <a name="memstore-size"></a>Tamanho do Memstore

Todas as edições são armazenadas na memória intermédia, chamada um *Memstore*. Isto aumenta a quantidade total de dados que podem ser gravados no disco numa única operação, e -velocidades de acesso subsequente para as edições recentes. O tamanho do Memstore é definido pelos dois parâmetros seguintes:

* `hbase.regionserver.global.memstore.UpperLimit`: Define a percentagem máxima do servidor de região Memstore combinado pode utilizar.

* `hbase.regionserver.global.memstore.LowerLimit`: Define a percentagem mínima de servidor de região que Memstore combinado pode utilizar.

Para otimizar para leituras aleatórias, pode reduzir os limites superior e inferiores de Memstore.


#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Número de linhas obtidas durante a análise do disco

O `hbase.client.scanner.caching` definição define o número de linhas a ler a partir do disco quando o `next` método é chamado um scanner.  O valor predefinido é 100. Maior o número, os menos que as chamadas remotas efetuadas do cliente para o servidor de região, resultando em verificações mais rápidas. No entanto, isto também irá aumentar pressão de memória no cliente.

![Número de HBase de linhas obtidas](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]
> Não defina o valor de forma a que o tempo entre a invocação do método seguinte num detetor de é maior do que o limite de tempo de análise. A duração de tempo limite de scanner é definida pelo `hbase.regionserver.lease.period` propriedade.


### <a name="optimize-write-heavy-workloads"></a>Otimizar cargas de trabalho pesado de escrita

As seguintes configurações são importantes para melhorar o desempenho das cargas de trabalho pesado de escrita.


#### <a name="maximum-region-file-size"></a>Tamanho de ficheiro máximo região

HBase armazena dados num formato interno do ficheiro, chamado *HFile*. A propriedade `hbase.hregion.max.filesize` define o tamanho de um único HFile para uma região.  Uma região é dividida em duas regiões se a soma de todos os HFiles numa região é maior do que esta definição.
 
![Filesize máximo HBase HRegion](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Quanto maior for o tamanho do ficheiro de região, o número mais pequeno de divisões. Pode aumentar o tamanho do ficheiro para determinar um valor que resulta no máximo desempenho de escrita.


#### <a name="avoid-update-blocking"></a>Evitar a bloquear a atualização

* A propriedade `hbase.hregion.memstore.flush.size` define o tamanho que Memstore é descarregado no disco. O tamanho predefinido é de 128 MB.

* O multiplicador do Hbase região bloco é definido pela `hbase.hregion.memstore.block.multiplier`. O valor predefinido é 4. O máximo permitido é de 8.

* HBase bloqueia atualizações se for o Memstore (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bytes.

    Com os valores predefinidos de libertação de tamanho e bloco multiplicador, as atualizações estão bloqueadas quando Memstore é 128 * 4 = 512 MB de tamanho. Para reduzir a atualização bloquear contagem, aumente o valor de `hbase.hregion.memstore.block.multiplier`.

![Multiplicador do bloco região de HBase](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)


### <a name="define-memstore-size"></a>Definir o tamanho do Memstore

O tamanho do Memstore é definido pelo `hbase.regionserver.global.memstore.UpperLimit` e `hbase.regionserver.global.memstore.LowerLimit` parâmetros. Definir estes valores para cada outros reduz coloca em pausa durante escreve (também causar libertar mais frequentes) e resulta no desempenho de escrita de aumento.


### <a name="set-memstore-local-allocation-buffer"></a>Definir a memória intermédia do Memstore alocação local

Utilização de memória intermédia de alocação local do Memstore é determinada pela propriedade `hbase.hregion.memstore.mslab.enabled`. Quando ativada (verdadeiro), isto impede a fragmentação de área dinâmica para dados durante a operação de escrita intensivas. O valor predefinido é verdadeiro.
 
![hbase.hregion.memstore.mslab.enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)


## <a name="next-steps"></a>Passos Seguintes

* [Gerir clusters do HDInsight com a IU da web do Ambari](hdinsight-hadoop-manage-ambari.md)
* [API de REST do Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
