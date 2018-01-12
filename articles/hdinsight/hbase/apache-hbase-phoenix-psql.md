---
title: Em massa carregamento para o Apache Phoenix utilizando psql - Azure HDInsight | Microsoft Docs
description: Utilize a ferramenta de psql para carregar dados de carregamento em massa para tabelas de Phoenix.
services: hdinsight
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
ms.date: 11/10/2017
ms.author: ashishth
ms.openlocfilehash: 0a623113ee9f3fe2c0f5f616ecd79b8311a8ffc1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="bulk-load-data-into-phoenix-using-psql"></a>Carregamento em massa para o Phoenix utilizando psql

[O Apache Phoenix](http://phoenix.apache.org/) é um open source, paralelo em grande escala base de dados relacional incorporado no [HBase](../hbase/apache-hbase-overview.md). O Phoenix fornece consultas de como o SQL através de HBase. O Phoenix utiliza controladores JDBC para permitir aos utilizadores criar, eliminar e alterar as tabelas, índices, vistas e sequências e upsert linhas SQL individualmente e em massa. O Phoenix utiliza compilação nativo noSQL, em vez de utilizar o MapReduce para compilar consultas, para criar aplicações de latência baixa por cima do HBase. O Phoenix adiciona coprocessadores para suportar a execução com o código fornecido pelo cliente no espaço de endereço do servidor, ao executar o código localizado conjuntamente com os dados. Isto minimiza a transferência de dados de cliente/servidor.  Trabalhar com dados com o Phoenix no HDInsight, primeiro criar tabelas e, em seguida, carregar dados para o-los.

## <a name="bulk-loading-with-phoenix"></a>Em massa carregar com Phoenix

Existem várias formas de obter os dados no HBase, incluindo através de APIs, uma tarefa de MapReduce com TableOutputFormat, cliente ou inputting os dados manualmente utilizando a shell de HBase. O Phoenix fornece dois métodos para carregar dados CSV para tabelas de Phoenix: um cliente ao carregar a ferramenta com o nome `psql`e numa ferramenta de carregamento em massa baseada em MapReduce.

O `psql` ferramenta é o único thread e é mais adequada para carregar megabytes ou gigabytes de dados. Todos os ficheiros CSV para ser carregado tem de ter a extensão de ficheiro '. csv'.  Também pode especificar ficheiros de script do SQL Server no `psql` linha de comandos com a extensão de ficheiro. 'SQL'.

Em massa carregar com o MapReduce é utilizada para muito maiores volumes de dados, normalmente em cenários de produção, como MapReduce utiliza vários threads.

Antes de iniciar o carregamento de dados, certifique-se de que o Phoenix está ativado e que as definições de tempo limite de consulta estão como esperado.  Aceder ao dashboard do Ambari de cluster do HDInsight, selecione de HBase e, em seguida, no separador de configuração.  Desloque para baixo para verificar se o Apache Phoenix está definido como `enabled` conforme mostrado:

![Definições de Cluster do HDInsight do Apache Phoenix](./media/apache-hbase-phoenix-psql/ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Utilize `psql` a tabelas de carregamento em massa

1. Criar uma nova tabela, em seguida, guardar a consulta com nome de ficheiro `createCustomersTable.sql`.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

2. Copie o ficheiro CSV (conteúdo de exemplo mostrado) como `customers.csv` para um `/tmp/` diretório para carregamento para a tabela recentemente criado.  Utilize o `hdfs` comandos para copiar o ficheiro CSV para a localização de origem pretendido.

    ```
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,Norway
    ... 4997 more rows 
    ```

    ```bash
    hdfs dfs -copyToLocal /example/data/customers.csv /tmp/
    ```

3. Criar uma consulta SQL SELECT para verificar os dados de entrada carregados corretamente, em seguida, guardar a consulta com nome de ficheiro `listCustomers.sql`. Pode utilizar qualquer consulta SQL.
     ```sql
    SELECT Name, Income from Customers group by Country;
    ```

4. Em massa carregar os dados, abrindo um *novo* janela de comandos do Hadoop. Primeiro altere para a localização do diretório de execução com a `cd` comando e, em seguida, utilize o `psql` ferramenta (Python `psql.py` comando). 

    O exemplo seguinte espera que copiou a `customers.csv` ficheiro a partir de uma conta de armazenamento para o diretório temporário local utilizando `hdfs` como no passo 2 acima.

    ```bash
    cd /usr/hdp/current/phoenix-client/bin

    python psql.py ZookeeperQuorum createCustomersTable.sql /tmp/customers.csv listCustomers.sql
    ```

    > [!NOTE] 
    > Para determinar o `ZookeeperQuorum` nome, localize a cadeia de quórum zookeeper no ficheiro `/etc/hbase/conf/hbase-site.xml` com o nome da propriedade `hbase.zookeeper.quorum`.

5. Depois do `psql` conclusão da operação, deverá ver uma mensagem na janela de comandos:

    ```
    CSV Upsert complete. 5000 rows upserted
    Time: 4.548 sec(s)
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>Utilizar o MapReduce a tabelas de carregamento em massa

Para o carregamento de um maior débito distribuído por cluster, utilize a ferramenta de carga do MapReduce. Este carregador primeiro converte todos os dados HFiles e, em seguida, fornece o HFiles criada para HBase.

1. Inicie o carregador de CSV MapReduce, utilizando o `hadoop` comando com jar de cliente o Phoenix:

    ```bash
    hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table CUSTOMERS --input /data/customers.csv
    ```

2. Criar uma nova tabela com uma instrução de SQL, tal como com `CreateCustomersTable.sql` anterior no passo 1.

3. Para verificar o esquema da sua tabela, execute `!describe inputTable`.

4. Determinar o caminho de localização aos seus dados de entrada, tais como o exemplo `customers.csv` ficheiro. Os ficheiros de entrada podem ser na sua conta de armazenamento WASB/ADLS. Neste cenário de exemplo, os ficheiros de entrada estão no `<storage account parent>/inputFolderBulkLoad` diretório.

5. Mude para o diretório de execução para o comando de carregamento em massa MapReduce:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

6. Localize o `ZookeeperQuorum` valor `/etc/hbase/conf/hbase-site.xml`, com o nome da propriedade `hbase.zookeeper.quorum`.

7. Configurar o classpath e executar o `CsvBulkLoadTool` da ferramenta de comando:

    ```bash
    /usr/hdp/current/phoenix-client$ HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /inputFolderBulkLoad/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure
    ```

8. Para utilizar o MapReduce com ADLS, localize o diretório de raiz ADLS, que é o `hbase.rootdir` valor `hbase-site.xml`. O comando seguinte, é o diretório de raiz ADLS `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. Neste comando, especifique a entrada ADLS e pastas como parâmetros de saída:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

## <a name="recommendations"></a>Recomendações

* Utilize a mesmo média de armazenamento para pastas de entrada e de saída, WASB ou ADLS. Transferir dados WASB para ADLS, pode utilizar o `distcp` comando:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Utilize nós de trabalho de tamanho maior. Os processos de mapa da cópia em massa MapReduce produzem grandes quantidades de saída temporária preencher o espaço não DFS disponíveis. Para uma grande quantidade de carregamento em massa, utilize nós de trabalho mais e tamanho maior. O número de nós de trabalho que atribuir diretamente para o cluster afeta a velocidade de processamento.

* Divida os ficheiros de entrada em segmentos de ~ 10 GB. Carregamento em massa é uma operação de armazenamento intensivas, pelo que dividir os ficheiros de entrada em vários segmentos resulta numa melhor desempenho.

* Evite hotspots de servidor de região. Se a sua chave de linha forma monótona está a aumentar, escritas sequenciais HBase podem induce hotspotting do servidor de região. *Salting* a chave de linha reduz escritas sequenciais. O Phoenix fornece uma forma de forma transparente salt a chave de linha com um byte salting para uma tabela específica, tal como é referido abaixo.

## <a name="next-steps"></a>Passos Seguintes

* [Dados em massa carregar com Apache Phoenix](http://phoenix.apache.org/bulk_dataload.html)
* [Utilizar o Apache Phoenix com clusters de HBase baseado em Linux no HDInsight](../hbase/apache-hbase-phoenix-squirrel-linux.md)
* [Tabelas salted](https://phoenix.apache.org/salted.html)
* [O Phoenix gramática](http://phoenix.apache.org/language/index.html)
