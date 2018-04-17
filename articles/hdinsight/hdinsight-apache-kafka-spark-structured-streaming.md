---
title: Transmissão em Fluxo Estruturada do Apache Spark com o Kafka - Azure HDInsight | Microsoft Docs
description: Saiba como utilizar a transmissão em fluxo (DStream) do Apache Spark para introduzir ou extrair dados do Apache Kafka. Neste exemplo, vai transmitir dados através de um bloco de notas Jupyter do Spark no HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2018
ms.author: larryfr
ms.openlocfilehash: 49c13bbea537d7de60ecf509bc28675191c0b34d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Utilizar a Transmissão em Fluxo Estruturada do Spark com o Kafka no HDInsight

Saiba como utilizar a Transmissão em Fluxo Estruturada do Spark para ler os dados do Apache Kafka no Azure HDInsight.

A transmissão em fluxo estruturada do Spark é um motor de processamento de fluxos incorporado no SQL do Spark. Permite-lhe expressar computações de transmissão em fluxo, tal como a computação em lotes o faz em dados estáticos. Para obter mais informações sobre a Transmissão em Fluxo Estruturada, veja o [Structured Streaming Programming Guide [Alpha] (Guia de Programação da Transmissão em Fluxo Estruturada [Alfa])](http://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) em Apache.org.

> [!IMPORTANT]
> Este exemplo utiliza o Spark 2.2 no HDInsight 3.6.
>
> Os passos neste documento criam um grupo de recursos do Azure que contém um cluster do Spark no HDInsight e um cluster do Kafka no HDInsight. Estes dois clusters estão localizados numa Rede Virtual do Azure, o que permite que o cluster do Spark comunique diretamente com o cluster do Kafka.
>
> Quando tiver concluído os passos neste documento, elimine os clusters para evitar encargos em excesso.

## <a name="create-the-clusters"></a>Criar os clusters

O Apache Kafka no HDInsight não fornece acesso aos mediadores Kafka através da Internet pública. Tudo o que utilize Kafka tem de estar na mesma rede virtual do Azure. Neste tutorial, os clusters do Kafka e do Spark estão localizados na mesma rede virtual do Azure. 

O diagrama seguinte mostra como a comunicação flui entre o Spark e o Kafka:

![Diagrama de clusters do Spark e Kafka numa rede virtual do Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> O serviço Kafka está limitado à comunicação na rede virtual. Outros serviços em cluster, como SSH e Ambari, podem ser acedidos através da Internet. Para obter mais informações sobre as portas públicas disponíveis com o HDInsight, veja [Portas e URIs utilizados pelo HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Para sua comodidade, os passos seguintes utilizam um modelo do Azure Resource Manager para criar clusters do Kafka e Spark dentro de uma rede virtual.

1. Utilize o botão seguinte para iniciar sessão no Azure e abrir o modelo no Portal do Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    O modelo do Azure Resource Manager está localizado em **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**.

    Este modelo cria os seguintes recursos:

    * Um cluster do Kafka no HDInsight 3.6.
    * Um cluster do Spark 2.2.0 no HDInsight 3.6.
    * Uma Rede Virtual do Azure, que contém os clusters do HDInsight.

    > [!IMPORTANT]
    > O bloco de notas de transmissão em fluxo estruturada utilizado neste exemplo requer o Spark no HDInsight 3.6. Se utilizar uma versão anterior do Spark no HDInsight, irá receber mensagens de erro ao utilizar o bloco de notas.

2. Utilize as seguintes informações para preencher as entradas da secção **Modelo personalizado**:

    | Definição | Valor |
    | --- | --- |
    | Subscrição | A sua subscrição do Azure |
    | Grupo de recursos | O grupo de recursos que contém os recursos. |
    | Localização | A região do Azure na qual os recursos são criados. |
    | Nome de Cluster do Spark | O nome do cluster do Spark. |
    | Nome do Cluster do Kafka | O nome do cluster do Kafka. |
    | Nome de Utilizador de Início de Sessão do Cluster | O nome de utilizador administrador para os clusters. |
    | Palavra-passe de Início de Sessão do Cluster | A palavra-passe de utilizador administrador para os clusters. |
    | Nome de Utilizador SSH | O nome de utilizador SSH para os clusters. |
    | Palavra-passe do SSH | A palavra-passe do utilizador SSH. |
   
    ![Captura de ecrã do modelo personalizado](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

4. Por fim, marque **Afixar ao dashboard** e, em seguida, selecione **Compra**. 

> [!NOTE]
> A criação dos clusters pode demorar até 20 minutos.

## <a name="get-the-notebook"></a>Obter o bloco de notas

O código de exemplo descrito neste documento está disponível em [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>Carregar os blocos de notas

Para carregar o bloco de notas do projeto para o cluster Spark no HDInsight, siga os passos seguintes:

1. No seu browser, ligue ao bloco de notas Jupyter no cluster do Spark. No seguinte URL, substitua `CLUSTERNAME` pelo nome do seu cluster do __Spark__:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Quando lhe for pedido, introduza o início de sessão do cluster (admin) e a palavra-passe utilizada quando criou o cluster.

2. No lado direito superior da página, utilize o botão __Carregar__ para carregar o ficheiro __spark-structured-streaming-kafka.ipynb__ para o cluster. Selecione __Abrir__ para iniciar o carregamento.

    ![Utilizar o botão de carregamento para selecionar e carregar um bloco de notas](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Selecionar o ficheiro KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Localize a entrada __spark-structured-streaming-kafka.ipynb__ na lista de blocos de notas e selecione o botão __Carregar__ junto à mesma.

    ![Para carregar o bloco de notas, utilizar o botão de carregamento para a entrada KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>Utilizar o bloco de notas

Depois de os ficheiros serem carregados, selecione a entrada __spark-structured-streaming-kafka.ipynb__ para abrir o bloco de notas. Para saber como utilizar a transmissão em fluxo estruturada do Spark com o Kafka no HDInsight, siga as instruções no bloco de notas.

## <a name="clean-up-resources"></a>Limpar recursos

Para limpar os recursos criados por este tutorial, pode eliminar o grupo de recursos. Ao eliminar o grupo de recursos também elimina o cluster do HDInsight associado e quaisquer outros recursos associados ao grupo de recursos.

Para remover o grupo de recursos através do Portal do Azure:

1. No Portal do Azure, expanda o menu no lado esquerdo para abrir o menu de serviços e, em seguida, escolha __Grupos de Recursos__, para apresentar a lista dos seus grupos de recursos.
2. Encontre o grupo de recursos a eliminar e, em seguida, clique com o botão direito do rato em __Mais__ (...) no lado direito da lista.
3. Selecione __Eliminar grupo de recursos__ e, em seguida, confirme.

> [!WARNING]
> A faturação do cluster do HDInsight tem início quando o cluster é criado e termina quando é eliminado. A faturação é rateada por minuto, pelo que deve sempre eliminar o cluster quando deixar de ser utilizado.
> 
> Eliminar um cluster do Kafka no HDInsight elimina quaisquer dados armazenados no Kafka.

## <a name="next-steps"></a>Passos seguintes

Agora que aprendeu a utilizar a Transmissão em Fluxo Estruturada do Spark, consulte os seguintes documentos para obter mais informações sobre como trabalhar com Spark e Kafka:

* [Como utilizar a transmissão em fluxo (DStream) do Spark com o Kafka](hdinsight-apache-spark-with-kafka.md).
* [Começar pelo Bloco de Notas Jupyter e o Spark no HDInsight](spark/apache-spark-jupyter-spark-sql.md)