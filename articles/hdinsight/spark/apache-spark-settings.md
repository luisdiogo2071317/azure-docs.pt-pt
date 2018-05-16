---
title: Configurar definições de Spark - Azure HDInsight | Microsoft Docs
description: Como configurar o Spark para um cluster do HDInsight.
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
ms.date: 01/26/2018
ms.author: maxluk
ms.openlocfilehash: db61cc81f51772aa98c034f1bfdf51777cfd68e7
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="configure-spark-settings"></a>Configurar as definições do Spark

Um cluster do HDInsight Spark inclui uma instalação da biblioteca do Apache Spark.  Cada cluster do HDInsight inclui os parâmetros de configuração predefinida para todos os respetivos serviços instalados, incluindo Spark.  Um aspeto chave da gestão de um cluster de Hadoop do HDInsight está a monitorizar carga de trabalho, incluindo as tarefas do Spark, para se certificar de que as tarefas em execução de forma previsível. Para executar melhor Spark tarefas, considere a configuração de cluster físico ao determinar como otimizar a configuração de lógica do cluster.

O cluster do Apache Spark do HDInsight predefinida inclui os seguintes nós: três nós de ZooKeeper, dois nós principais e um ou mais nós de trabalho:

![Arquitetura do Spark HDInsight](./media/apache-spark-settings/spark-hdinsight-arch.png)

O número de VMs e os tamanhos VM, para os nós do cluster do HDInsight também podem afetar a sua configuração de Spark. Valores de configuração do HDInsight não predefinidos, muitas vezes, necessitam de valores de configuração do Spark não predefinidas. Quando cria um cluster do HDInsight Spark, são apresentadas sugeridos tamanhos de VM para cada uma dos componentes. Atualmente a [tamanhos de VM do Linux com otimização de memória](../../virtual-machines/linux/sizes-memory.md) do Azure são D12 v2 ou superior.

## <a name="spark-versions"></a>Versões do Spark

Utilize a versão de Spark melhor para o cluster.  O serviço de HDInsight inclui várias versões do Spark e o HDInsight em si.  Cada versão do Spark inclui um conjunto de predefinições de cluster.  

Quando cria um novo cluster, seguem-se as versões de Spark atuais para escolher entre:

![Versões do Spark](./media/apache-spark-settings/spark-version.png)

O Spark 2 pode executar muito maior do que o Spark 1. x. O Spark 2 tem um número de otimizações de desempenho, tais como Tungsten, Otimização da consulta de Catalyst e muito mais.  

> [!NOTE]
> A versão predefinida do Apache Spark no HDInsight serviço podem ser alteradas sem aviso prévio. Se tiver uma dependência de versão, a Microsoft recomenda que especifique essa versão específico quando criar clusters utilizando o .NET SDK/Azure PowerShell e da CLI do Azure.

Apache Spark tem três localizações de configuração do sistema:

* Propriedades de Spark controlar a maioria dos parâmetros da aplicação e pode ser definidas utilizando um `SparkConf` objeto, ou através das propriedades do sistema de Java.
* As variáveis de ambiente podem ser utilizadas para definir o computador, definições, tais como o endereço IP, através de `conf/spark-env.sh` script em cada nó.
* O registo pode ser configurado através de `log4j.properties`.

Quando seleciona uma versão específica do Spark, o seu cluster inclui as definições de configuração predefinida.  Pode alterar os valores de configuração do Spark predefinidos utilizando um ficheiro de configuração personalizado do Spark.  Um exemplo é mostrado abaixo.

```
    spark.hadoop.io.compression.codecs org.apache.hadoop.io.compress.GzipCodec
    spark.hadoop.mapreduce.input.fileinputformat.split.minsize 1099511627776
    spark.hadoop.parquet.block.size 1099511627776
    spark.sql.files.maxPartitionBytes 1099511627776
    spark.sql.files.openCostInBytes 1099511627776
```

O exemplo apresentado acima substitui vários valores predefinidos para os parâmetros de configuração do cinco Spark.  Estes são o codec de compressão, Hadoop MapReduce dividir o tamanho mínimo e tamanhos de bloco parquet bem como a partição Spar SQL e tamanhos de ficheiro aberto valores predefinidos.  Estas alterações de configuração são escolhidas porque os dados associados e tarefas (neste exemplo, dados genomic) têm características específicas, que irão efetuar a melhor com as seguintes definições de configuração personalizada.

---

## <a name="view-cluster-configuration-settings"></a>Ver definições de configuração de cluster

Verifique as definições de configuração de cluster do HDInsight atuais antes de executar a otimização do desempenho no cluster. Iniciar o Dashboard do portal do Azure HDInsight clicando a **Dashboard** ligação no painel de cluster do Spark. Inicie sessão com o nome de utilizador e palavra-passe do administrador do cluster.

IU da Web do Ambari é apresentada uma vista de dashboard de métricas de utilização de recursos de cluster chave.  O Dashboard do Ambari mostra a configuração do Apache Spark e outros serviços que têm instalado. O Dashboard inclui um **do histórico de configurações** separador, onde pode ver informações de configuração para todos os serviços instalados, incluindo Spark.

Para ver os valores de configuração do Apache Spark, selecione **do histórico de configurações**, em seguida, selecione **Spark2**.  Selecione o **folhas** separador, em seguida, selecione o `Spark` (ou `Spark2`, consoante a sua versão) ligação na lista de serviço.  É apresentada uma lista de valores de configuração para o cluster:

![Configurações do Spark](./media/apache-spark-settings/spark-config.png)

Para ver e alterar os valores de configuração individuais do Spark, selecione qualquer ligação com a palavra "spark" no título de ligação.  Configurações para Spark incluem ambos os valores de configuração personalizada e avançadas destas categorias:

* Predefinições de Spark2 personalizadas
* Propriedades de métricas de Spark2 personalizadas
* Predefinições de Spark2 avançadas
* Spark2-env avançada
* Avançadas spark2 hive-site-substituição

Se criar um conjunto de não predefinidas de valores de configuração, em seguida, também pode ver o histórico de atualizações de configuração.  Este histórico de configuração pode ser útil para ver que configuração predefinida não tem um desempenho ideal.

> [!NOTE]
> Para ver, mas não alterar, definições de configuração do cluster de Spark comuns, selecione o **ambiente** separador de nível superior no **IU da tarefa de Spark** interface.

## <a name="configuring-spark-executors"></a>Configurar o executor do Spark

O diagrama seguinte mostra os objetos da chave Spark: o programa de controlador e o contexto de Spark associados e o Gestor de clusters e respetivos *n* nós de trabalho.  Cada nó de trabalho inclui um Executor, uma cache, e *n* instâncias de tarefas.

![Objetos de cluster](./media/apache-spark-settings/spark-arch.png)

Tarefas do Spark utilizam recursos de trabalho, particularmente memória, pelo que é comum para ajustar os valores de configuração do Spark para o nó de trabalho executores.

São três os parâmetros de chave que são, muitas vezes, tendo em conta o para otimizar as configurações de Spark para melhorar os requisitos da aplicação `spark.executor.instances`, `spark.executor.cores`, e `spark.executor.memory`. Um Executor é um processo iniciado para uma aplicação de Spark. Um Executor é executado no nó de trabalho e é responsável pelas tarefas para a aplicação. Para cada cluster, o número predefinido de executor e os tamanhos de executor é calculado com base no número de nós de trabalho e o tamanho de nó de trabalho. Estes são armazenados no `spark-defaults.conf` sobre os nós principais do cluster.  Pode editar estes valores num cluster em execução, selecionando o **personalizada spark-predefinições** ligação na IU da web do Ambari.  Depois de efetuar alterações, lhe for pedida da IU para **reiniciar** todos os serviços afetados.

> [!NOTE]
> Estes parâmetros de três configuração podem ser configurados ao nível do cluster (para todas as aplicações executadas no cluster) e também especificados para cada aplicação individuais.

Outra origem de informações sobre os recursos a ser utilizada pelo executor Spark é a IU da aplicação de Spark.  Na IU do Spark, selecione o **executores** separador para apresentar vistas de resumo e detalhes da configuração e recursos consumidos pelo executor.  Estas vistas podem ajudar a determinar se deve alterar os valores predefinidos de executor de Spark para todo o cluster ou um determinado conjunto de execuções de tarefa.

![Executores do Spark](./media/apache-spark-settings/spark-executors.png)

Em alternativa, pode utilizar a API de REST do Ambari para verificar programaticamente definições de configuração de cluster do HDInsight e Spark.  Estão disponíveis mais informações no [referência da API do Ambari no GitHub](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Consoante a carga de trabalho do Spark, pode determinar a que uma configuração de Spark não predefinidas fornece mais otimizado Spark execuções de tarefa.  Deve efetuar benchmark testar com cargas de trabalho de exemplo para validar as configurações de cluster não predefinidas.  Alguns dos parâmetros comuns que pode considerar ajustar são:

* `--num-executors` Define o número de executor.
* `--executor-cores` Define o número de núcleos para cada executor. Recomendamos que utilize executores middle-sized, tal como outros processos também consumam uma parte da memória disponível.
* `--executor-memory` controla o tamanho de memória (tamanho de área dinâmica para dados) de cada executor no YARN e terá de deixar alguns memória para a sobrecarga de execução.

Eis um exemplo de dois nós de trabalho com valores de configuração diferente:

![Duas configurações de nó](./media/apache-spark-settings/executor-config.png)

A lista seguinte mostra o executor do Spark chave parâmetros de memória.

* `spark.executor.memory` Define a quantidade total de memória disponível para um executor.
* `spark.storage.memoryFraction` (predefinida ~ 60%) define a quantidade de memória disponível para armazenar RDDs persistentes.
* `spark.shuffle.memoryFraction` (predefinida ~ 20%) define a quantidade de memória reservada para shuffle.
* `spark.storage.unrollFraction` e `spark.storage.safetyFraction` (perfazendo em ~ 30% do total de memória) - estes valores são utilizados internamente pelo Spark e não deve ser alterados.

YARN controla a soma de máxima de memória utilizada pelos contentores em cada nó de Spark. O diagrama seguinte mostra as relações de cada nó entre objetos de configuração do YARN e objetos de Spark.

![Gestão de memória do Spark YARN](./media/apache-spark-settings/yarn-spark-memory.png)

## <a name="change-parameters-for-an-application-running-in-jupyter-notebook"></a>Altere os parâmetros para uma aplicação em execução no bloco de notas do Jupyter

Clusters do Spark no HDInsight incluem um número de componentes, por predefinição. Cada um destes componentes inclui configuração os valores predefinidos, que podem ser substituídos conforme necessário.

* Spark Core - Spark Core, Spark SQL, Spark, transmissão em fluxo APIs, GraphX e o MLlib
* Anaconda - um python do Gestor de pacote
* O Livy - a API de REST do Apache Spark, utilizado para submeter tarefas remotas para um cluster do Spark do HDInsight
* Notas do Jupyter e Zeppelin - IU interativa baseada no browser para interagir com o cluster do Spark
* O controlador ODBC - liga-se a clusters do Spark no HDInsight para ferramentas de business intelligence (BI), tais como o Microsoft Power BI e o Tableau

Para aplicações em execução no bloco de notas do Jupyter, utilize o `%%configure` alterações do comando para efetuar a configuração de dentro do bloco de notas do próprio. Estas alterações de configuração serão aplicadas para as tarefas de Spark são executadas a partir da sua instância do bloco de notas. Deve efetuar essas alterações no início da aplicação, antes de executar a sua primeira célula do código. A configuração foi alterada é aplicada à sessão Livy quando é criada.

> [!NOTE]
> Para alterar a configuração de uma fase posterior da aplicação, utilize o `-f` parâmetro (force). No entanto, o progresso de todas as na aplicação serão perdido.

O código abaixo mostra como alterar a configuração para uma aplicação em execução no bloco de notas do Jupyter.

```
    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}
```

## <a name="conclusion"></a>Conclusão

Existem várias definições de configuração de núcleos que precisa de monitorizar e ajustar para garantir que as tarefas do Spark executadas de forma previsível e performant. Estas definições ajudam a determinar a configuração de cluster do Spark melhor para as cargas de trabalho específicas.  Também terá de monitorizar a execução de longa execução e/ou recurso de consumo execuções de tarefa de Spark.  O Centro de desafios mais comuns em torno da pressão de memória devido a configurações incorrecta (executores particularmente incorretamente tamanho), execução longa operações e tarefas, o que resultam em operações Cartesian.

## <a name="next-steps"></a>Passos Seguintes

* [Componentes do Hadoop e versões disponíveis com o HDInsight?](../hdinsight-component-versioning.md)
* [Gerir os recursos de um cluster do Spark no HDInsight](apache-spark-resource-manager.md)
* [Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](../hdinsight-hadoop-provision-linux-clusters.md)
* [Configuração do Apache Spark](https://spark.apache.org/docs/latest/configuration.html)
* [Com o Spark no YARN](https://spark.apache.org/docs/latest/running-on-yarn.html)
