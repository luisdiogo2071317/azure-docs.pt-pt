---
title: Utilizar o Interactive Query com o Azure HDInsight
description: Saiba como utilizar o Interactive Query (LLAP do Hive) com o HDInsight.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/22/2018
ms.openlocfilehash: 73f7e523ed0abc7d0453096cf783761dd6a884ba
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628669"
---
# <a name="use-interactive-query-with-hdinsight"></a>Utilizar uma consulta interativa com o HDInsight
Consulta interativa (também denominado LLAP do Hive, ou [processamento analítico de baixa latência](https://cwiki.apache.org/confluence/display/Hive/LLAP)) é um HDInsight do Azure [tipo de cluster](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Consulta interativa suporta a colocação em cache na memória, que torna as consultas do Hive muito mais rápidas e interativas.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)] 

Um cluster do Interactive Query é diferente de um cluster do Hadoop. Ele contém apenas o serviço do Hive. 

> [!NOTE]
> Pode acessar o serviço do Hive no cluster do Interactive Query apenas através de vista do Hive do Ambari, Beeline e o controlador Microsoft Hive Open Database Connectivity (ODBC do Hive). Não é possível acessá-lo através da consola do Hive, Templeton, a ferramenta de linha de comandos do Azure (CLI) do Azure ou do Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Criar um cluster do Interactive Query
Para obter informações sobre como criar um cluster do HDInsight, consulte [criar clusters Hadoop no HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Escolha o tipo de cluster do Interactive Query.

## <a name="execute-hive-queries-from-interactive-query"></a>Executar consultas do Hive no Interactive Query
Para executar consultas do Hive, tem as seguintes opções:

* Utilizar o Power BI

    Ver [dados visualizar consulta interativa do Hive com o Power BI no Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) veja [visualizar grandes volumes de dados com o Power BI no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
 
* Utilizar Zeppelin

    Ver [utilização Zeppelin para executar consultas do Hive no Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).

* Utilizar o Visual Studio

    Ver [ligue-se ao Azure HDInsight e executar consultas do Hive com o Data Lake Tools para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Utilizar o Visual Studio Code

    Ver [utilização Visual Studio Code para Hive, LLAP ou pySpark](../hdinsight-for-vscode.md).
* Execute o Hive com o modo de exibição do Ambari Hive.
  
    Ver [utilizar a vista do Hive com o Hadoop no HDInsight do Azure](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Execute o Hive com o Beeline.
  
    Ver [utilizar o Hive com o Hadoop no HDInsight com o Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Pode utilizar Beeline do nó principal ou a partir de um nó de extremidade vazio. Recomendamos que utilize o Beeline de um nó de extremidade em branco. Para obter informações sobre como criar um cluster do HDInsight ao utilizar um nó de extremidade em branco, consulte [utilizar nós de extremidade vazios no HDInsight](../hdinsight-apps-use-edge-node.md).
* Execução do Hive através de ODBC do Hive.
  
    Ver [ligar o Excel ao Hadoop com o controlador Microsoft Hive ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Para localizar a cadeia de ligação de conectividade de banco de dados de Java (JDBC):

1. Inicie sessão no Ambari, com o seguinte URL: https://\<nome do cluster\>. AzureHDInsight.net.
2. No menu da esquerda, selecione **Hive**.
3. Para copiar o URL, selecione o ícone de área de transferência:
   
   ![Hadoop do HDInsight Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [criar clusters do Interactive Query no HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Saiba como [visualizar grandes volumes de dados com o Power BI no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Saiba como [utilizar o Zeppelin para executar consultas do Hive no Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).
* Saiba como [executar consultas do Hive com o Data Lake Tools para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Saiba como [utilizar ferramentas do HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* Saiba como [utilizar a vista do Hive com o Hadoop no HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Saiba como [utilizar Beeline para submeter consultas do Hive no HDInsight](../hadoop/apache-hadoop-use-hive-beeline.md).
* Saiba como [ligar o Excel ao Hadoop com o controlador Microsoft Hive ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

