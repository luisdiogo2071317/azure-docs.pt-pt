---
title: "Operacionalize um pipeline de análise de dados - Azure | Microsoft Docs"
description: "Configurar e executar um pipeline de dados de exemplo que é acionado por novos dados e produz resultados concisas."
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
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 7a439c9d25a470a2474b427f6b20addb6ff3b0c7
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Operacionalize um pipeline de análise de dados

*Pipelines de dados* underly muitas soluções de análise de dados. Como o nome sugere, um pipeline de dados aceita dados não processados, cleans reshapes-la conforme necessário e, em seguida, normalmente, efetua cálculos ou agregações antes de armazenar os dados processados. Os dados processados são consumidos por clientes, relatórios ou APIs. Um pipeline de dados tem de fornecer resultados repetíveis, se num agendamento ou quando é acionada por novos dados.

Este artigo descreve como operacionalize seus pipelines de dados para repetibilidade, utilizar o Oozie em execução em clusters do HDInsight Hadoop. O cenário de exemplo orienta-o através de um pipeline de dados que prepara e processa dados de séries de tempo companhia aérea voo.

No cenário seguinte, os dados de entrada são um ficheiro simples que contém um lote de dados de eventos durante um mês. Estes dados voo incluem informações como airport de origem e de destino, quilómetros flown, distanciamento e tempos de chegada e assim sucessivamente. O objetivo com este pipeline é resumem o desempenho de companhia aérea diária, onde cada companhia aérea que se tem uma linha para cada dia com os atrasos de partida e chegada médios, em minutos e os totais quilómetros flown nesse dia.

| ANO | MÊS | DAY_OF_MONTH | OPERADORA |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | AA | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | LD | 6.935409 | -2.1893024 | 1909696 |

O pipeline de exemplo aguarda até que os dados de eventos de um novo período de tempo chega, em seguida, armazena essas informações detalhadas de trânsito para o armazém de dados do Hive para estatísticas de longo prazo. O pipeline também cria um muito mais pequeno conjunto de dados que resume apenas os dados de voo diária. Estes dados de resumo de voo diária são enviados para uma base de dados do SQL Server para fornecer relatórios, tal como para um Web site.

O diagrama seguinte ilustra o pipeline de exemplo.

![Pipeline de dados de eventos](./media/hdinsight-operationalize-data-pipeline/pipeline-overview.png)

## <a name="oozie-solution-overview"></a>Descrição geral da solução de Oozie

Este pipeline utiliza o Apache Oozie em execução num cluster do HDInsight Hadoop.

Oozie descreve os pipelines em termos de *ações*, *fluxos de trabalho*, e *coordenadores*. Ações de determinam o trabalho real para efetuar, tal como executar uma consulta do Hive. Fluxos de trabalho definem a sequência de ações. Os coordenadores definem a agenda para quando o fluxo de trabalho é executado. Os coordenadores também podem aguardar na disponibilidade de novos dados antes de iniciar uma instância do fluxo de trabalho.

O diagrama seguinte mostra o design de alto nível de pipeline de Oozie este exemplo.

![Pipeline de dados do Oozie voo](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

### <a name="provision-azure-resources"></a>Aprovisionar os recursos do Azure

Este pipeline requer uma base de dados do SQL do Azure e um cluster de Hadoop do HDInsight na mesma localização. A base de dados do SQL do Azure armazena tanto os dados de resumos produzidos pelo pipeline e o arquivo de metadados do Oozie.

#### <a name="provision-azure-sql-database"></a>Aprovisionar SQL Database do Azure

1. No portal do Azure, criar um novo grupo de recursos com o nome `oozie` para conter todos os recursos utilizados por este exemplo.
2. Dentro do `oozie` grupo de recursos, aprovisionar um Azure SQL Server e base de dados. Não precisa de uma base de dados maior do que o escalão de preço S1 Standard.
3. No portal do Azure, navegue para o painel da base de dados do SQL recentemente implementado e selecione **ferramentas**.

    ![Botão de ferramentas](./media/hdinsight-operationalize-data-pipeline/sql-db-tools.png)

4. Selecione **Editor de consultas**.

    ![Botão de Editor de consultas](./media/hdinsight-operationalize-data-pipeline/sql-db-query-editor.png)

5. No **Editor de consultas** painel, selecione **início de sessão**.

    ![Botão de início de sessão](./media/hdinsight-operationalize-data-pipeline/sql-db-login1.png)

6. Introduza as credenciais da base de dados SQL e selecione **OK**.

   ![Formulário de início de sessão](./media/hdinsight-operationalize-data-pipeline/sql-db-login2.png)

7. Na área de texto do Editor de consultas, introduza as seguintes declarações de SQL para criar o `dailyflights` tabela que irá armazenar os dados resumidos de cada execução do pipeline.

    ```
    CREATE TABLE dailyflights
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER CHAR(2),
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    GO

    CREATE CLUSTERED INDEX dailyflights_clustered_index on dailyflights(YEAR,MONTH,DAY_OF_MONTH,CARRIER)
    GO
    ```

8. Selecione **executar** para executar as instruções SQL.

    ![Botão de execução](./media/hdinsight-operationalize-data-pipeline/sql-db-run.png)

A base de dados do SQL do Azure está agora pronto.

#### <a name="provision-an-hdinsight-hadoop-cluster"></a>Aprovisionar um Cluster de Hadoop do HDInsight

1. No portal do Azure, selecione **+ novo** e procure HDInsight.
2. Selecione **Criar**.
3. No painel de noções básicas, forneça um nome exclusivo para o cluster e escolha a sua subscrição do Azure.

    ![Nome de cluster do HDInsight e subscrição](./media/hdinsight-operationalize-data-pipeline/hdi-name-sub.png)

4. No **tipo de Cluster** painel, selecione o **Hadoop** tipo, de cluster **Linux** do sistema operativo e a versão mais recente do cluster do HDInsight. Deixe o **camada do Cluster** em **padrão**.

    ![Tipo de cluster do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-cluster-type.png)

5. Escolha **selecione** para aplicar a seleção de tipo de cluster.
6. Concluir o **Noções básicas** painel ao fornecer uma palavra-passe de início de sessão e selecionando a `oozie` recursos de grupo da lista, em seguida, selecione **seguinte**.

    ![Painel de noções básicas do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-basics.png)

7. No **armazenamento** painel, deixe o armazenamento primário tipo definido como **Storage do Azure**, selecione **criar nova**e forneça um nome para a nova conta.

    ![Definições de conta de armazenamento do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-storage.png)

8. Para o **Metastore definições**, em **selecionar uma base de dados do SQL Server para o ramo de registo**, escolha a base de dados que criou anteriormente.

    ![Definições do Metastore do Hive do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-hive.png)

9. Selecione **autenticar a base de dados SQL**.

    ![Autenticar Metastore de ramo de registo do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql.png)

10. Introduza o nome de utilizador do SQL Server da base de dados e a palavra-passe e escolha **selecione**. 

       ![Metastore do Hive do HDInsight autenticar o início de sessão](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql-login.png)

11. Reverter o **Metastore definições** painel, selecione a base de dados para os metadados do Oozie, armazenar e autenticar como fez anteriormente. 

       ![Definições do Metastore do HDInsight](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-settings.png)

12. Selecione **seguinte**.
13. No **resumo** painel, selecione **criar** para implementar o cluster.

### <a name="verify-ssh-tunneling-setup"></a>Certifique-se a configuração de túnel de SSH

Para utilizar a consola Web do Oozie para ver o estado do coordenador e instâncias de fluxo de trabalho, configure um túnel SSH ao cluster do HDInsight. Para obter mais informações, consulte [túnel SSH](hdinsight-linux-ambari-ssh-tunnel.md).

> [!NOTE]
> Também pode utilizar o Chrome com o [Foxy Proxy](https://getfoxyproxy.org/) extensão para procurar os recursos do cluster da web através do túnel SSH. Configurar a proxy de todos os pedidos através do anfitrião do `localhost` na porta do túnel 9876. Esta abordagem é compatível com o subsistema Windows para o Linux, também conhecido como Bash no Windows 10.

1. Execute o seguinte comando para abrir um túnel SSH para o cluster:

    ```
    ssh -C2qTnNf -D 9876 sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
    ```

2. Certifique-se de que o túnel está operacional, navegando até Ambari no seu nó principal ao navegar para:

    http://headnodehost:8080

3. Para o acesso a **consola da Web de Oozie** na dentro do Ambari, selecione **Oozie**, **ligações rápidas**e, em seguida, selecione **consola da Web de Oozie**.

### <a name="configure-hive"></a>Configurar o ramo de registo

1. Transferir um ficheiro CSV de exemplo que contém dados de eventos durante um mês. Transfira o ficheiro ZIP `2017-01-FlightData.zip` do [repositório do HDInsight Github](https://github.com/hdinsight/hdinsight-dev-guide) e deszipe-lo para o ficheiro CSV `2017-01-FlightData.csv`. 

2. Copie este ficheiro CSV até a conta de armazenamento do Azure ligado ao cluster do HDInsight e colocar o `/example/data/flights` pasta.

Pode copiar o ficheiro com o SCP no seu `bash` shell sessão.

1. Utilize o SCP para copiar os ficheiros do seu computador local no armazenamento local do seu nó principal do cluster de HDInsight.

    ```bash
    scp ./2017-01-FlightData.csv sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:2017-01-FlightData.csv
    ```

2. Utilize o comando HDFS ao copiar o ficheiro do seu armazenamento local do nó principal para armazenamento do Azure.

    ```bash
    hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
    ```

Os dados de exemplo estão agora disponíveis. No entanto, o pipeline necessita de duas tabelas do Hive para processar, uma para os dados de entrada (`rawFlights`) e outra para os dados resumidos (`flights`). Crie estas tabelas no Ambari da seguinte forma.

1. Inicie sessão no Ambari, navegando até [http://headnodehost:8080](http://headnodehost:8080).
2. Na lista de serviços, selecione **Hive**.

    ![A seleção do ramo de registo no Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Selecione **aceda à vista** junto a etiqueta de 2.0 de vista do Hive.

    ![A seleção de vista do Hive no Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Na área de texto de consulta, cole as seguintes instruções para criar o `rawFlights` tabela. O `rawFlights` tabela fornece uma esquema em leitura para os ficheiros CSV dentro de `/example/data/flights` pasta no armazenamento do Azure. 

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS rawflights (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    ) 
    LOCATION '/example/data/flights'
    ```

5. Selecione **executar** para criar a tabela.

    ![Consulta do Hive no Ambari](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-query.png)

6. Para criar o `flights` tabela, substitua o texto na área de texto de consulta com as seguintes declarações. O `flights` tabela é uma tabela de gerido do ramo de registo que particiona dados carregados por ano, mês e dia do mês. Esta tabela irá conter todos os dados históricos voo, com a granularidade mais baixo presente em dados de origem de uma linha por voo.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;

    CREATE TABLE flights
    (
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT
    )
    PARTITIONED BY (YEAR INT, MONTH INT, DAY_OF_MONTH INT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    );
    ```

7. Selecione **executar** para criar a tabela.

### <a name="create-the-oozie-workflow"></a>Criar o fluxo de trabalho do Oozie

Normalmente, o pipelines processarem os dados em lotes por um determinado intervalo de tempo. Neste caso, o pipeline processa os dados de voo diariamente. Esta abordagem permite para os ficheiros de entrada CSV a chegada diariamente, semanalmente, mensalmente ou anual.

O fluxo de trabalho de exemplo processa o trânsito dados-por-dia, nos três passos principais:

1. Executar uma consulta do Hive, a extrair os dados para o intervalo de datas nesse dia a partir do ficheiro CSV origem representado pelo `rawFlights` tabela e inserir dados no `flights` tabela.
2. Execute uma consulta do Hive para criar dinamicamente uma tabela de testes no ramo de registo para o dia, que contém uma cópia dos dados voo resumidos por dia e operadora.
3. Utilizar o Apache Sqoop para copiar todos os dados da tabela de teste diária no ramo de registo para o destino `dailyflights` tabela numa SQL Database do Azure. Sqoop lê as linhas da origem de dados protegido pela tabela de Hive que reside no armazenamento do Azure e carrega-os na base de dados do SQL Server a utilizar uma ligação de JDBC.

Estes três passos são coordenados por um fluxo de trabalho do Oozie. 

1. Criar uma consulta no ficheiro `hive-load-flights-partition.hql`.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;
    
    INSERT OVERWRITE TABLE flights
    PARTITION (YEAR, MONTH, DAY_OF_MONTH)
    SELECT  
        FL_DATE,
        CARRIER,
        FL_NUM,
        ORIGIN,
        DEST,
        DEP_DELAY,
        ARR_DELAY,
        ACTUAL_ELAPSED_TIME,
        DISTANCE,
        YEAR,
        MONTH,
        DAY_OF_MONTH
    FROM rawflights
    WHERE year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    As variáveis do Oozie, utilize a sintaxe `${variableName}`. Estas variáveis são definidas `job.properties` ficheiro, tal como descrito num passo subsequente. Oozie substitui os valores reais em tempo de execução.

2. Criar uma consulta no ficheiro `hive-create-daily-summary-table.hql`.

    ```
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER STRING,
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName}
    SELECT  year, month, day_of_month, carrier, avg(dep_delay) avg_dep_delay, 
            avg(arr_delay) avg_arr_delay, sum(distance) total_distance 
    FROM flights
    GROUP BY year, month, day_of_month, carrier 
    HAVING year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Esta consulta cria uma tabela de testes que irá armazenar apenas os dados resumidos durante um dia, tome nota da instrução SELECT que calcula a média atrasos e total de distância flown pela operadora por dia. Os dados inseridos nesta tabela armazenada numa localização conhecida (o caminho indicado pela variável hiveDataFolder) para que pode ser utilizado como origem para Sqoop no próximo passo.

3. Execute o seguinte comando do Sqoop.

    ```
    sqoop export --connect ${sqlDatabaseConnectionString} --table ${sqlDatabaseTableName} --export-dir ${hiveDataFolder} -m 1 --input-fields-terminated-by "\t"
    ```

Estes três passos são expressos em três ações separadas no seguinte ficheiro de fluxo de trabalho do Oozie, com o nome `workflow.xml`.

```
<workflow-app name="loadflightstable" xmlns="uri:oozie:workflow:0.5">
    <start to = "RunHiveLoadFlightsScript"/>
    <action name="RunHiveLoadFlightsScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptLoadPartition}</script>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
        </hive>
        <ok to="RunHiveCreateDailyFlightTableScript"/>
        <error to="fail"/>
    </action>

    <action name="RunHiveCreateDailyFlightTableScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptCreateDailyTable}</script>
            <param>hiveTableName=${hiveDailyTableName}</param>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
            <param>hiveDataFolder=${hiveDataFolder}/${year}/${month}/${day}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
    </action>

    <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}/${year}/${month}/${day}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
    </action>
    <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
    </kill>
    <end name="end"/>
</workflow-app>
```

As consultas do Hive duas são acedidas pelo respetivo caminho no armazenamento do Azure e os restantes valores de variáveis são fornecidos pelos seguintes `job.properties` ficheiro. Este ficheiro configura o fluxo de trabalho para ser executada para a data 3rd de Janeiro de 2017.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.wf.application.path=${appBase}/load_flights_by_day
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableName=dailyflights${year}${month}${day}
hiveDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/${year}/${month}/${day}
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights
year=2017
month=01
day=03
```

A tabela seguinte resume a cada uma das propriedades e indica onde pode encontrar os valores para o seu ambiente.

| Propriedade | Origem de valor |
| --- | --- |
| nameNode | O caminho completo para o contentor de armazenamento do Azure ligado ao cluster do HDInsight. |
| jobTracker | O nome de anfitrião interno para o cluster ativo YARN head nó. Na home page do Ambari, selecione o YARN da lista de serviços, em seguida, escolha o Gestor de recursos do Active Directory. O nome de anfitrião URI é apresentada na parte superior da página. Acrescente a porta 8050. |
| queueName | O nome da fila utilizado durante o agendamento as ações de ramo de registo YARN. Deixe como predefinição. |
| oozie.use.system.libpath | Deixe como verdadeiro. |
| appBase | O caminho para a subpasta no armazenamento do Azure para implementar o fluxo de trabalho do Oozie e ficheiros de suporte. |
| oozie.wf.application.path | A localização do fluxo de trabalho Oozie `workflow.xml` para ser executada. |
| hiveScriptLoadPartition | O caminho no armazenamento do Azure para o ficheiro de consulta do Hive `hive-load-flights-partition.hql`. |
| hiveScriptCreateDailyTable | O caminho no armazenamento do Azure para o ficheiro de consulta do Hive `hive-create-daily-summary-table.hql`. |
| hiveDailyTableName | O nome gerado dinamicamente a utilizar para a tabela de testes. |
| hiveDataFolder | O caminho no armazenamento do Azure para os dados contidos por tabela de teste. |
| sqlDatabaseConnectionString | JDBC sintaxe cadeia de ligação à base de dados SQL do Azure. |
| sqlDatabaseTableName | O nome da tabela na base de dados do SQL do Azure na qual o resumidas linhas estão inseridas. Deixe como `dailyflights`. |
| ano | O componente de ano do dia para o trânsito resumos são calculados. Deixe como está. |
| mês | O componente de mês do dia para o trânsito resumos são calculados. Deixe como está. |
| dia | O dia do componente de mês do dia para o trânsito resumos são calculados. Deixe como está. |

> [!NOTE]
> Certifique-se ao atualizar a sua cópia o `job.properties` ficheiro com valores específicos ao seu ambiente, antes de poder implementar e executar o fluxo de trabalho do Oozie.

### <a name="deploy-and-run-the-oozie-workflow"></a>Implementar e executar o fluxo de trabalho do Oozie

Utilizar o SCP da sessão bash para implementar o seu fluxo de trabalho do Oozie (`workflow.xml`), as consultas do Hive (`hive-load-flights-partition.hql` e `hive-create-daily-summary-table.hql`) e a configuração da tarefa (`job.properties`).  No Oozie, apenas o `job.properties` ficheiro pode existir no armazenamento local do headnode. Todos os outros ficheiros têm de ser armazenados no HDFS, este cenário do armazenamento do Azure. A ação de Sqoop utilizada pelo fluxo de trabalho depende de um controlador JDBC para comunicação com a base de dados do SQL Server, que têm de ser copiados do nó principal para o HDFS.

1. Criar o `load_flights_by_day` subpasta por baixo do caminho do utilizador no armazenamento local do nó principal.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 'mkdir load_flights_by_day'

2. Copiar todos os ficheiros no diretório atual (o `workflow.xml` e `job.properties` ficheiros) até o `load_flights_by_day` subpasta.

        scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:load_flights_by_day

3. SSH para o nó principal e navegue para o `load_flights_by_day` pasta.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
        cd load_flights_by_day

4. Copie os ficheiros de fluxo de trabalho para HDFS.

        hdfs dfs -put ./* /oozie/load_flights_by_day

5. Cópia `sqljdbc41.jar` do nó principal do local para a pasta de fluxo de trabalho no HDFS:

        hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /oozie/load_flights_by_day

6. Execute o fluxo de trabalho.

        oozie job -config job.properties -run

7. Observe o estado utilizando a consola Web do Oozie. A partir da dentro do Ambari, selecione **Oozie**, **ligações rápidas**e, em seguida, **consola da Web de Oozie**. Sob o **tarefas de fluxo de trabalho** separador, selecione **todas as tarefas**.

    ![Fluxos de trabalho do Oozie Web consola](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

8. Quando o estado é concluída com êxito, consultar a tabela de base de dados SQL para ver as linhas inseridas. No portal do Azure, navegue para o painel da base de dados do SQL Server, selecione **ferramentas**e abra o **Editor de consultas**.

        SELECT * FROM dailyflights

Agora que o fluxo de trabalho está em execução para o dia de teste único, pode encapsular este fluxo de trabalho com um coordenador que agenda o fluxo de trabalho para que seja executada diariamente.

### <a name="run-the-workflow-with-a-coordinator"></a>Executar o fluxo de trabalho com um coordenador

Para agendar este fluxo de trabalho para que seja executado diariamente (ou todos os dias, um intervalo de data), pode utilizar um coordenador. Um coordenador é definido por um ficheiro XML, por exemplo `coordinator.xml`:

```
<coordinator-app name="daily_export" start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" timezone="UTC" xmlns="uri:oozie:coordinator:0.4">
    <datasets>
        <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
            <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
            <done-flag></done-flag>
        </dataset>
    </datasets>
    <input-events>
        <data-in name="event_input1" dataset="ds_input1">
            <instance>${coord:current(0)}</instance>
        </data-in>
    </input-events>
    <action>
        <workflow>
            <app-path>${appBase}/load_flights_by_day</app-path>
            <configuration>
                <property>
                    <name>year</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'yyyy')}</value>
                </property>
                <property>
                    <name>month</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'MM')}</value>
                </property>
                <property>
                    <name>day</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveScriptLoadPartition</name>
                    <value>${hiveScriptLoadPartition}</value>
                </property>
                <property>
                    <name>hiveScriptCreateDailyTable</name>
                    <value>${hiveScriptCreateDailyTable}</value>
                </property>
                <property>
                    <name>hiveDailyTableNamePrefix</name>
                    <value>${hiveDailyTableNamePrefix}</value>
                </property>
                <property>
                    <name>hiveDailyTableName</name>
                    <value>${hiveDailyTableNamePrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}${coord:formatTime(coord:nominalTime(), 'MM')}${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveDataFolderPrefix</name>
                    <value>${hiveDataFolderPrefix}</value>
                </property>
                <property>
                    <name>hiveDataFolder</name>
                    <value>${hiveDataFolderPrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}/${coord:formatTime(coord:nominalTime(), 'MM')}/${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>sqlDatabaseConnectionString</name>
                    <value>${sqlDatabaseConnectionString}</value>
                </property>
                <property>
                    <name>sqlDatabaseTableName</name>
                    <value>${sqlDatabaseTableName}</value>
                </property>
            </configuration>
        </workflow>
    </action>
</coordinator-app>
```

Como pode ver, a maioria do coordenador é apenas a transmissão de informações de configuração para a instância de fluxo de trabalho. No entanto, existem alguns itens importantes para chamar.

* Ponto de 1: O `start` e `end` atributos no `coordinator-app` o próprio elemento controlar o intervalo de tempo durante o qual é executado o coordenador.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Um coordenador é responsável por ações dentro de agendamento de `start` e `end` data intervalo, de acordo com o intervalo especificado pelo `frequency` atributo. Cada ação agendada é por sua vez executado o fluxo de trabalho como configurado. Na definição do coordenador acima, o coordenador está configurado para executar ações de 1 de Janeiro de 2017 5th de Janeiro de 2017. A frequência é definida para 1 dia pelo [Oozie expressão de linguagem](http://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) expressão frequência `${coord:days(1)}`. Como resultado, o coordenador de agendamento de uma ação (e, por conseguinte, o fluxo de trabalho) uma vez por dia. Para intervalos de data que estão no passado, tal como neste exemplo, a ação será possível agendar a execução sem atrasos. O início da data a partir do qual uma ação está agendada para execução é denominado o *tempo nominal*. Por exemplo, para processar os dados para 1 de Janeiro de 2017 o coordenador de ação irá agendar ação com um tempo nominal de 2017-01-01T00:00:00 GMT.

* Ponto de 2: dentro do intervalo de data do fluxo de trabalho, o `dataset` elemento Especifica onde no HDFS, procure os dados de um determinado período de tempo e configura a forma como o Oozie determina se os dados estão disponíveis ainda para processamento.

    ```
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    O caminho para os dados no HDFS baseia-se dinamicamente, de acordo com a expressão fornecida no `uri-template` elemento. Este coordenador, uma frequência de um dia é também utilizada com o conjunto de dados. Enquanto as datas de início e de fim no controlo de elemento coordenador quando as ações são agendadas (e define as horas das suas nominal), o `initial-instance` e `frequency` no conjunto de dados, controlar o cálculo da data em que é utilizada na construir o `uri-template`. Neste caso, defina inicial de instâncias para um dia antes do início do coordenador para se certificar de que escolherá o primeiro dia da visão (1/1/2017) de dados. Cálculo de data o conjunto de dados avança do valor `initial-instance` (31/12/2016) avançadas em incrementos de frequência do conjunto de dados (1 dia) até encontrar a data mais recente que não transmite a hora nominal definido pelo coordenador (2017-01-01T00:00:00 em GMT para a primeira ação).

    Vazia `done-flag` elemento indica que quando Oozie verifica a presença de dados de entrada no momento appointed, Oozie determina dados se disponíveis ao presença de um ficheiro ou diretório. Neste caso, é a presença de um ficheiro csv. Se existir um ficheiro csv, Oozie assume que os dados são prontos e inicia uma instância de fluxo de trabalho para processar o ficheiro. Se não houver nenhum ficheiro csv presente, Oozie assume os dados ainda não foram pronta e essa execução do fluxo de trabalho entra no estado de espera.

* Ponto de 3: O `data-in` elemento Especifica o timestamp específico para utilizar como o nominal tempo ao substituir os valores existentes na `uri-template` para o conjunto de dados associado.

    ```
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    Neste caso, defina a instância para a expressão `${coord:current(0)}`, que traduz-se utilizando a hora nominal da ação como originalmente agendada pelo coordenador. Por outras palavras, quando o coordenador de agenda a ação de execução com um tempo nominal de 01/01/2017, em seguida, 01/01/2017 é o que é utilizado para substituir o ano (2017) e variáveis de mês (01) no modelo de URI. Assim que o modelo URI é calculado para esta instância, Oozie verifica se o ficheiro ou diretório esperado está disponível e a agenda a próxima execução do fluxo de trabalho em conformidade.

Combinam os três pontos anteriores para produzir uma situação em que o coordenador de agendas de processamento dos dados de origem, de forma por dia. 

* Ponto de 1: O coordenador começa com uma data nominal de 2017-01-01.

* Ponto de 2: Oozie procura dados disponíveis em `sourceDataFolder/2017-01-FlightData.csv`.

* Ponto de 3: Quando Oozie localiza esse ficheiro, mesmo as agendas de uma instância de fluxo de trabalho que irá processar os dados de 2017-01-01. Oozie, em seguida, continua a processar para 2017-01-02. Esta avaliação repete até mas não incluindo 2017-01-05.

Tal como com fluxos de trabalho, a configuração de um coordenador está definida num `job.properties` ficheiro, que tem um superconjunto das definições utilizadas pelo fluxo de trabalho.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.coord.application.path=${appBase}
sourceDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableNamePrefix=dailyflights
hiveDataFolderPrefix=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights

```

As propriedades de novas apenas introduzidas nesta `job.properties` ficheiro são:

| Propriedade | Origem de valor |
| --- | --- |
| oozie.coord.application.path | Indica a localização do `coordinator.xml` ficheiro que contém o coordenador de Oozie para ser executada. |
| hiveDailyTableNamePrefix | O prefixo utilizado ao criar dinamicamente o nome da tabela da tabela de teste. |
| hiveDataFolderPrefix | O prefixo do caminho armazenar todas as tabelas de testes. |

### <a name="deploy-and-run-the-oozie-coordinator"></a>Implementar e executar o coordenador de Oozie

Para executar o pipeline com um coordenador, continuar de forma semelhante para o fluxo de trabalho, exceto que trabalhem a partir de uma pasta de um nível acima a pasta que contém o fluxo de trabalho. Esta Convenção de pasta separa os coordenadores dos fluxos de trabalho no disco, pelo que pode associar um coordenador com fluxos de trabalho subordinado diferentes.

1. Utilize SCP do seu computador local para copiar os ficheiros de coordenador até o armazenamento local do nó principal do cluster.

    ```bash
    scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:~
    ```

2. SSH para o nó principal.

    ```bash
    ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 
    ```

3. Copie os ficheiros de coordenador para HDFS.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. Execute o coordenador.

    ```bash
    oozie job -config job.properties -run
    ```

5. Verifique o estado utilizando a consola Web do Oozie, este tempo, selecionando o **coordenador tarefas** separador e, em seguida, **todas as tarefas**.

    ![Tarefas do coordenador de consola Web do Oozie](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Selecione uma instância de coordenador para apresentar a lista de ações agendadas. Neste caso, deverá ver quatro ações com as horas nominal no intervalo de 1/1/2017 para 1/4/2017.

    ![Tarefa de coordenador de consola Web do Oozie](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Cada ação nesta lista corresponde a uma instância do fluxo de trabalho que processa dias um de dados, onde o início nesse dia é indicado nominal dentro do tempo.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação do Apache Oozie](http://oozie.apache.org/docs/4.2.0/index.html)

<!-- * Build the same pipeline [using Azure Data Factory](tbd.md).  -->
