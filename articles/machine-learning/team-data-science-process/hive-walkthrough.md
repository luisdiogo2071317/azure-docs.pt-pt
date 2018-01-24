---
title: Explore os dados num cluster de Hadoop e criar modelos no Azure Machine Learning | Microsoft Docs
description: "Para um cenário ponto-a-ponto, a utilização de um cluster de Hadoop do HDInsight para criar e implementar um modelo a utilizar o processo de ciência de dados de equipa."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev
ms.openlocfilehash: daf5168ba9a21a56d72fc14649c349b0fb63a167
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>O processo de ciência de dados de equipa em ação: utilização do Azure HDInsight Hadoop clusters
Esta explicação passo a passo, utilizamos o [processo de ciência de dados de equipa (TDSP)](overview.md) num cenário de ponto a ponto. Utilizamos uma [cluster Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) para armazenar, explore e os dados de funcionalidade engenheiro de publicamente disponível [NYC Taxi viagens](http://www.andresmh.com/nyctaxitrips/) conjunto de dados e para os dados de exemplo para baixo. Para processar várias classes e binária classificação e tarefas preditiva de regressão, iremos criar modelos de dados com o Azure Machine Learning. 

Para instruções que mostra como lidar com um conjunto de dados maior, consulte [processo de ciência de dados de equipa - utilizando o Azure HDInsight Clusters do Hadoop um conjunto de dados de 1 TB](hive-criteo-walkthrough.md).

Também pode utilizar um bloco de notas IPython para realizar as tarefas apresentadas as instruções que utiliza o conjunto de dados de 1 TB. Para obter mais informações, consulte [instruções Criteo utilizando uma ligação de ODBC do Hive](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="dataset"></a>Descrição do conjunto de dados de NYC Taxi viagens
Os dados de NYC Taxi viagem são cerca de 20 GB de ficheiros comprimidos valores separados por vírgulas (CSV) (GB de ~ 48 descomprimido). Tem mais de 173 milhões de viagens individuais e inclui fares pagas para cada viagem. Cada registo viagem inclui a localização pick-up e drop-off e tempo, acesso anónimos (controlador) número de licença e número de medallion (o taxi um ID exclusivo). Os dados abrange todos os viagens no ano 2013 e são fornecidos nos seguintes dois conjuntos de dados de cada mês:

- Os ficheiros CSV trip_data contém detalhes de viagem. Isto inclui o número de passageiros, pick-up e pontos de drop-off, duração de viagem e comprimento viagem. Seguem-se alguns registos de exemplo:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- Os ficheiros CSV trip_fare contém detalhes de fare paga para cada viagem. Isto inclui o tipo de pagamento, a quantidade de fare, surcharge e, sugestões e impostos tolls e a quantidade total paga. Seguem-se alguns registos de exemplo:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

A chave exclusiva para associar a viagem\_dados e viagem\_fare é composto por campos: medallion, acesso\_licença e recolha\_datetime. Para obter todos os detalhes relevantes para uma determinado viagem, é suficiente associar estas três chaves.

## <a name="mltasks"></a>Exemplos de tarefas de predição
Determine o tipo de predições que pretende efetuar com base na análise de dados. Isto ajuda a clarificar as tarefas que têm de incluir o processo. Seguem-se exemplos de três dos problemas de predição que iremos abordar esta explicação passo a passo. Estas baseiam o *sugestão\_quantidade*:

- **Classificação binária**: prever ou não uma sugestão foi paga para uma viagem. Ou seja, um *sugestão\_quantidade* que é superior ao $0 é um exemplo positivo, enquanto um *sugestão\_quantidade* $ 0 é um exemplo negativo.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Classificação de várias classes**: prever o intervalo de quantidades de sugestão paga para a viagem. Iremos dividir os *sugestão\_quantidade* em cinco classes:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Tarefa de regressão**: prever a quantidade de sugestão paga para uma viagem.  

## <a name="setup"></a>Configurar um cluster do HDInsight Hadoop para análise avançada
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

Pode configurar um ambiente do Azure para análise avançada que utiliza um cluster do HDInsight em três passos:

1. [Criar uma conta de armazenamento](../../storage/common/storage-create-storage-account.md): esta conta de armazenamento é utilizada para armazenar dados no Blob storage do Azure. Os dados utilizados nos clusters do HDInsight também residem aqui.
2. [Personalizar clusters do Hadoop do Azure HDInsight para o processo de análise avançada e tecnologia](customize-hadoop-cluster.md). Este passo cria um cluster de Hadoop do HDInsight com 64-bit Anaconda Python 2.7 instalado em todos os nós. Existem dois passos importantes lembrar-se ao personalizar o cluster do HDInsight.
   
   * Lembre-se associar a conta de armazenamento criada no passo 1 com o cluster do HDInsight quando estiver a criar. Esta conta de armazenamento acede aos dados que são processados no cluster.
   * Depois de criar o cluster, ative o acesso remoto para o nó principal do cluster. Navegue para o **configuração** separador e selecione **ativar remoto**. Este passo Especifica as credenciais de utilizador utilizadas para início de sessão remoto.
3. [Criar uma área de trabalho do Azure Machine Learning](../studio/create-workspace.md): Utilize esta área de trabalho para criar modelos de machine learning. Esta tarefa é abordada depois de concluir uma exploração de dados inicial e para baixo-amostragem, utilizando o cluster do HDInsight.

## <a name="getdata"></a>Obter os dados a partir de uma origem pública
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

Para copiar o [NYC Taxi viagens](http://www.andresmh.com/nyctaxitrips/) conjunto de dados ao seu computador da localização pública, utilize qualquer um dos métodos descritos em [mover dados para e do Blob storage do Azure](move-azure-blob.md).

Aqui, iremos descrevem como utilizar o AzCopy para transferir os ficheiros que contêm dados. Para transferir e instalar o AzCopy, siga as instruções apresentadas em [introdução ao utilitário de linha de comandos AzCopy](../../storage/common/storage-use-azcopy.md).

1. A partir de uma janela da linha de comandos, execute os seguintes comandos do AzCopy, substituindo *< path_to_data_folder >* com o destino pretendido:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Quando a cópia estiver concluída, verá um total de 24 ficheiros zipped na pasta de dados escolhido. Descomprimir os ficheiros transferidos para o mesmo diretório no seu computador local. Tome nota da pasta onde residem os ficheiros descomprimidos. Esta pasta é referida como o *< caminho\_para\_unzipped_data\_ficheiros\>*  em que modo.

## <a name="upload"></a>Carregar os dados para o contentor predefinido do cluster do HDInsight Hadoop
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

Nos seguintes comandos do AzCopy, substitua os seguintes parâmetros com os valores reais que especificou ao criar o cluster do Hadoop e unzipping os ficheiros de dados.

* ***&#60; path_to_data_folder >*** o diretório (juntamente com o caminho) no seu computador que contém os ficheiros de dados deszipada.  
* ***&#60; nome da conta de armazenamento de cluster do Hadoop >*** a conta de armazenamento associada cluster do HDInsight.
* ***&#60; contentor predefinido do cluster de Hadoop >*** contentor predefinido utilizado pelo cluster. Tenha em atenção que o nome do contentor predefinido é, normalmente, o mesmo nome de cluster em si. Por exemplo, se o cluster é chamado "abc123.azurehdinsight.net", o contentor predefinido é abc123.
* ***&#60; a chave de conta de armazenamento >*** a chave para a conta de armazenamento utilizada pelo cluster.

A partir de uma linha de comandos ou janela do Windows PowerShell, execute os seguintes comandos do AzCopy dois.

Este comando carrega os dados de viagem para o ***nyctaxitripraw*** diretório no contentor predefinido do cluster de Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Este comando carrega os dados de fare para o ***nyctaxifareraw*** diretório no contentor predefinido do cluster de Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Os dados agora devem estar no armazenamento de BLOBs e pronto a ser consumidos dentro do cluster do HDInsight.

## <a name="#download-hql-files"></a>Inicie sessão no nó principal do cluster do Hadoop e preparar para análises exploratórias dados
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

Para aceder ao nó principal do cluster para análise de dados exploratórias e baixo-amostragem dos dados, siga o procedimento descrito no [aceder ao nó principal do Cluster de Hadoop](customize-hadoop-cluster.md).

Esta explicação passo a passo, utilizamos principalmente consultas escritas em [Hive](https://hive.apache.org/), uma linguagem de consulta como o SQL Server, para efetuar explorations dados preliminar. As consultas do Hive são armazenadas em ficheiros .hql. Vamos, em seguida, baixo-exemplo estes dados a utilizar no Machine Learning para criar modelos.

Para preparar o cluster para análises exploratórias dados, transferir os ficheiros de .hql que contém os scripts de ramo de registo relevantes do [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) para um diretório local (C:\temp) no nó principal. Para tal, abra de linha de comandos a partir de nó principal do cluster e, execute os seguintes dois comandos:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Estes dois comandos transferir todos os ficheiros de .hql necessários esta explicação passo a passo para o diretório local ***C:\temp &#92;*** no nó principal.

## <a name="#hive-db-tables"></a>Criar base de dados do Hive e tabelas particionadas por mês
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

Agora está pronto para criar as tabelas do Hive do conjunto de dados de taxi NYC.
No nó principal do cluster de Hadoop, abra a linha de comandos do Hadoop no ambiente de trabalho do nó principal. Introduza o diretório de ramo de registo, executando o seguinte comando:

    cd %hive_home%\bin

> [!NOTE]
> Executar todos os comandos de ramo de registo esta explicação passo a passo da Reciclagem do ramo de registo / linha de comandos do diretório. Isto processa automaticamente quaisquer problemas de caminho. Utilizamos os termos "Linha de diretório de ramo de registo", "Hive bin / linha de comandos do diretório" e "Hadoop linha de comandos"-no alternadamente nestas instruções.
> 
> 

A partir da linha de comandos do diretório da ramo de registo, execute o seguinte comando na linha de comandos de Hadoop do nó principal do. Isto submete a consulta do Hive para criar a base de dados do Hive e tabelas:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Eis o conteúdo a **C:\temp\sample\_hive\_criar\_db\_e\_tables.hql** ficheiro. Esta ação cria a base de dados do Hive **nyctaxidb**e as tabelas **viagem** e **fare**.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Este script de ramo de registo cria duas tabelas:

* O **viagem** tabela contém detalhes viagem de cada substituir (detalhes do controlador, pick-up a hora, distância viagem e vezes).
* O **fare** tabela contém detalhes fare (quantidade fare, a quantidade de sugestão, tolls e surcharges).

Se necessitar de qualquer assistência adicional com estes procedimentos, ou que pretende investigar aqueles alternativos, consulte a secção [consultas submeter Hive diretamente a partir da linha de comandos do Hadoop](move-hive-tables.md#submit).

## <a name="#load-data"></a>Carregar dados para as tabelas do Hive, partições
> [!NOTE]
> Normalmente, trata-se uma tarefa de admin.
> 
> 

O conjunto de dados de taxi NYC tem uma criação de partições natural por mês, o que utilizamos para ativar a tempos mais rápidos de processamento e a consulta. Os seguintes comandos do PowerShell (emitidos a partir do diretório de ramo de registo utilizando a linha de comandos do Hadoop) carregar dados para a viagem e fare as tabelas do Hive, particionadas por mês.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

O **exemplo\_hive\_carregar\_dados\_por\_partitions.hql** ficheiro contém o seguinte **carregar** comandos:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Tenha em atenção que um número de consultas do Hive aqui utilizados no processo de exploração envolve observar apenas uma ou duas partições. Mas estas consultas podem ser executadas em todo o conjunto de dados.

### <a name="#show-db"></a>Mostrar as bases de dados do cluster do HDInsight Hadoop
Para mostrar as bases de dados criados no cluster do HDInsight Hadoop dentro da janela de linha de comandos do Hadoop, execute o seguinte comando na linha de comandos de Hadoop:

    hive -e "show databases;"

### <a name="#show-tables"></a>Mostrar as tabelas do Hive a **nyctaxidb** base de dados
Para mostrar as tabelas a **nyctaxidb** base de dados, execute o seguinte comando na linha de comandos de Hadoop:

    hive -e "show tables in nyctaxidb;"

Iremos pode confirmar que as tabelas são particionadas, executando o seguinte comando:

    hive -e "show partitions nyctaxidb.trip;"

O resultado esperado é:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Da mesma forma, podemos garantir que o se a tabela de fare está particionada executando o seguinte comando:

    hive -e "show partitions nyctaxidb.fare;"

O resultado esperado é:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Exploração de dados e de engenharia da funcionalidade no ramo de registo
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Pode utilizar consultas do Hive para realizar a exploração de dados e a funcionalidade de tarefas para os dados carregados para as tabelas do Hive de engenharia. Seguem-se exemplos de tais tarefas:

* Ver os registos de 10 principais em ambas as tabelas.
* Explore as distribuições de dados de alguns campos no variando intervalos de tempo.
* Investigue a qualidade dos dados dos campos de latitude e longitude.
* Gere etiquetas de classificação de várias classes e binária com base na quantidade de sugestão.
* Gere funcionalidades pela computação as distâncias viagem direta.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Exploração: Ver os registos de 10 principais em viagem de tabela
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Para ver o aspeto de dados, examine 10 registos de cada tabela. Para inspecionar os registos, execute as seguintes consultas de dois separadamente a partir da linha de diretório de ramo de registo na consola da linha de comandos do Hadoop.

Para obter os registos de 10 principais na tabela viagem de primeiro mês:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Para obter os registos de 10 principais na tabela fare do primeiro mês:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Pode guardar os registos para um ficheiro para uma visualização conveniente. Uma alteração de pequenas a consulta precedente executa esta operação:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Exploração: Ver o número de registos em cada uma das 12 partições
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

De interesse é a forma como o número de viagens varia durante o ano de calendário. Agrupamento por mês mostra a distribuição viagens.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Isto dá-na seguinte saída:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Aqui, a primeira coluna é mês e o segundo é o número de viagens para esse mês.

Também iremos pode contar o número total de registos no nosso conjunto de dados viagem executando o seguinte comando na linha de diretório de ramo de registo:

    hive -e "select count(*) from nyctaxidb.trip;"

Isto gera:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Utilizar comandos semelhantes às apresentado para o conjunto de dados viagem, iremos podem emitir consultas do Hive de linha de diretório de ramo de registo para o conjunto de dados fare validar o número de registos.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Isto dá-na seguinte saída:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Tenha em atenção que o mesmo número exato de viagens por mês é devolvido para ambos os conjuntos de dados. Isto fornece a validação primeiro que os dados foram carregados corretamente.

Pode contagem do número total de registos no conjunto de dados fare utilizando o seguinte comando a partir da linha de diretório de ramo de registo:

    hive -e "select count(*) from nyctaxidb.fare;"

Isto gera:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

O número total de registos em ambas as tabelas também é o mesmo. Esta opção fornece uma segundo validação que os dados foram carregados corretamente.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploração: Distribuição de viagem por medallion
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Neste exemplo identifica os medallions (taxi números) com maior que 100 viagens dentro de um determinado período de tempo. A consulta é vantajosa contra o acesso de tabela particionada necessitar, porque este está a ter pela variável de partição **mês**. Os resultados da consulta são escritos num ficheiro local, **queryoutput.tsv**, na `C:\temp` no nó principal.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Eis o conteúdo do **exemplo\_hive\_viagem\_contagem\_por\_medallion.hql** ficheiro para inspecção.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medallion no conjunto de dados de taxi NYC identifica um ficheiro cab exclusivo. Pode identificar os ficheiros CAB é intuitivas ocupado utilizando aqueles efetuadas mais do que um determinado número de viagens num período de tempo específico. O exemplo seguinte identifica os ficheiros. cab que efetuou a mais do que uma centena viagens na primeira durante três meses e guarda a consulta resulta num ficheiro local, **C:\temp\queryoutput.tsv**.

Eis o conteúdo do **exemplo\_hive\_viagem\_contagem\_por\_medallion.hql** ficheiro para inspecção.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

A partir da linha de comandos do diretório da ramo de registo, execute o seguinte comando:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Exploração: Viagem distribuição através da licença medallion e acesso
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Quando a explorar a um conjunto de dados, queremos frequentemente examinar o número de co-occurences dos grupos de valores. Esta secção fornece um exemplo de como fazê-lo para controladores e ficheiros CAB.

O **exemplo\_hive\_viagem\_contagem\_por\_medallion\_license.hql** fare conjunto de dados de grupos de ficheiros no **medallion** e **hack_license**e devolve o número de cada combinação. Seguem-se o seu conteúdo:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Esta consulta devolve combinações de controlador e o ficheiro cab, ordenadas pelo descendente diversas viagens.

A partir da linha de comandos do diretório da ramo de registo, execute:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Os resultados da consulta são escritos num ficheiro local, **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Exploração: Avaliar a qualidade dos dados verificando longitude inválido ou registos de latitude
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Um objetivo comuns de análise de dados exploratórias é weed saída registos inválidos ou danificados. O exemplo nesta secção determina se os campos de longitude ou latitude contém um valor até que ponto fora da área de NYC. Uma vez que é provável que esses registos tem um valor de latitude de longitude errada, queremos eliminá-los a partir de quaisquer dados que está a ser utilizado para a modelação.

Eis o conteúdo de **exemplo\_hive\_qualidade\_assessment.hql** ficheiro para inspecção.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


A partir da linha de comandos do diretório da ramo de registo, execute:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

O *-S* argumento incluído neste comando suprime printout de ecrã do estado das tarefas de mapa/reduza ramo de registo. Isto é útil porque faz o ecrã de impressão do resultado de consulta do Hive mais legível.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Exploração: Distribuições de classe binário das sugestões viagem
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Para o problema de classificação binária descrito no [exemplos das tarefas de predição](hive-walkthrough.md#mltasks) secção, é útil para saber se uma sugestão foi fornecida ou não. Esta distribuição das sugestões é binária:

* Sugestão fornecido (1 de classe, sugestão\_culmina > $0)  
* nenhuma sugestão (classe 0, sugestão\_quantidade = $0)

O seguinte **exemplo\_hive\_tipped\_frequencies.hql** ficheiro efetua este procedimento:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

A partir da linha de comandos do diretório da ramo de registo, execute:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Exploração: Distribuições na definição de várias classes de classe
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Para o problema de classificação de várias classes descrito no [exemplos das tarefas de predição](hive-walkthrough.md#mltasks) secção, este conjunto de dados também presta-se a uma classificação natural para prever a quantidade de sugestões fornecido. Podemos utilizar intervalos binários para definir os intervalos de sugestão na consulta. Para obter as distribuições de classe para a vários intervalos de sugestão, utilize o **exemplo\_hive\_sugestão\_intervalo\_frequencies.hql** ficheiro. Seguem-se o respetivo conteúdo.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Execute o seguinte comando a partir da consola da linha de comandos do Hadoop:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Exploração: A distância direta entre duas localizações de longitude latitude de computação
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Pode querer saber se há uma diferença entre a distância entre duas localizações e a distância viagem real do taxi direta. Um passenger poderá ser menos provável é que a sugestão se estes descobrir que o controlador tem intencionalmente colocada-los por uma rota de tempo.

Para ver a comparação entre distância viagem real e o [Haversine distância](http://en.wikipedia.org/wiki/Haversine_formula) entre dois pontos de longitude latitude (a distância "ótimo círculo"), pode utilizar as funções de trigonometric disponíveis dentro do ramo de registo:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

A consulta anterior, R é radius terrestre em quilómetros e pi é convertida em radianos. Tenha em atenção que os pontos de longitude latitude são filtrados para remover os valores que são longe da área de NYC.

Neste caso, iremos escrever os resultados para um diretório denominado **queryoutputdir**. A sequência dos seguintes comandos cria pela primeira vez este diretório de saída e, em seguida, executa o comando de ramo de registo.

A partir da linha de comandos do diretório da ramo de registo, execute:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Os resultados da consulta são escritos nove blobs do Azure (**queryoutputdir/000000\_0** para **queryoutputdir/000008\_0**), sob o contentor predefinido do cluster de Hadoop.

Para ver o tamanho dos blobs individuais, execute o seguinte comando a partir da linha de diretório de ramo de registo:

    hdfs dfs -ls wasb:///queryoutputdir

Para ver os conteúdos de um determinado ficheiro, diga **000000\_0**, utilize do Hadoop `copyToLocal` comando.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`pode ser muito lento para ficheiros grandes e não é recomendado para utilização com as mesmas.  
> 
> 

Uma vantagem de ter estes dados residem num blob do Azure é que podemos pode explorar os dados dentro do Machine Learning, utilizando o [importar dados] [ import-data] módulo.

## <a name="#downsample"></a>Dados de exemplo para baixo e criar modelos de Machine Learning
> [!NOTE]
> Normalmente, trata-se uma tarefa de scientist de dados.
> 
> 

Após a fase de análise de dados exploratórias, iremos agora está prontos para baixo-sample os dados para a criação de modelos de Machine Learning. Nesta secção, mostramos como utilizar uma consulta do Hive para os dados de exemplo para baixo. Machine Learning, em seguida, acede à [importar dados] [ import-data] módulo.

### <a name="down-sampling-the-data"></a>Os dados de amostragem para baixo
Existem dois passos neste procedimento. Primeiro vamos associar o **nyctaxidb.trip** e **nyctaxidb.fare** tabelas em três chaves que estão presentes em todos os registos: **medallion**, **hack\_ licença**, e **recolha\_datetime**. Em seguida, geramos uma etiqueta de classificação binária, **tipped**e uma etiqueta de classificação de várias classes, **sugestão\_classe**.

Para poder utilizar os dados de objeto de amostragem de baixo diretamente a partir de [importar dados] [ import-data] módulo no Machine Learning, deve armazenar os resultados da consulta anterior a uma tabela do Hive interna. No que se segue, iremos criar uma tabela do Hive interna e preencher o respetivo conteúdo com os dados associados e objeto de amostragem de baixo.

A consulta aplica-se o padrão do Hive funções diretamente para gerar o seguinte a partir de **recolha\_datetime** campo:
- Hora do dia
- Semana do ano
- dia da semana (1 consegue para segunda-feira e 7 consegue para Domingo)

A consulta também gera a distância entre as localizações pick-up e drop-off direta. Para obter uma lista completa dessas funções, consulte [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

A consulta, em seguida, baixo-samples os dados para que os resultados da consulta podem ajustar à Azure Machine Learning Studio. Apenas 1 por cento do conjunto de dados original é importado para o studio.

Seguem-se os conteúdos do **exemplo\_hive\_preparar\_para\_aml\_full.hql** ficheiro prepara dados para o modelo a criar no Machine Learning:

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Para executar esta consulta a partir da linha de diretório de ramo de registo:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Agora, temos uma tabela interna, **nyctaxidb.nyctaxi_downsampled_dataset**, que podem ser acedidos utilizando o [importar dados] [ import-data] módulo do Machine Learning. Além disso, podemos utilizar este conjunto de dados para a criação de modelos de Machine Learning.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>Utilizar o módulo de importar dados no Machine Learning para aceder aos dados de objeto de amostragem para baixo
Para emitir consultas do Hive a [importar dados] [ import-data] módulo do Machine Learning, terá acesso a uma área de trabalho de aprendizagem. Também precisa de acesso às credenciais do cluster e a respetiva conta de armazenamento associadas.

Seguem-se alguns detalhes sobre o [importar dados] [ import-data] módulo e os parâmetros de entrada:

**URI do servidor de HCatalog**: se o nome do cluster é **abc123**, isto é simplesmente: https://abc123.azurehdinsight.net.

**Nome de conta de utilizador do Hadoop**: O nome de utilizador escolhido para o cluster (não o nome de utilizador de acesso remoto).

**Palavra-passe de conta do Hadoop ser**: A palavra-passe escolhida para o cluster (não o acesso remoto palavra-passe).

**Localização dos dados de saída**: Isto é escolhido para ser o Azure.

**Nome da conta de armazenamento do Azure**: nome da conta do storage predefinida associada com o cluster.

**Nome do contentor do Azure**: Este é o nome do contentor predefinido para o cluster e, normalmente é o mesmo que o nome do cluster. Para um cluster chamado **abc123**, este é abc123.

> [!IMPORTANT]
> Qualquer tabela pretendemos consultar utilizando o [importar dados] [ import-data] módulo no Machine Learning tem de ser uma tabela interna.
> 
> 

Eis como determinar se uma tabela **T** numa base de dados **D.db** é uma tabela interna. A partir da linha de comandos do diretório da ramo de registo, execute o seguinte comando:

    hdfs dfs -ls wasb:///D.db/T

Se a tabela é uma tabela interna e é preenchida, o seu conteúdo deve mostrar aqui.

Outra forma de determinar se uma tabela é uma tabela interna é utilizar o Explorador de armazenamento do Azure. Utilizá-la para navegar para o nome de contentor predefinido do cluster e, em seguida, filtre pelo nome de tabela. Se a tabela e o respetivo conteúdo apresentada, confirma que o se se trata de uma tabela interna.

Eis uma captura de ecrã da consulta do Hive e o [importar dados] [ import-data] módulo:

![Captura de ecrã de consulta do Hive do módulo de importar dados](./media/hive-walkthrough/1eTYf52.png)

Porque os nossos dados de objeto de amostragem de baixo residem num contentor predefinido, a consulta do Hive resultante do Machine Learning é muito simple. É apenas um **SELECIONAR * de nyctaxidb.nyctaxi\_downsampled\_dados**.

O conjunto de dados pode agora ser utilizado como ponto de partida para a criação de modelos de Machine Learning.

### <a name="mlmodel"></a>Criar modelos de Machine Learning
Agora pode avançar para a criação de modelo e implementação de modelo no [Machine Learning](https://studio.azureml.net). Os dados estão prontos para a utilização na corrigir os problemas de predição identificados anteriormente:

- **Classificação binária**: prever ou não uma sugestão foi paga para uma viagem.

  **Learner utilizada:** regressão logística da duas classes

  a. Para este problema, a etiqueta de destino (ou classe) é **tipped**. O conjunto de dados original amostragem em baixo tem algumas colunas que são fugas de destino para esta fase experimental classificação. Em particular, **sugestão\_classe**, **sugestão\_quantidade**, e **total\_quantidade** revelam informações sobre o destino da etiqueta que não está disponível em tempo de teste. Iremos remover estas colunas de consideração ao utilizar o [selecionar colunas no conjunto de dados] [ select-columns] módulo.

  O diagrama seguinte mostra a nossa experiência para prever ou não uma sugestão foi paga para uma determinado viagem:

  ![Diagrama da experimentação](./media/hive-walkthrough/QGxRz5A.png)

  b. Para esta fase experimental, nosso distribuições de etiqueta de destino foram aproximadamente 1:1.

   O gráfico seguinte mostra a distribuição de sugestão etiquetas de classe para o problema de classificação binária:

  ![Gráfico de distribuição de etiquetas de classe de sugestão](./media/hive-walkthrough/9mM4jlD.png)

    Como resultado, obtemos uma área sob a curva (AUC) de 0.987, conforme mostrado na figura seguinte:

  ![Gráfico de valor AUC](./media/hive-walkthrough/8JDT0F8.png)

- **Classificação de várias classes**: prever o intervalo de quantidades de sugestão paga para viagem, utilizando as classes definidas anteriormente.

  **Learner utilizada:** regressão logística da várias classes

  a. Para este problema, é a nossa etiqueta de destino (ou classe) **sugestão\_classe**, o que pode demorar um dos cinco valores (0,1,2,3,4). Como no caso de classificação binária, temos algumas colunas que são fugas de destino para esta fase experimental. Em particular, **tipped**, **sugestão\_quantidade**, e **total\_quantidade** revelar informações sobre a etiqueta de destino que não está disponível em tempo de teste. Iremos remover estas colunas utilizando o [selecionar colunas no conjunto de dados] [ select-columns] módulo.

  O diagrama seguinte mostra a experimentação para prever no qual bin uma sugestão é provável que enquadra-se. Os intervalos de binários são: classe 0: sugestão = $0, 1 de classe: Sugestão > $0 e sugestão < = $5, classe 2: Sugestão > $5 e sugestão < = $10, 3 da classe: Sugestão > $10 e sugestão < = $20 e 4 de classe: Sugestão > $20.

  ![Diagrama da experimentação](./media/hive-walkthrough/5ztv0n0.png)

  Agora vamos mostrar aspeto a distribuição de classe de teste real. Classe 0 e 1 de classe são prevalente atualmente e as outras classes são raras.

  ![Gráfico de distribuição de classe de teste](./media/hive-walkthrough/Vy1FUKa.png)

  b. Para esta fase experimental, utilizamos uma matriz de confusão ao observar os accuracies de predição. Isto é mostrado aqui:

  ![Matriz de confusão](./media/hive-walkthrough/cxFmErM.png)

  Tenha em atenção que enquanto os accuracies de classe em classes de prevalente atualmente são bastante boas, o modelo não fazer um bom trabalho de "learning" nas classes rarer.

- **Tarefa de regressão**: para prever a quantidade de sugestão paga para uma viagem.

  **Learner utilizada:** Boosted árvore de decisões

  a. Para este problema, a etiqueta de destino (ou classe) é **sugestão\_quantidade**. Neste caso são fugas de destino: **tipped**, **sugestão\_classe**, e **total\_quantidade**. Todas estas variáveis revelar informações sobre a quantidade de sugestão está normalmente disponível em tempo de teste. Iremos remover estas colunas utilizando o [selecionar colunas no conjunto de dados] [ select-columns] módulo.

  O diagrama seguinte mostra a experimentação para prever a quantidade da sugestão especificada:

  ![Diagrama da experimentação](./media/hive-walkthrough/11TZWgV.png)

  b. Para problemas de regressão, iremos medir accuracies da predição ao observar o erro ao quadrado no serão as predições e o coeficiente de determinação:

  ![Captura de ecrã de estatísticas de predição](./media/hive-walkthrough/Jat9mrz.png)

  Aqui, o coeficiente de determinação é 0.709, implying que cerca de 71 por cento da variância é explicado por coefficients o modelo.

> [!IMPORTANT]
> Para saber mais sobre o Machine Learning e como aceder e utilizá-lo, consulte o artigo [o que é o Machine Learning](../studio/what-is-machine-learning.md). Além disso, o [galeria do Azure AI](https://gallery.cortanaintelligence.com/) abrange uma gama de experimentações e fornece uma introdução de detalhado para o leque de funcionalidades do Machine Learning.
> 
> 

## <a name="license-information"></a>Informações de licença
Estas instruções de exemplo e os respetivos scripts associadas são partilhados com a licença MIT pela Microsoft. Para obter mais detalhes, consulte o **LICENSE.txt** ficheiro no diretório do código de exemplo no GitHub.

## <a name="references"></a>Referências
• [Andrés Monroy NYC Taxi viagens página de transferência](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi viagem dados por Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi e Limousine Commission investigação e estatísticas](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



