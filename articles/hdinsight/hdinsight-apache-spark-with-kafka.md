---
title: Apache Spark, transmissão em fluxo com Kafka - Azure HDInsight
description: Saiba como utilizar o Spark Apache Spark para transmitir dados para dentro ou fora do Apache Kafka com DStreams. Neste exemplo, vai transmitir dados através de um bloco de notas Jupyter do Spark no HDInsight.
keywords: exemplo de kafka, zookeeper do kafka, kafka, spark, o exemplo de kafka de transmissão em fluxo de transmissão em fluxo do spark
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: jasonh
ms.openlocfilehash: 607ad43f5cae3915b964caf816bd5fb5e3302ddf
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592980"
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-on-hdinsight"></a>Apache Spark (DStream) de exemplo com o Kafka no HDInsight de transmissão em fluxo

Saiba como utilizar o Spark Apache Spark para transmitir dados para dentro ou fora do Apache Kafka no HDInsight usando DStreams. Este exemplo utiliza um bloco de notas do Jupyter que é executada no cluster do Spark.

> [!NOTE]
> Os passos neste documento criam um grupo de recursos do Azure que contém um cluster do Spark no HDInsight e um cluster do Kafka no HDInsight. Estes dois clusters estão localizados numa Rede Virtual do Azure, o que permite que o cluster do Spark comunique diretamente com o cluster do Kafka.
>
> Quando tiver concluído os passos neste documento, elimine os clusters para evitar encargos em excesso.

> [!IMPORTANT]
> Este exemplo utiliza DStreams, que é uma tecnologia de transmissão em fluxo do Spark mais antiga. Para obter um exemplo que utiliza o Spark mais recente funcionalidades de transmissão em fluxo, consulte a [Spark transmissão em fluxo estruturada com o Kafka](hdinsight-apache-kafka-spark-structured-streaming.md) documento.

## <a name="create-the-clusters"></a>Criar os clusters

O Apache Kafka no HDInsight não fornece acesso aos mediadores Kafka através da Internet pública. Tudo o que se comunica com o Kafka tem de estar na mesma rede virtual do Azure que os nós do cluster do Kafka. Neste exemplo, os clusters de Kafka e Spark estão localizados numa rede virtual do Azure. O diagrama seguinte mostra como a comunicação flui entre os clusters:

![Diagrama de clusters do Spark e Kafka numa rede virtual do Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Embora o Kafka em si está limitado à comunicação na rede virtual, outros serviços em cluster como SSH e Ambari podem ser acedidos através da internet. Para obter mais informações sobre as portas públicas disponíveis com o HDInsight, veja [Portas e URIs utilizados pelo HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Embora pode criar uma rede virtual do Azure, Kafka, e clusters do Spark manualmente, é mais fácil de usar um modelo Azure Resource Manager. Utilize os seguintes passos para implementar uma rede virtual do Azure, Kafka e do Spark clusters para a sua subscrição do Azure.

1. Utilize o botão seguinte para iniciar sessão no Azure e abrir o modelo no Portal do Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    O modelo do Azure Resource Manager está localizado em **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    > [!WARNING]
    > Para garantir a disponibilidade do Kafka no HDInsight, o cluster tem de conter pelo menos três nós de trabalho. Este modelo cria um cluster do Kafka que contém três nós de trabalho.

    Este modelo cria um cluster do HDInsight 3.6 para Kafka e Spark.

2. Utilize as seguintes informações para preencher as entradas a **implementação personalizada** secção:
   
    ![Implantação personalizada do HDInsight](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Grupo de recursos**: Crie um grupo ou selecione um existente. Este grupo contém o cluster do HDInsight.

    * **Localização**: selecione uma localização geograficamente próximo de.

    * **Nome do Cluster de base**: este valor é utilizado como o nome de base para o Spark e clusters do Kafka. Por exemplo, introduzir **hdi** cria um cluster do Spark com o nome __spark hdi__ e um cluster de Kafka com o nome **kafka-hdi**.

    * **Nome de utilizador de início de sessão do cluster**: O nome de utilizador de administrador para os clusters do Spark e Kafka.

    * **Palavra-passe de início de sessão do cluster**: A palavra-passe de utilizador do administrador para os clusters do Spark e Kafka.

    * **Nome de utilizador SSH**: O utilizador SSH para criar para os clusters do Spark e Kafka.

    * **Palavra-passe SSH**: A palavra-passe do utilizador SSH para os clusters do Spark e Kafka.

3. Leia os **Termos e Condições** e selecione **Aceito os temos e as condições apresentados acima**.

4. Por fim, marque **Afixar ao dashboard** e, em seguida, selecione **Compra**. Demora cerca de 20 minutos para criar os clusters.

Assim que os recursos foram criados, é apresentada uma página de resumo.

![Resumo para a vnet e a clusters do grupo de recursos](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Tenha em atenção que os nomes dos HDInsight clusters estão **spark NOME_DE_BASE** e **kafka NOME_DE_BASE**, onde NOME_DE_BASE é o nome que indicou para o modelo. Utilize esses nomes em passos posteriores ao estabelecer ligação com os clusters.

## <a name="use-the-notebooks"></a>Utilize os blocos de notas

O código de exemplo descrito neste documento está disponível em [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Para concluir este exemplo, siga os passos a `README.md`.

## <a name="delete-the-cluster"></a>Eliminar o cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Uma vez que os passos neste documento criam ambos os clusters no mesmo grupo de recursos do Azure, pode eliminar o grupo de recursos no portal do Azure. A eliminação do grupo remove todos os recursos criados através dos seguintes deste documento, a rede Virtual do Azure e conta de armazenamento utilizada pelos clusters.

## <a name="next-steps"></a>Passos Seguintes

Neste exemplo, aprendeu a utilizar o Spark para ler e escrever para o Kafka. Utilize as seguintes ligações para descobrir outras maneiras de trabalhar com o Kafka:

* [Introdução ao Apache Kafka no HDInsight](kafka/apache-kafka-get-started.md)
* [Utilizar MirrorMaker para criar uma réplica de Kafka no HDInsight](kafka/apache-kafka-mirroring.md)
* [Utilizar o Apache Storm com Kafka no HDInsight](hdinsight-apache-storm-with-kafka.md)

