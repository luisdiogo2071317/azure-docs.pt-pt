---
title: 'Tutorial: Realizar a extração, transformação, operações de carregamento (ETL), utilizando o Apache Hive no HDInsight do Azure'
description: Neste tutorial, saiba como extrair dados de um conjunto de dados do CSV não processado, transformá-los com o Apache Hive no Azure HDInsight e, em seguida, carregá-los no banco de dados do Azure SQL com o Sqoop.
services: storage
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: jamesbak
ms.openlocfilehash: 65d2d69c788a54371664d1a443a79bd121332470
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105157"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-apache-hive-on-azure-hdinsight"></a>Tutorial: Extrair, transformar e carregar dados com o Apache Hive no HDInsight do Azure

Neste tutorial, vai realizar uma operação de ETL: extrair, transformar e carregar dados. Pega um arquivo de dados não processado do CSV, importe-o para um cluster do Azure HDInsight, transformá-los com o Apache Hive e carregá-los para uma base de dados SQL do Azure com o Apache Sqoop.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Extrair e carregar os dados para um cluster do HDInsight.
> * Transforme os dados utilizando o Apache Hive.
> * Carregar os dados para uma base de dados SQL do Azure ao utilizar o Sqoop.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* **Um cluster do Hadoop baseado em Linux no HDInsight**: Para criar um novo cluster do HDInsight baseado em Linux, veja [configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](./data-lake-storage-quickstart-create-connect-hdi-cluster.md).

* **Base de dados SQL do Azure**: Vai utilizar uma base de dados SQL do Azure como arquivo de dados de destino. Se não tiver uma base de dados SQL, veja [Criar uma base de dados SQL do Azure no portal do Azure](../../sql-database/sql-database-get-started.md).

* **CLI do Azure**: Se ainda não instalou a CLI do Azure, consulte [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Um cliente SSH**: Para obter mais informações, consulte [ligar ao HDInsight (Hadoop) através de SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Os passos neste artigo necessitam de um cluster do HDInsight que utiliza o Linux. Linux é o único sistema operativo que é utilizado no Azure HDInsight versão 3.4 ou posterior. Para obter mais informações, veja [HDInsight retirement on Windows](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

### <a name="download-the-flight-data"></a>Transferir os dados de voos

Este tutorial utiliza dados de voo do Bureau de estatísticas de transporte para demonstrar como realizar uma operação de ETL. Tem de transferir estes dados para concluir o tutorial.

1. Aceda a [pesquisa e administração de tecnologia inovadora, Bureau de estatísticas de transportes](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

1. Na página, selecione os seguintes valores:

   | Nome | Valor |
   | --- | --- |
   | **Ano de filtro** |2013 |
   | **Período de filtro** |Janeiro |
   | **Campos** |Year (Ano), FlightDate (Data do Voo), UniqueCarrier (Transportadora Individual), Carrier (Transportadora), FlightNum (Número do Voo), OriginAirportID (ID do Aeroporto de Origem), Origin (Origem), OriginCityName (Nome da Cidade de Origem), OriginState (Estado de Origem), DestAirportID (ID do Aeroporto de Destino), Dest (Destino), DestCityName (Nome da Cidade de Destino), DestState (Estado de Destino), DepDelayMinutes (Minutos de Atraso da Partida), ArrDelay (Atraso à Chegada), ArrDelayMinutes (Minutos de Atraso à Chegada), CarrierDelay (Atraso da Transportadora), WeatherDelay (Atraso Devido às Condições Atmosféricas), NASDelay (Atraso Devido à NAS), SecurityDelay (Atraso Devido a Segurança), LateAircraftDelay (Atraso Devido a Avião Atrasado). |

1. Limpe todos os outros campos.

1. Selecione **Transferir**. Obter um ficheiro. zip com os campos de dados que selecionou.

## <a name="extract-and-upload-the-data"></a>Extrair e carregar os dados

Nesta secção, vai utilizar `scp` para carregar dados para o seu cluster do HDInsight.

Abra uma linha de comandos e utilize o seguinte comando para carregar o ficheiro .zip para o nó principal do cluster do HDInsight:

```bash
scp <FILE_NAME>.zip <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net:<FILE_NAME.zip>
```

* Substitua \<nome_ficheiro > com o nome do ficheiro. zip.
* Substitua \<SSH_USER_NAME > com o início de sessão SSH para o cluster do HDInsight.
* Substitua \<CLUSTER_NAME > com o nome do HDInsight cluster.

Se utilizar uma palavra-passe para autenticar o início de sessão SSH, é-lhe pedida a palavra-passe. 

Se utilizar uma chave pública, poderá ter de utilizar o parâmetro `-i` e especificar o caminho para a chave privada correspondente. Por exemplo, `scp -i ~/.ssh/id_rsa FILE_NAME.zip USER_NAME@CLUSTER_NAME-ssh.azurehdinsight.net:`.

Depois de concluído o carregamento, utilize SSH para ligar ao cluster. Na linha de comandos, introduza o seguinte comando:

```bash
ssh <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net
```

Utilize o seguinte comando para descomprimir o ficheiro .zip:

```bash
unzip <FILE_NAME>.zip
```

O comando extrai uma **. csv** ficheiro que é, aproximadamente, 60 MB.

Utilize os seguintes comandos para criar um diretório e copie o ficheiro *.csv* para o diretório:

```bash
hdfs dfs -mkdir -p abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data
hdfs dfs -put <FILE_NAME>.csv abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data/
```

Utilize o seguinte comando para criar o sistema de ficheiros de geração 2 de armazenamento do Data Lake.

```bash
hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/
```

## <a name="transform-the-data"></a>Transformar os dados

Nesta secção, vai utilizar Beeline para executar uma tarefa Apache Hive.

Como parte do trabalho do Apache Hive, importar os dados do ficheiro. csv para uma tabela com o nome do Apache Hive **atrasos**.

Na linha de comandos SSH que já tem para o cluster do HDInsight, utilize o seguinte comando para criar e editar um ficheiro novo designado **flightdelays.hql**:

```bash
nano flightdelays.hql
```

Utilize o seguinte texto como o conteúdo desse ficheiro:

```hiveql
 DROP TABLE delays_raw;
 -- Creates an external table over the csv file
 CREATE EXTERNAL TABLE delays_raw (
    YEAR string,
    FL_DATE string,
    UNIQUE_CARRIER string,
    CARRIER string,
    FL_NUM string,
    ORIGIN_AIRPORT_ID string,
    ORIGIN string,
    ORIGIN_CITY_NAME string,
    ORIGIN_CITY_NAME_TEMP string,
    ORIGIN_STATE_ABR string,
    DEST_AIRPORT_ID string,
    DEST string,
    DEST_CITY_NAME string,
    DEST_CITY_NAME_TEMP string,
    DEST_STATE_ABR string,
    DEP_DELAY_NEW float,
    ARR_DELAY_NEW float,
    CARRIER_DELAY float,
    WEATHER_DELAY float,
    NAS_DELAY float,
    SECURITY_DELAY float,
    LATE_AIRCRAFT_DELAY float)
 -- The following lines describe the format and location of the file
 ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
 LINES TERMINATED BY '\n'
 STORED AS TEXTFILE
 LOCATION 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data';

-- Drop the delays table if it exists
DROP TABLE delays;
-- Create the delays table and populate it with data
-- pulled in from the CSV file (via the external table defined previously)
CREATE TABLE delays
LOCATION abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/processed
AS
SELECT YEAR AS year,
    FL_DATE AS flight_date,
    substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
    substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
    substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
    ORIGIN_AIRPORT_ID AS origin_airport_id,
    substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
    substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
    substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
    DEST_AIRPORT_ID AS dest_airport_id,
    substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
    substring(DEST_CITY_NAME,2) AS dest_city_name,
    substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
    DEP_DELAY_NEW AS dep_delay_new,
    ARR_DELAY_NEW AS arr_delay_new,
    CARRIER_DELAY AS carrier_delay,
    WEATHER_DELAY AS weather_delay,
    NAS_DELAY AS nas_delay,
    SECURITY_DELAY AS security_delay,
    LATE_AIRCRAFT_DELAY AS late_aircraft_delay
FROM delays_raw;
```

Para guardar o ficheiro, selecione a tecla Esc e, em seguida, introduza `:x`.

Para iniciar o Hive e executar o ficheiro **flightdelays.hql**, utilize o seguinte comando:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
```

Quando a execução do script __flightdelays.hql__ for concluída, utilize o seguinte comando para abrir uma sessão interativa do Beeline:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
```

Quando receber a linha de comandos `jdbc:hive2://localhost:10001/>`, utilize a seguinte consulta para obter dados a partir dos dados importados de atrasos de voos:

```hiveql
INSERT OVERWRITE DIRECTORY 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output'
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
SELECT regexp_replace(origin_city_name, '''', ''),
    avg(weather_delay)
FROM delays
WHERE weather_delay IS NOT NULL
GROUP BY origin_city_name;
```

Esta consulta obtém uma lista de cidades em que os voos se atrasaram devido às condições atmosféricas, juntamente com o tempo de atraso médio e guarda-a em `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output`. Depois, o Sqoop lê os dados a partir dessa localização e exporta-os para a Base de Dados SQL do Azure.

Para sair do Beeline, introduza `!quit` na linha de comandos.

## <a name="create-a-sql-database-table"></a>Criar uma base de dados SQL

É necessário o nome de servidor da base de dados SQL para esta operação. Conclua estes passos para encontrar o nome do servidor.

1. Aceda ao [Portal do Azure](https://portal.azure.com).

1. Selecione **bases de dados SQL**.

1. Filtrar no nome da base de dados que optar por utilizar. O nome do servidor é mostrado na coluna **Nome do servidor**.

1. Filtrar no nome da base de dados que pretende utilizar. O nome do servidor é mostrado na coluna **Nome do servidor**.

    ![Obter os detalhes do servidor SQL do Azure](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Obter os detalhes do servidor SQL do Azure")

    Existem muitas formas de ligar à Base de Dados SQL e criar uma tabela. Os passos seguintes utilizam [FreeTDS](http://www.freetds.org/) do cluster do HDInsight.

Para instalar o FreeTDS, utilize o seguinte comando a partir de uma ligação SSH ao cluster:

```bash
sudo apt-get --assume-yes install freetds-dev freetds-bin
```

Depois de concluída a instalação, utilize o seguinte comando para ligar ao servidor de base de dados SQL.

* Substitua \<SERVER_NAME > com o nome do servidor de base de dados SQL.
* Substitua \<ADMIN_LOGIN > com o início de sessão de administrador para a base de dados SQL.
* Substitua \<DATABASE_NAME > com o nome de base de dados.

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -p 1433 -D <DATABASE_NAME>
```

Quando lhe for pedido, introduza a palavra-passe para início de sessão de administrador da base de dados SQL.

Receberá um resultado semelhante ao seguinte texto:

```
locale is "en_US.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
Default database being set to sqooptest
1>
```

Na `1>` , introduza as seguintes instruções:

```hiveql
CREATE TABLE [dbo].[delays](
[origin_city_name] [nvarchar](50) NOT NULL,
[weather_delay] float,
CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
([origin_city_name] ASC))
GO
```

Quando for introduza a declaração `GO`, as instruções anteriores são avaliadas.
A consulta cria uma tabela denominada **atrasos**, que tem um índice em cluster.

Utilize a seguinte consulta para verificar se a tabela é criada:

```hiveql
SELECT * FROM information_schema.tables
GO
```

O resultado é semelhante ao seguinte texto:

```
TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
databaseName       dbo             delays        BASE TABLE
```

Introduza `exit` na linha de comandos `1>` para sair do utilitário tsql.

## <a name="export-and-load-the-data"></a>Exportar e carregar os dados

Nas seções anteriores, que copiou os dados transformados na localização `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output`. Nesta secção, vai utilizar Sqoop para exportar os dados de `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` para a tabela criada na base de dados SQL do Azure.

Utilize o seguinte comando para verificar se o Sqoop pode ver a sua base de dados SQL:

```bash
sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
```

O comando devolve uma lista de bases de dados, incluindo a base de dados em que criou o **atrasos** tabela.

Utilize o seguinte comando para exportar dados a partir da **hivesampletable** de tabela para o **atrasos** tabela:

```bash
sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<FILE_SYSTEM_NAME>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
```

Sqoop liga-se à base de dados que contém o **atrasos** tabela e dados de exportações da `/tutorials/flightdelays/output` diretório para o **atrasos** tabela.

Depois do `sqoop` comando estiver concluído, utilize o utilitário de tsql para ligar à base de dados:

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
```

Utilize as seguintes instruções para verificar que os dados foram exportados para o **atrasos** tabela:

```sql
SELECT * FROM delays
GO
```

Deverá ver uma lista dos dados na tabela. A tabela inclui o nome da cidade e o tempo médio dos atrasos dos voos.

Introduza `exit` para saia do utilitário de tsql.

## <a name="clean-up-resources"></a>Limpar recursos

Todos os recursos utilizados neste tutorial são pré-existentes. Limpeza de não é necessária.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais formas de trabalhar com dados no HDInsight, consulte o artigo seguinte:

> [!div class="nextstepaction"]
> [Extrair, transformar e carregar dados com o Azure Databricks](./data-lake-storage-use-hdi-cluster.md)
