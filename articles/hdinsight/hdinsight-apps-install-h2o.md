---
title: Instalação publicado aplicação - H2O Sparkling máximo - Azure HDInsight | Microsoft Docs
description: Instalar e utilizar a aplicação de Hadoop do H2O Sparkling máximo por terceiros.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 9a03588b3327c3ab231f5c2cae17488f4d63bde7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31402112"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Instalar a aplicação publicada - H2O Sparkling máximo

Este artigo descreve como instalar e executar o [H20 Sparkling máximo](http://www.h2o.ai/) publicado a aplicação de Hadoop no Azure HDInsight. Para obter uma descrição geral da plataforma de aplicação do HDInsight e uma lista de disponíveis independentes de Software (ISV) aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>Sobre H2O Sparkling máximo

H2O Sparkling máximo é um open source, plataforma de learning totalmente distribuída em memória máquina com escalabilidade linear. Máximo de Sparkling H2O permitem combinar o rápida e dimensionável algoritmos do machine learning de H2O com as capacidades do Spark. Com Sparkling máximo, os utilizadores podem unidade cálculo de Scala, R e Python utilizando a IU de fluxo de H2O.

Máximo de Sparkling H2O fornece:

* **Fácil de utilizar WebUI e interfaces familiares** – definir configurar e começar rapidamente a utilizar qualquer um dos H2O intuitiva baseada na web fluxo GUI ou ambientes como R, Python, Java, Scala, JSON e as APIs de H2O de programação.
* **Suporte de dados-agnóstica para todos os tipos de base de dados e ficheiros comuns** – facilmente explorar e modelo de macrodados do dentro do Microsoft Excel, R Studio, Tableau e muito mais. Ligar-se aos dados a partir de origens de dados do HDFS, S3, SQL e NoSQL.
* **Extremamente dimensionável munging de macrodados e análise** – associações grande H2O pode efetuar 7 x mais rápida do que as operações de data.table R e dimensionar de forma linear para milhões de 10 x associações de linha de mil milhões de 10.
* **A classificação de dados em tempo real** – rapidamente implementar modelos em produção através de objetos de Java simples antigo (POJO), com otimização de modelo de objetos Java (MOJO), ou a API de REST H2O.

### <a name="resource-links"></a>Ligações de recursos

* [Plano de engenharia H2O.ai](https://jira.h2o.ai/)
* [Home page do H2O.ai](http://www.h2o.ai/)
* [Documentação de H2O.ai](http://docs.h2o.ai/)
* [Suporte de H2O.ai](https://support.h2o.ai/)
* [Código base de código aberto H2O.ai](https://github.com/h2oai/)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação no novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Cluster tier(s): Standard ou Premium
* Tipo de cluster: Spark
* Versões de cluster: 3.5 ou 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Instalar o máximo de Sparkling H2O publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações de ISV disponíveis, leia o artigo [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>Iniciar H2O Sparkling máximo

1. Após a instalação, pode começar a utilizar H2O Sparkling máximo (h2o sparklingwater) do seu cluster no portal do Azure, abrindo blocos de notas do Jupyter (`https://<ClusterName>.azurehdinsight.net/jupyter`). Pode também aceder à Jupyter selecionando **Cluster dashboard** a partir do painel do seu cluster no portal, em seguida, selecionar **bloco de notas do Jupyter**. Lhe for pedido para introduzir as suas credenciais. Introduza as credenciais de Hadoop do cluster tal como especificado na criação do cluster.

2. No Jupyter, verá três pastas: H2O-PySparkling-exemplos, PySpark exemplos e Scala exemplos. Selecione o **exemplos de PySparkling H2O** pasta.

    ![Blocos de notas do Jupyter inicial](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. O primeiro passo ao criar um novo bloco de notas é configurar o ambiente de Spark. Estas informações estão incluídas no **Sentiment_analysis_with_Sparkling_Water** exemplo. Quando configurar o ambiente do Spark, lembre-se de que utilize o jar correto e especifique o endereço IP fornecido pela saída da primeira célula.

    ![Blocos de notas do Jupyter inicial](./media/hdinsight-apps-install-h2o/spark-config.png)

4. Inicia o Cluster de H2O.

    ![Iniciar o cluster](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. Depois do Cluster H2O está a funcionar, abra H2O fluxo acedendo a **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`**.

    > [!NOTE]
    > Se não for possível abrir o fluxo H2O, tente limpar a cache do browser. Se ainda não é possível contactar, provavelmente, não tem recursos suficientes no cluster. Tente aumentar o número de nós de trabalho sob o **cluster de escala** opção no painel do cluster.

    ![Fluxo H2O dashboard](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Selecione o **Million_Songs.flow** exemplo no menu à direita. Quando lhe for pedido um aviso, clique em **carga bloco de notas**. Esta demonstração foi concebida para ser executada dentro de alguns minutos, com dados reais. O objetivo é para prever a partir dos dados se o song foi libertado antes ou depois de 2004 utilizando classificação binária.

    ![Selecione Million_Songs.flow](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Localizar o caminho que contém **milsongs-conformidade com cls-train.csv.gz**e substitua o caminho completo com **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Localizar o caminho que contém **milsongs-conformidade com cls-test.csv.gz** e substitua-o com **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Para executar todas as instruções nas células bloco de notas, selecione o **executar todos os** botão na barra de ferramentas.

    ![Executar todos](./media/hdinsight-apps-install-h2o/run-all.png)

10. Após alguns minutos, deverá ver um resultado semelhante ao seguinte.

    ![Saída](./media/hdinsight-apps-install-h2o/output.png)

Já está! Tiver harnessed intelligence artificial no Spark dentro de um fim de minutos. Agora pode explorar mais exemplos H2O fluxo que demonstram diferentes tipos de algoritmos do machine learning.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de H2O](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Instalar aplicações HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada no HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux através da ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilize nós de limite vazio no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder a clusters do HDInsight e para testar e alojamento de aplicações do HDInsight.
