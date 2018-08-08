---
title: Instalar a aplicação publicada - Sparkling Water da H2O - Azure HDInsight
description: Instalar e utilizar a aplicação do Hadoop de terceiros Sparkling Water da H2O.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: e1e43bc06b30959bc5e7e692d88bd0d053a03ad4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593558"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Instalar a aplicação publicada - Sparkling Water da H2O

Este artigo descreve como instalar e executar o [H20 Sparkling Water](http://www.h2o.ai/) publicado a aplicação do Hadoop no HDInsight do Azure. Para uma visão geral da plataforma de aplicações do HDInsight e uma lista de disponíveis independentes fabricante de Software (ISV) a aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>Sobre o Sparkling Water da H2O

O Sparkling Water da H2O é uma código-fonte aberto, plataforma de aprendizagem de máquina de dentro da memória totalmente distribuído com escalabilidade linear. O Sparkling Water da H2O permitem-lhe combinar o rápidas e escaláveis algoritmos de machine learning da H2O com as capacidades do Spark. Com o Sparkling Water, os utilizadores podem orientar o cálculo de Scala, R e Python com a IU do Flow H2O.

O Sparkling Water da H2O fornece:

* **Serem fáceis de usar e interfaces familiares** – defina a cópia de segurança e começar a trabalhar rapidamente utilizando qualquer um dos H2O intuitiva baseada na web fluxo GUI ou ambientes, tais como o R, Python, Java, Scala, JSON e as APIs da H2O de programação.
* **Suporte de dados independente para todos os tipos de ficheiro e de base de dados comuns** – facilmente explorar e modelar dados grandes volumes de dentro do Microsoft Excel, R Studio, Tableau e muito mais. Ligar a dados a partir de origens de dados do HDFS, S3, SQL e NoSQL.
* **Mudanças irreversíveis em dados grandes e escaláveis em massa e de análise** – associações grande da H2O pode efetuar 7 vezes mais rápido do que operações de data.table de R e dimensionado linearmente para 10 mil milhões x associações de linha de 10 mil milhões.
* **Classificação de dados em tempo real** – rapidamente implementar modelos para produção com objetos do bom e velho Java (POJO), objetos com otimização de modelo de Java (MOJO), ou a API de REST da H2O.

### <a name="resource-links"></a>Hiperligações de recurso

* [Mapa de engenharia de H2O.ai](http://jira.h2o.ai/)
* [Página inicial do H2O.ai](http://www.h2o.ai/)
* [Documentação de H2O.ai](http://docs.h2o.ai/)
* [Suporte de H2O.ai](https://support.h2o.ai/)
* [Base de código do H2O.ai código-fonte aberto](https://github.com/h2oai/)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação num novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Escalões de cluster:{0}<br>tipos de cluster: Standard ou Premium
* Tipo de cluster: Spark
* Versões de cluster: 3.5 ou 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Instalar o Sparkling Water da H2O publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações disponíveis do ISV, leia [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>Inicie o Sparkling Water da H2O

1. Após a instalação, pode começar a utilizar o Sparkling Water da H2O (h2o sparklingwater) do seu cluster no portal do Azure, abrindo o Jupyter Notebooks (`https://<ClusterName>.azurehdinsight.net/jupyter`). Também pode obter para Jupyter selecionando **dashboard de clusters** a partir do seu painel de cluster no portal, em seguida, selecionamos **bloco de notas do Jupyter**. Lhe for pedido para introduzir as suas credenciais. Introduza as credenciais de Hadoop do cluster conforme especificado na criação do cluster.

2. No Jupyter, verá três pastas: H2O-PySparkling-exemplos, exemplos de PySpark e Scala exemplos. Selecione o **exemplos de PySparkling H2O** pasta.

    ![Página inicial da blocos de notas do Jupyter](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. A primeira etapa ao criar um novo bloco de notas é configurar o ambiente de Spark. Estas informações estão incluídas no **Sentiment_analysis_with_Sparkling_Water** exemplo. Ao configurar o ambiente de Spark, certifique-se de que usar o jar correto e especifique o endereço IP fornecido pela saída da primeira célula.

    ![Página inicial da blocos de notas do Jupyter](./media/hdinsight-apps-install-h2o/spark-config.png)

4. Inicie o Cluster de H2O.

    ![Iniciar o cluster](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. Depois do Cluster de H2O estiver em execução, abra H2O fluxo da **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`**.

    > [!NOTE]
    > Se não for possível abrir o fluxo da H2O, experimente limpar a cache do browser. Se ainda não é possível aceder ao mesmo, provavelmente não tem recursos suficientes no seu cluster. Experimente aumentar o número de nós de trabalho sob o **Dimensionar cluster** opção no seu painel de cluster.

    ![Dashboard de fluxo da H2O](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Selecione o **Million_Songs.flow** exemplo no menu à direita. Quando lhe for pedido com um aviso, clique em **carga bloco de notas**. Esta demonstração foi concebida para ser executado dentro de alguns minutos, com dados reais. O objetivo é prever a partir dos dados se a música foi lançada antes ou depois de 2004 com classificação binária.

    ![Selecione Million_Songs.flow](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Localizar o caminho que contém **milsongs-cls-train.csv.gz**e substitua todo o caminho com **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Localizar o caminho que contém **milsongs-cls-test.csv.gz** e substitua-o por **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Para executar todas as instruções dentro das células de bloco de notas, selecione o **executar todos** botão na barra de ferramentas.

    ![Executar todos](./media/hdinsight-apps-install-h2o/run-all.png)

10. Após alguns minutos, deverá ver um resultado semelhante ao seguinte.

    ![Saída](./media/hdinsight-apps-install-h2o/output.png)

Já está! Manipulamos inteligência artificial no Spark em questão de minutos. Agora, pode explorar mais exemplos no fluxo da H2O que demonstram os diferentes tipos de algoritmos de machine learning.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação da H2O](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada para o HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux com ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder aos clusters do HDInsight e de teste e alojamento de aplicações do HDInsight.
