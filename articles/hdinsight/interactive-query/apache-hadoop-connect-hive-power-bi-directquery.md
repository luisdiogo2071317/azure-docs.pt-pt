---
title: Visualizar dados de ramo de registo de consultas interativas com o Power BI no Azure HDInsight | Microsoft Docs
description: Saiba como utilizar o Microsoft Power BI para visualizar dados de consulta de ramo de registo interativo processados pelo Azure HDInsight.
keywords: hdinsight, hadoop, o hive, interativa consulta hive interativa, LLAP, directquery
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: jgao
ms.openlocfilehash: 290e600b7be4a6f9fb57afa50bb771e42e6a0624
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Visualizar dados de ramo de registo de consultas interativas com o Microsoft Power BI utilizando a consulta direta no Azure HDInsight

Saiba como ligar o Microsoft Power BI para clusters de consulta interativos do Azure HDInsight e visualizar os dados de Hive consulta direta a utilizar. Neste tutorial, carregar os dados de uma tabela de Hive hivesampletable ao Power BI. A tabela de Hive contém alguns dados de utilização de telemóvel. Em seguida, representar os dados de utilização num mapa mundo:

![HDInsight Power BI o relatório de mapa](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Como ligar ao ramo ODBC a utilizar, consulte [dados visualizar o Hive com o Microsoft Power BI utilizando ODBC no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 

## <a name="prerequisites"></a>Pré-requisitos
Antes de passar neste artigo, tem de ter os seguintes itens:

* **Cluster do HDInsight**. O cluster pode ser um cluster do HDInsight com o Hive ou um cluster de consulta interativa recentemente publicado. Para criar clusters, consulte [Criar cluster](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Ambiente de trabalho do Microsoft Power BI](https://powerbi.microsoft.com/desktop/)**. Pode transferir uma cópia do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Carregar dados do HDInsight

A tabela de Hive hivesampletable inclui todos os clusters do HDInsight.

1. Inicie sessão no Power BI Desktop.
2. Clique em de **home page** separador, clique em **obter dados** do **dados externos** do Friso e, em seguida, selecione **mais**.

    ![Dados aberto HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Do **obter dados** painel, escreva **hdinsight** na caixa de pesquisa. Se não vir **HDInsight interativa consulta (Beta)**, tem de atualizar o seu ambiente de trabalho do Power BI para a versão mais recente.
4. Clique em **HDInsight interativa consulta (Beta)**e, em seguida, clique em **Connect**.
5. Clique em **continuar** para fechar o **conector de pré-visualização** caixa de diálogo de aviso.
6. De **consulta interativa HDInsight**, selecione ou introduza as seguintes informações:

    - **Servidor**: introduza o nome do cluster de consulta interativa, por exemplo *myiqcluster.azurehdinsight.net*.
    - **Base de dados**: para este tutorial, introduza **predefinido**.
    - **Modo de conectividade de dados**: para este tutorial, selecione **DirectQuery**.

    ![HDInsight consulta interativa power bi directquery ligar](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. Clique em **OK**.
8. Introduza as credenciais de utilizador HTTP e, em seguida, clique em **OK**.  O nome de utilizador predefinido é **admin**
9. No painel esquerdo, selecione **hivesampletale**e, em seguida, clique em **carga**.

    ![HDInsight consulta interativa power bi hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Visualizar dados

Continue a partir do último procedimento.

1. A partir do painel de visualizações, selecione **mapa**.  É um ícone de globo.

    ![Relatório de customizes HDInsight Power BI](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. No painel campos, selecione **país** e **devicemake**. Pode ver os dados desenhados no mapa.
3. Expanda o mapa.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a visualizar dados do HDInsight através do Power BI.  Para obter mais informações, consulte os artigos seguintes:

* [Visualizar dados do Hive com o Microsoft Power BI utilizando ODBC no Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Utilize Zeppelin para executar consultas do Hive no Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Ligar o Excel para o HDInsight com o controlador ODBC do Microsoft Hive](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Ligar o Excel ao Hadoop com o Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Ligar ao Azure HDInsight e executar consultas do Hive, utilizando ferramentas do Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Utilizar a ferramenta do Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carregar dados para o HDInsight](./../hdinsight-upload-data.md).
