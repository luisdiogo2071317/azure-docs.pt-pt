---
title: Visualizar dados de consulta interativa do Hive com o Power BI no Azure HDInsight
description: Saiba como utilizar o Microsoft Power BI para visualizar dados de consulta interativa do Hive processados pelo Azure HDInsight.
keywords: hdinsight, hadoop, hive, interactive consultas interativas do hive, LLAP, directquery
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/14/2018
ms.openlocfilehash: 4dcfcb5e70b9eb6626be1f3528781a8c5b1bd5c4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593031"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Visualizar dados de consulta interativa do Hive com o Microsoft Power BI através da consulta direta no Azure HDInsight

Saiba como ligar o Microsoft Power BI para clusters do Azure HDInsight Interactive Query e visualize os dados de Hive através da consulta direta. Neste tutorial, carregue os dados de uma tabela de Hive hivesampletable para o Power BI. A tabela de Hive contém alguns dados de utilização do telefone celular. Em seguida, representar os dados de utilização num mapa-mundo:

![HDInsight Power BI, o relatório de mapa](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Pode aproveitar o [controlador ODBC do Hive](../hadoop/apache-hadoop-connect-hive-power-bi.md) para importar através do conector ODBC genérico no Power BI Desktop. No entanto não é recomendado para cargas de trabalho de BI dadas a natureza não interativa do motor de consulta do Hive. [Conector do Interactive Query do HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md) e [conector do Spark do HDInsight](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) são opções melhor para seu desempenho.

## <a name="prerequisites"></a>Pré-requisitos
Antes de passar por este artigo, tem de ter os seguintes itens:

* **Cluster de HDInsight**. O cluster pode ser um cluster do HDInsight com o Hive ou um cluster do Interactive Query recém-lançada. Para criar clusters, veja [criar um cluster](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. Pode baixar uma cópia a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Carregar dados do HDInsight

A tabela de Hive hivesampletable é fornecido com todos os clusters do HDInsight.

1. Inicie sessão no Power BI Desktop.
2. Clique nas **home page** separador, clique em **obter dados** partir o **dados externos** Friso e, em seguida, selecione **mais**.

    ![Dados de open HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Partir do **obter dados** painel, escreva **hdinsight** na caixa de pesquisa. Se não vir **Interactive Query do HDInsight (Beta)**, tem de atualizar o seu ambiente de trabalho do Power BI para a versão mais recente.
4. Clique em **Interactive Query do HDInsight (Beta)** e, em seguida, clique em **Connect**.
5. Clique em **continuar** para fechar a **conector de pré-visualização** caixa de diálogo de aviso.
6. Partir **Interactive Query do HDInsight**, selecione ou introduza as seguintes informações:

    - **Servidor**: introduza o nome do cluster do Interactive Query, por exemplo *myiqcluster.azurehdinsight.net*.
    - **Base de dados**: para este tutorial, introduza **predefinição**.
    - **Modo de conectividade de dados**: para este tutorial, selecione **DirectQuery**.

    ![Ligar o HDInsight consulta interativa power bi directquery](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. Clique em **OK**.
8. Introduza as credenciais de utilizador HTTP e, em seguida, clique em **OK**.  O nome de utilizador predefinido é **admin**
9. No painel esquerdo, selecione **hivesampletale**e, em seguida, clique em **carga**.

    ![HDInsight consulta interativa power bi hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Visualizar dados

Continue a partir do último procedimento.

1. No painel visualizações, selecione **mapa**.  É um ícone de globo.

    ![HDInsight Power BI personaliza o relatório](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. No painel campos, selecione **país** e **devicemake**. Pode ver os dados desenhados no mapa.
3. Expanda o mapa.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a visualizar dados do HDInsight com o Power BI.  Para obter mais informações, consulte os artigos seguintes:

* [Visualizar dados do Hive com o Microsoft Power BI com ODBC no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Utilizar o Zeppelin para executar consultas do Hive no Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Ligar o Excel ao HDInsight com o controlador Microsoft Hive ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Ligar o Excel ao Hadoop com o Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Ligar ao Azure HDInsight e executar consultas do Hive com o Data Lake Tools para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Utilize a ferramenta do Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carregar dados para o HDInsight](./../hdinsight-upload-data.md).
