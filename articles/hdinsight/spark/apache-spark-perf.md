---
title: Otimizar as tarefas do Spark para desempenho - Azure HDInsight | Microsoft Docs
description: "Mostra as estratégias comuns para o melhor desempenho dos clusters do Spark."
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 64ddb70f071a9fadc6fef64dcd3506c6d6255481
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="optimize-spark-jobs"></a>Otimizar as tarefas do Spark

Saiba como otimizar a configuração de cluster do Spark para a carga de trabalho específica.  O desafio mais comuns é a pressão de memória, devido a configurações incorrecta (tamanho particularmente mal executores), execução longa operações e tarefas que resultam em operações Cartesian. Pode acelerar tarefas com a colocação em cache adequado e permitindo para [desfasamento de dados](#optimize-joins-and-shuffles). Para obter o melhor desempenho, monitorizar e rever execuções de tarefa de Spark de longa execução e o consumo de recursos.

As secções seguintes descrevem as otimizações de tarefa de Spark comuns e as recomendações.

## <a name="choose-the-data-abstraction"></a>Escolha a abstração de dados

Spark 1. x utiliza RDDs dados abstratos e, em seguida, Spark 2 introduzida DataFrames e conjuntos de dados. Considere os seguintes merits relativos:

* **DataFrames**
    * Melhor opção na maioria das situações
    * Fornece a Otimização da consulta através de Catalyst
    * Geração de código fase todo
    * Acesso direto à memória
    * Baixa sobrecarga libertação de memória (GC)
    * Não como programador compatível com como conjuntos de dados, como não são as verificações de tempo de compilação ou programação do objeto de domínio
* **DataSets**
    * Boa em complexas pipelines ETL onde o impacto do desempenho é aceitável
    * Não é bom em agregações em que o impacto de desempenho pode ser considerável
    * Fornece a Otimização da consulta através de Catalyst
    * Compatível com programadores, fornecendo as verificações de programação e tempo de compilação do objeto de domínio
    * Adiciona o overhead de serialização/anulação da serialização
    * Elevada sobrecarga de GC
    * Interrompe a geração de código fase todo
* **RDDs**
    * No Spark 2, não terá de utilizar RDDs, a menos que necessita para criar um novo RDD personalizado
    * Sem otimização de consulta através de Catalyst
    * Não existem geração de código de fase de todo
    * Elevada sobrecarga de GC
    * Tem de utilizar legado de 1. x do Spark APIs

## <a name="use-optimal-data-format"></a>Utilize o formato de dados ideal

O Spark suporta vários formatos, tais como csv, json, xml, parquet, orc e avro. O Spark pode ser expandido para suportar vários formatos mais com origens de dados externas - para obter mais informações, consulte [Spark pacotes](https://spark-packages.org).

O formato para um desempenho melhor é parquet com *compressão snappy*, que é o predefinido no Spark 2. x. Parquet armazena os dados no formato columnar e está altamente otimizado no Spark.

## <a name="select-default-storage"></a>Selecione o armazenamento de predefinido

Quando cria um novo cluster do Spark, tem a opção para selecionar o Blob Storage do Azure ou do Azure Data Lake Store como armazenamento de predefinido do cluster. Ambas as opções dão-lhe a vantagem de armazenamento de longa duração em clusters transitórios, pelo que os dados são automaticamente eliminados quando elimina o cluster. Pode recriar um cluster transitório e continuar a aceder aos seus dados.

| Tipo de arquivo | Sistema de Ficheiros | Velocidade | Transitório | Casos de utilização |
| --- | --- | --- | --- | --- |
| Armazenamento de Blobs do Azure | **wasb:**//url/ | **Standard** | Sim | Cluster transitório |
| Azure Data Lake Store | **adl:**//url/ | **Faster** | Sim | Cluster transitório |
| Local HDFS | **hdfs:**//url/ | **Fastest** | Não | Cluster de 24x7 interativa |

## <a name="use-the-cache"></a>Utilizar a cache

Spark fornece os seus próprios mecanismos de colocação em cache nativos, que podem ser utilizados como através de métodos diferentes `.persist()`, `.cache()`, e `CACHE TABLE`. Esta nativo colocação em cache é efetiva com conjuntos de dados pequenos, bem como em pipelines ETL, onde tem resultados intermédios da cache. No entanto, o Spark nativo a colocação em cache atualmente não funcionar bem com criação de partições, uma vez que uma tabela em cache não manter os dados de criação de partições. É uma técnica de colocação em cache mais genérica e fiável *colocação em cache de camada de armazenamento*.

* Spark nativo a colocação em cache (não recomendado)
    * Boa para conjuntos de dados pequenos.
    * Não work, com a criação de partições, que podem ser alterados em futuras versões do Spark.

* Armazenamento nível colocação em cache (recomendado)
    * Podem ser implementadas utilizando [Alluxio](http://www.alluxio.org/).
    * Utiliza na memória e a colocação em cache de SSD.

* HDFS local (recomendado)
    * `hdfs://mycluster`caminho.
    * Utiliza a colocação em cache de SSD.
    * Dados em cache serão perdidos quando eliminar o cluster, que requerem uma reconstrução da cache.

## <a name="use-memory-efficiently"></a>Utilizar memória de forma eficiente

O Spark funciona, colocar dados na memória, para que gerir recursos de memória é um aspeto chave da otimizar a execução de tarefas do Spark.  Existem várias técnicas que pode aplicar a utilizar memória do seu cluster de forma eficiente.

* Prefere partições de dados mais pequenas e de conta para o tamanho dos dados, tipos e a distribuição na sua estratégia de criação de partições.
* Considere o mais recente, mais eficiente [serialização de dados de Kryo](https://github.com/EsotericSoftware/kryo), em vez de serialização de Java predefinido.
* Preferir utilizar o YARN, como que separa `spark-submit` pelo batch.
* Monitorizar e otimizar as definições de configuração do Spark.

Para sua referência, a estrutura de memória do Spark e alguns parâmetros de memória de executor de chave são apresentados na imagem seguinte.

### <a name="spark-memory-considerations"></a>Considerações sobre a memória do Spark

Se estiver a utilizar o YARN, YARN controla a soma de máxima de memória utilizada por todos os contentores em cada nó de Spark.  O diagrama seguinte mostra os objetos principais e relações.

![Gestão de memória do Spark YARN](./media/apache-spark-perf/yarn-spark-memory.png)

Para o endereço 'fora de memória' mensagens, tente:

* Reveja Shuffles de gestão do DAG. Reduzir ao lado do mapa reducting, previamente partição (ou bucketize) de origem de dados, maximizar shuffles único e reduzir a quantidade de dados enviados.
* Prefere `ReduceByKey` com o respetivo limite de memória fixo para `GroupByKey`, que fornece as agregações, modos de janela e outras funções, mas tem um limite de memória unbounded ann.
* Prefere `TreeReduce`, que não mais trabalho no executores ou partições, `Reduce`, que tem todo o trabalho no controlador.
* Tire partido DataFrames em vez dos objetos RDD de nível inferior.
* Crie ComplexTypes encapsular ações, como "Principais N", várias agregações ou operações de modos de janela.

## <a name="optimize-data-serialization"></a>Otimizar a serialização de dados

Tarefas do Spark são distribuídas para que serialização de dados adequada é importante para o melhor desempenho.  Existem duas opções de serialização para Spark:

* A serialização de Java é a predefinição.
* Serialização de Kryo é um formato mais recente e pode resultar em mais rápido e mais compactar serialização de Java.  Kryo requer que registar as classes no seu programa e, se ainda não suporta todos os tipos serializáveis.

## <a name="use-bucketing"></a>Utilizar bucketing

Bucketing é semelhante à criação de partições de dados, mas cada registo pode conter um conjunto de valores de coluna em vez de apenas um. Bucketing funciona bem para criação de partições em números de grandes dimensões (por milhões deles ou mais) de valores, tais como identificadores de produto. Um registo é determinado por hash a chave de registo da linha. Tabelas agrupadas oferecem otimizações exclusivas porque armazenam metadados sobre como estivessem agrupados e ordenados.

Algumas funcionalidades bucketing avançadas são:

* Otimização da consulta com base nas informações de metadados bucketing
* Agregações otimizadas
* Associações otimizadas

Pode utilizar a criação de partições e bucketing ao mesmo tempo.

## <a name="optimize-joins-and-shuffles"></a>Otimizar as associações e shuffles

Se tiver tarefas lentas num associação ou Shuffle, a causa é provavelmente *desfasamento de dados*, que é asymmetry nos seus dados de tarefa. Por exemplo, uma tarefa de mapa poderá demorar 20 segundos, mas em execução uma tarefa de onde os dados estão associado a um ou fora de posição demora horas.   Para corrigir dados dissimetrias, que deve salt a chave completa ou utilize um *isolada salt* para apenas algumas subconjunto de chaves.  Se estiver a utilizar um salt isolado, deve continuar a filtrar para isolar o subconjunto de chaves salted em associações de mapa. Outra opção consiste em introduz uma coluna de registo e agregar previamente no registos primeiro.

Outro fator causar associações lentas pode ser o tipo de associação. Por predefinição, Spark utiliza o `SortMerge` tipo de associação. Este tipo de associação é mais adequado para grandes conjuntos de dados, mas é viáveis dispendioso porque, tem primeiro de ordenação à esquerda e direita lados de dados antes de intercalação-los.

A `Broadcast` associação é mais adequada para conjuntos de dados menores, ou onde é muito menor do que o lado do lado da associação a um. Este tipo de associação difunde lado para todos os executores e requer mais memória difunde em geral.

Pode alterar o tipo de associação na sua configuração através da definição `spark.sql.autoBroadcastJoinThreshold`, ou pode definir uma sugestão de associação com as APIs de DataFrame (`dataframe.join(broadcast(df2))`).

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")
sql("SELECT col1, col2 FROM V_JOIN")
```

Se estiver a utilizar tabelas agrupadas, em seguida, tiver uma terceira aderir ao tipo, o `Merge` associação. Um conjunto de dados previamente particionado e previamente ordenado corretamente irá ignorar a fase de ordenação dispendiosas de um `SortMerge` associação.

A ordem das associações é importante, especialmente nas consultas mais complexas. Começar a utilizar a associa mais seletiva. Além disso, mover associações que aumentam o número de linhas após agregações sempre que possível.

Para gerir o paralelismo, especificamente no caso de associações Cartesian, pode adicionar estruturas aninhadas, modos de janela e talvez ignorar os passos de uma ou mais na sua tarefa de Spark.

## <a name="customize-cluster-configuration"></a>Personalizar a configuração de cluster

Consoante a carga de trabalho de cluster do Spark, pode determinar que uma configuração de Spark não predefinidas resultaria em obter mais informações com otimização de execução da tarefa de Spark.  Efetue benchmark testar com cargas de trabalho de exemplo para validar as configurações de cluster não predefinidas.

Seguem-se alguns parâmetros comuns, que pode ajustar:

* `--num-executors`Define o número apropriado de executor.
* `--executor-cores`Define o número de núcleos para cada executor. Normalmente, deve ter executores middle-sized, tal como outros processos consumam algumas da memória disponível.
* `--executor-memory`Define o tamanho de memória para cada executor, que controla o tamanho da pilha no YARN. Deve deixar alguns memória para a sobrecarga de execução.

### <a name="select-the-correct-executor-size"></a>Selecione o tamanho de executor correto

Ao decidir a configuração de executor, considere a sobrecarga de coleção (GC) de libertação da memória de Java.

* Fatores para reduzir o tamanho de executor:
    1. Reduza o tamanho da pilha abaixo 32 GB para manter GC gerais < 10%.
    2. Reduza o número de núcleos para manter GC gerais < 10%.

* Fatores para aumentar o tamanho de executor:
    1. Reduza a comunicação sobrecarga entre executores.
    2. Reduza o número de ligações abertas entre executores (N2) em clusters maiores (> 100 executores).
    3. Aumente o tamanho da pilha para acomodar para tarefas intensivas em memória.
    4. Opcional: Reduza o overhead de memória de por executor.
    5. Opcional: Aumentar a simultaneidade e de utilização ao sobre-subscrever a CPU.

Como uma geral regra prática quando selecionar o tamanho de executor:
    
1. Comece com 30 GB por executor e distribuir núcleos máquina disponíveis.
2. Aumente o número de núcleos de executor para clusters maiores (> 100 executores).
3. Aumentar ou diminuir o tamanhos com base na versão de avaliação executa e nos fatores anteriores, tais como a sobrecarga de GC.

Quando executar consultas em simultâneo, considere o seguinte:

1. Comece por 30 GB por executor e todos os núcleos da máquina.
2. Criar Spark paralelo várias aplicações sobre-subscrever a CPU (aproximadamente melhoramento de latência de 30%).
3. Distribua consultas em todas as aplicações paralelas.
4. Aumentar ou diminuir o tamanhos com base na versão de avaliação executa e nos fatores anteriores, tais como a sobrecarga de GC.

Monitorize o desempenho das consultas para valores atípicos ou outros problemas de desempenho, observando a vista de linha cronológica, gráfico SQL, estatísticas de tarefas e assim sucessivamente. Por vezes, algumas do executor são mais lentas do que outros, e tarefas muito demorar mais tempo a executar. Isto acontece frequentemente em clusters maiores (> 30 nós). Neste caso, divida o trabalho num grande número de tarefas, pelo que pode compensar o Programador de tarefas lentas. Por exemplo, ter, pelo menos, duas vezes como muitas tarefas como o número de núcleos de executor na aplicação. Também pode ativar a execução speculative das tarefas com `conf: spark.speculation = true`.

## <a name="optimize-job-execution"></a>Otimizar a execução da tarefa

* Conforme necessário, por exemplo, se utilizar os dados duas vezes, em seguida, colocar em cache-lo em cache.
* Variáveis de difusão para todos os executores. As variáveis só são serializadas depois, resultando em pesquisas mais rápidas.
* Utilize o conjunto de threads em controladores, o que resulta numa operação mais rápida para muitas tarefas.

Monitorize as tarefas em execução regularmente para problemas de desempenho. Se precisar de mais ampla de determinados problemas, considere uma das seguinte ferramentas de criação de perfis de desempenho de:

* [Ferramenta de PAL Intel](https://github.com/intel-hadoop/PAT) monitoriza a CPU, armazenamento e utilização de largura de banda de rede.
* [Controlo da missão Oracle Java 8](http://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) perfis código Spark e o executor.

Chave de desempenho de consultas do Spark 2 é o motor de Tungsten, que depende de geração de código de fase de todo. Em alguns casos, a geração de código inteiro fase pode ser desativada. Por exemplo, se utilizar um tipo não mutável (`string`) na expressão de agregação, `SortAggregate` aparece em vez de `HashAggregate`. Por exemplo, para um melhor desempenho, tente o seguinte procedimento e, em seguida, volte a ativar a geração de código:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Passos Seguintes

* [Depurar tarefas de Spark em execução no Azure HDInsight](apache-spark-job-debugging.md)
* [Gerir os recursos de um cluster do Spark no HDInsight](apache-spark-resource-manager.md)
* [Utilizar a API de REST do Spark para submeter as tarefas remotas para um cluster do Spark](apache-spark-livy-rest-interface.md)
* [Otimização do Spark](https://spark.apache.org/docs/latest/tuning.html)
* [A forma como otimizar o seu Spark, na verdade, tarefas, funcionam](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Serialização de Kryo](https://github.com/EsotericSoftware/kryo)
