---
title: Utilizar o Zeppelin para executar consultas do Hive no HDInsight do Azure
description: Saiba como utilizar o Zeppelin para executar consultas do Hive.
keywords: o hdinsight, hadoop, hive, a consulta interativa, o LLAP
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: 3064c9cd141458307891f666bd5af9aa738cc021
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43093173"
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Utilizar o Zeppelin para executar consultas do Hive no HDInsight do Azure 

Os clusters do Interactive Query do HDInsight incluem blocos de notas do Zeppelin que pode utilizar para executar consultas interativas do Hive. Neste artigo, irá aprender a utilizar o Zeppelin para executar consultas do Hive no HDInsight do Azure. 

## <a name="prerequisites"></a>Pré-requisitos
Antes de passar por este artigo, tem de ter os seguintes itens:

* **Cluster do HDInsight Interactive Query**. Ver [criar um cluster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) para criar um cluster do HDInsight.  Certifique-se de escolher o tipo de consulta interativa. 

## <a name="create-a-zeppelin-note"></a>Criar uma nota de Zeppelin

1. Navegue para o URL seguinte:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Substitua **CLUSTERNAME** pelo nome do cluster.

2. Introduza o nome de utilizador do Hadoop e a palavra-passe. Na página Zeppelin, pode criar uma nova nota ou abra notas existentes. HiveSample contém alguns exemplos de consultas do Hive.  

    ![Zeppelin Interactive Query do HDInsight](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Clique em **criar nova nota**.
4. Escreva ou selecione os seguintes valores:

    - Nome da observação: introduza um nome para a nota.
    - O interpretador de predefinição: selecione **JDBC**.

5. Clique em **criar nota**.
6. Execute a seguinte consulta do Hive:

        %jdbc(hive)
        show tables

    ![Consulta de execuções do Interactive Query do HDInsight zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    O **%jdbc(hive)** instrução na primeira linha informa o bloco de notas para utilizar o interpretador de JDBC do Hive.

    A consulta deverá devolver uma tabela do Hive chamada *hivesampletable*.

    Seguem-se dois mais consultas do Hive que pode ser executado em relação a hivesampletable. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Comparando com o Hive tradicional, os resultados da consulta voltar tem mais rapidamente.


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a visualizar dados do HDInsight com o Power BI.  Para obter mais informações, consulte os artigos seguintes:

* [Visualizar dados de Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Visualizar dados de consulta interativa do Hive com o Power BI no Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Ligar o Excel ao HDInsight com o controlador Microsoft Hive ODBC](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Ligar o Excel ao Hadoop com o Power Query](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Ligar ao Azure HDInsight e executar consultas do Hive com o Data Lake Tools para Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Utilize a ferramenta do Azure HDInsight para Visual Studio Code](hdinsight-for-vscode.md).
* [Carregar dados para o HDInsight](./hdinsight-upload-data.md).
