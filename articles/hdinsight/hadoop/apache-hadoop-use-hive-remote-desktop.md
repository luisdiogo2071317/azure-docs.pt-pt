---
title: Utilizar o Hive do Hadoop e o ambiente de trabalho remoto no HDInsight - Azure
description: Saiba como ligar ao cluster do Hadoop no HDInsight com o ambiente de trabalho remoto e, em seguida, executar consultas do Hive utilizando a Interface de linha de comandos do Hive.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 3f59f0b396c50d1b7694f831bbe44649c1e5f760
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590488"
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Utilizar o Hive com o Hadoop no HDInsight com o ambiente de trabalho remoto
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Neste artigo, irá aprender a ligar a um cluster do HDInsight com o ambiente de trabalho remoto e, em seguida, executar consultas do Hive com o ramo de registo Interface de linha de comandos (CLI).

> [!IMPORTANT]
> Área de trabalho remota só está disponível em clusters do HDInsight que utilizam o Windows como o sistema operativo. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).
>
> Para o HDInsight 3.4 ou superior, consulte [utilizar o Hive com o HDInsight e o Beeline](apache-hadoop-use-hive-beeline.md) para informações sobre como executar consultas do Hive diretamente no cluster a partir de uma linha de comandos.

## <a id="prereq"></a>Pré-requisitos
Para concluir os passos neste artigo, precisará do seguinte:

* Um cluster do HDInsight baseado em Windows (Hadoop no HDInsight)
* Um computador cliente executando o Windows 10, Windows 8 ou Windows 7

## <a id="connect"></a>Ligue-se a área de trabalho remota
Ativar o ambiente de trabalho remoto para o cluster do HDInsight, em seguida, ligá-la ao seguir as instruções em [ligar a clusters do HDInsight através do RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Utilize o comando do Hive
Quando tiver ligado à área de trabalho para o cluster do HDInsight, utilize os passos seguintes para trabalhar com o Hive:

1. A partir da área de trabalho do HDInsight, inicie o **linha de comandos do Hadoop**.
2. Introduza o seguinte comando para iniciar a CLI do Hive:

        %hive_home%\bin\hive

    Quando tiver iniciado a CLI, poderá ver a linha de comandos do CLI do Hive: `hive>`.
3. Com a CLI, introduza as seguintes instruções para criar uma nova tabela com o nome **log4jLogs** com dados de exemplo:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Essas instruções executam as seguintes ações:

   * **DROP TABLE**: elimina a tabela e o ficheiro de dados, se a tabela já existe.
   * **Criar tabela externa**: cria uma nova tabela de "externa" no Hive. Tabelas externas armazenam apenas a definição de tabela no Hive (resta os dados na localização original).

     > [!NOTE]
     > Tabelas externas devem ser utilizadas quando espera que os dados subjacentes sejam atualizados por uma origem externa (por exemplo, um processo de carregamento de dados automatizada) ou por outra operação de MapReduce, mas é sempre melhor consultas do Hive para utilizar os dados mais recentes.
     >
     > Remover uma tabela externa faz **não** eliminar os dados, apenas a definição da tabela.
     >
     >
   * **FORMATO de linha**: informa ao Hive como os dados estiverem formatados. Neste caso, os campos em cada registo são separados por um espaço.
   * **LOCALIZAÇÃO de TEXTFILE AS ARMAZENADOS**: informa ao ramo de registo onde os dados são armazenados (o diretório/dados de exemplo) e que são armazenados como texto.
   * **SELECIONE**: seleciona uma contagem de todas as linhas em que coluna **t4** contém o valor **[erro]**. Isto deverá devolver um valor de **3** porque existem três linhas que contêm este valor.
   * **INPUT__FILE__NAME como '%.log'** -informa ao Hive que podemos deverá devolver apenas dados de ficheiros terminados em. log. Isso restringe a pesquisa para o ficheiro Sample log que contém os dados e impede que ela retornar dados do outro exemplo, ficheiros de dados que não correspondem o esquema que definimos.
4. Utilize as seguintes instruções para criar uma nova tabela de "interna" com o nome **registos de erros**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Essas instruções executam as seguintes ações:

   * **Criar tabela se não existe**: cria uma tabela se ainda não exista. Uma vez que o **externo** palavra-chave não é utilizado, esta é uma tabela interna, que é armazenada no armazém de dados de Hive e é gerenciada completamente o Hive.

     > [!NOTE]
     > Ao contrário **externo** tabelas, remover uma tabela interna também elimina os dados subjacentes.
     >
     >
   * **ARMAZENADOS ORC de AS**: armazena os dados no formato de (ORC) em colunas de linha otimizada. Este é um formato altamente otimizado e eficiente para armazenar os dados de Hive.
   * **SUBSTITUIR INSERT... SELECIONE**: seleciona linhas do **log4jLogs** tabela que contêm **[erro]**, em seguida, insere os dados no **registos de erros** tabela.

     Para verificar que apenas linhas que contêm **[erro]** na coluna t4 foram armazenados para o **registos de erros** da tabela, utilize a seguinte instrução para retornar todas as linhas da **registos de erros**:

       SELECIONAR * de registos de erros;

     Três linhas de dados devem ser devolvidas, tudo que contêm **[erro]** na coluna t4.

## <a id="summary"></a>Resumo
Como pode ver, o comando de ramo de registo fornece uma forma fácil de executar consultas do Hive num cluster do HDInsight interativamente, monitorizar o estado da tarefa e obter o resultado.

## <a id="nextsteps"></a>Passos seguintes
Para obter informações gerais sobre o Hive no HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)

Se estiver a utilizar com o Hive no Tez, consulte os seguintes documentos para informações de depuração:

* [Utilize a IU do Tez no HDInsight baseado em Windows](../hdinsight-debug-tez-ui.md)
* [Utilize a vista Ambari Tez no HDInsight baseado em Linux](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
