---
title: Começar com o Apache Kafka - Azure HDInsight | Microsoft Docs
description: Saiba como criar um cluster do Apache Kafka no Azure HDInsight. Saiba como criar tópicos, subscritores e consumidores.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/20/2018
ms.author: larryfr
ms.openlocfilehash: 27e6472480dac104de799ebf0e7579a7987f6c4c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="start-with-apache-kafka-on-hdinsight"></a>Introdução ao Apache Kafka no HDInsight

Saiba como criar e utilizar um cluster do [Apache Kafka](https://kafka.apache.org) no Azure HDInsight. O Kafka é uma plataforma open source de transmissão em fluxo distribuída, disponível com o HDInsight. É frequentemente utilizado como mediador de mensagens, uma vez que fornece funcionalidades semelhantes a uma fila de mensagens de publicação-subscrição. Kafka é, muitas vezes, utilizado com o Apache Spark e o Apache Storm para mensagens, controlo de atividade, agregação de fluxo ou transformação de dados.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Criar um cluster Kafka

Utilize os seguintes passos para criar um Kafka no cluster HDInsight:

1. No [portal do Azure](https://portal.azure.com), selecione **+ Criar um recurso**, **Dados + Análise** e, em seguida, selecione **HDInsight**.
   
    ![Criar um cluster HDInsight](./media/apache-kafka-get-started/create-hdinsight.png)

2. Em **Informações Básicas**, introduza as seguintes informações:

    * **Nome do Cluster**: o nome do cluster HDInsight. Este nome tem de ser exclusivo.
    * **Subscrição**: selecione a subscrição que pretende utilizar.
    * **Tipo de Cluster**: selecione esta entrada e, em seguida, defina os seguintes valores em **Configuração de cluster**:

        * **Tipo de Cluster**: Kafka
        * **Versão**: Kafka 0.10.0 (HDI 3.6)

        Utilize o botão **Selecionar** para guardar as definições do tipo de cluster.

        ![Selecionar tipo de cluster](./media/apache-kafka-get-started/set-hdinsight-cluster-type.png)

    * **Nome de utilizador de início de sessão do cluster** e **Palavra-passe de início de sessão do cluster**: o início de sessão ao aceder ao cluster através de HTTPS. Utilize estas credenciais para aceder aos serviços, como a IU Web do Ambari ou a API REST.
    * **Nome de utilizador do Secure Shell (SSH)**: o início de sessão utilizado ao aceder ao cluster através de SSH. Por predefinição, a palavra-passe é igual à palavra-passe de início de sessão do cluster.
    * **Grupo de Recursos**: o grupo de recursos em que o cluster é criado.
    * **Localização**: a região do Azure em que o cluster é criado.

        > [!IMPORTANT]
        > Para obter uma elevada disponibilidade de dados, recomendamos que selecione uma localização (região) que contenha __três domínios de falha__. Para obter mais informações, consulte a secção [Elevada disponibilidade de dados](#data-high-availability).
   
 ![Selecionar subscrição](./media/apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Utilize o botão __Seguinte__ para concluir a configuração básica.

4. Em **Armazenamento**, selecione ou crie uma Conta de armazenamento. Para seguir os passos neste documento, deixe os outros campos com os valores predefinidos. Utilize o botão __Seguinte__ para guardar a configuração do armazenamento.

    ![Configurar as definições de conta de armazenamento do HDInsight](./media/apache-kafka-get-started/set-hdinsight-storage-account.png)

5. Em __Aplicações (opcional)__, selecione __Seguinte__ para continuar. Não são necessárias aplicações para este exemplo.

6. Em __Tamanho do cluster__ , selecione __Seguinte__ para continuar.

    > [!WARNING]
    > Para garantir a disponibilidade do Kafka no HDInsight, o cluster tem de conter pelo menos três nós de trabalho. Para obter mais informações, consulte a secção [Elevada disponibilidade de dados](#data-high-availability).

    ![Definir o tamanho do cluster do Kafka](./media/apache-kafka-get-started/kafka-cluster-size.png)

    > [!IMPORTANT]
    > A entrada **discos por nó de trabalho** configura a escalabilidade do Kafka no HDInsight. O Kafka no HDInsight utiliza o disco local das máquinas virtuais no cluster. O Kafka recebe um fluxo intensivo de dados de E/S, pelo que o [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) é utilizado para proporcionar um elevado débito de transferência e fornecer uma maior capacidade de armazenamento por nó. O tipo de disco gerido pode ser __Standard__ (HDD) ou __Premium__ (SSD). Os discos Premium são utilizados com as VMs das séries DS e GS. Todos os outros tipos de VM utilizam discos Standard.

7. Em __Definições avançadas__, selecione __Seguinte__ para continuar.

8. Em **Resumo**, reveja a configuração do cluster. Utilize as ligações __Editar__ para alterar quaisquer definições que estejam incorretas. Por fim, utilize o botão the__Create__ para criar o cluster.
   
    ![Resumo da configuração do cluster](./media/apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > A criação do cluster pode demorar até 20 minutos.

## <a name="connect-to-the-cluster"></a>Ligar ao cluster

> [!IMPORTANT]
> Quando realizar os passos seguintes, tem de utilizar um cliente SSH. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Para ligar ao cluster através de SSH, tem de indicar o nome da conta de utilizador SSH e o nome do cluster. No exemplo seguinte, substitua `sshuser` e `clustername` pelo nome do cluster e da conta:

```ssh sshuser@clustername-ssh.azurehdinsight.net```

Quando lhe for pedido, introduza a palavra-passe que utilizou para a conta SSH.

Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Obter as informações dos anfitriões Zookeeper e Mediador

Ao trabalhar com o Kafka, tem de conhecer os anfitriões *Zookeeper* e *Mediador*. Estes anfitriões são utilizados com a API Kafka e com muitos dos utilitários que são enviados com o Kafka.

Utilize os seguintes passos para criar variáveis de ambiente que contêm as informações do anfitrião:

1. A partir de uma ligação SSH ao cluster, utilize o comando seguinte para instalar o utilitário `jq`. Este utilitário é utilizado para analisar documentos JSON e é útil para obter as informações do anfitrião mediador:
   
    ```bash
    sudo apt -y install jq
    ```

2. Para definir uma variável de ambiente para o nome do cluster, utilize o comando seguinte:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

3. Para definir uma variável de ambiente com as informações do anfitrião Zookeeper, utilize o comando seguinte:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Quando lhe for pedido, introduza a palavra-passe da conta do início de sessão do cluster (administrador).

4. Para verificar se a variável de ambiente está definida corretamente, utilize o comando seguinte:

    ```bash
     echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    ```

    Este comando devolve informações semelhantes ao texto seguinte:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

5. Para definir uma variável de ambiente com as informações do anfitrião Zookeeper e do mediador, utilize o comando seguinte:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

    Quando lhe for pedido, introduza a palavra-passe da conta do início de sessão do cluster (administrador).

6. Para verificar se a variável de ambiente está definida corretamente, utilize o comando seguinte:

    ```bash   
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    Este comando devolve informações semelhantes ao texto seguinte:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`
   
> [!WARNING]
> As informações devolvidas nesta sessão poderão não ser sempre precisas. Ao dimensionar o cluster, são adicionados ou removidos novos mediadores. Se ocorrer uma falha e um nó for substituído, o nome do anfitrião do nó poderá mudar.
>
> Deve obter as informações dos anfitriões Zookeeper e do mediador pouco tempo antes de as utilizar para garantir que as informações são válidas.

## <a name="create-a-topic"></a>Criar um tópico

O Kafka armazena fluxos de dados em categorias chamadas *tópicos*. A partir de uma ligação SSH a um nó principal do cluster, utilize um script fornecido com Kafka para criar um tópico:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Este comando liga ao Zookeeper com as informações do anfitrião armazenadas em `$KAFKAZKHOSTS`. Em seguida, cria um tópico do Kafka denominado **teste**. Pode verificar se o tópico foi criado, utilizando o seguinte script para tópicos de lista:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

O resultado deste comando apresenta uma lista de tópicos do Kafka no cluster.

## <a name="produce-and-consume-records"></a>Produzir e consumir registos

O Kafka armazena *registos* nos tópicos. Os registos são produzidos por *produtores* e consumidos por *consumidores*. Os produtores produzem registos para *mediadores* Kafka. Cada nó de trabalho no cluster HDInsight é um mediador Kafka.

Utilize os seguintes passos para armazenar os registos no tópico de teste criado anteriormente e, em seguida, leia-os através de um consumidor:

1. A partir da sessão SSH, utilize um script fornecido com Kafka para escrever registos no tópico:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Após este comando, chega a uma linha vazia.

2. Introduza uma mensagem de texto na linha vazia e carregue em Enter. Introduza algumas mensagens desta forma e, em seguida, utilize **Ctrl + C** para regressar à linha de comandos normal. Cada linha é enviada como um registo separado para o tópico do Kafka.

3. Utilize um script fornecido com Kafka para ler registos do tópico:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Este comando obtém os registos do tópico e apresenta-os. A utilização de `--from-beginning` indica ao consumidor para iniciar a partir do início da transmissão em fluxo, para que todos os registos sejam obtidos.

    > [!NOTE]
    > Se estiver a utilizar uma versão antiga do Kafka, substitua `--bootstrap-server $KAFKABROKERS` por `--zookeeper $KAFKAZKHOSTS`.

4. Utilize __Ctrl + C__ para parar o consumidor.

Também podem criar programaticamente produtores e consumidores. Para obter um exemplo de utilização desta API, veja o documento [Produtor Kafka e a API de Consumidor com o HDInsight](apache-kafka-producer-consumer-api.md).

## <a name="data-high-availability"></a>Elevada disponibilidade de dados

Cada região do Azure (localização) fornece _domínios de falha_. Um domínio de falha é um agrupamento lógico de hardware subjacente num centro de dados do Azure. Cada domínio de falha partilha um comutador de rede e uma fonte de alimentação. As máquinas virtuais e os discos geridos que implementam os nós num cluster HDInsight são distribuídos por esses domínios de falha. Esta arquitetura limita o possível impacto de falhas físicas de hardware.

Para obter informações sobre o número de domínios de falha numa região, consulte o documento [Disponibilidade das máquinas virtuais Linux](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

> [!IMPORTANT]
> Se possível, utilize uma região do Azure que contenha três domínios de falha e crie tópicos com um fator de replicação de 3.

Se utilizar uma região que contenha apenas dois domínios de falha, utilize um fator de replicação de 4 para distribuir as réplicas uniformemente entre os dois domínios de falha.

### <a name="kafka-and-fault-domains"></a>Kafka e os domínios de falha

O Kafka não está ciente dos domínios de falha. Durante a criação de réplicas de partição para tópicos, poderá não distribuir as réplicas corretamente para fins de elevada disponibilidade. Para garantir a elevada disponibilidade, utilize a [ferramenta de rebalanceamento de partições de Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Esta ferramenta deve ser executada numa sessão SSH para o nó principal do cluster do Kafka.

Para garantir a maior disponibilidade dos seus dados do Kafka, deve reequilibrar as réplicas de partições do tópico ao:

* Criar um novo tópico ou partição

* Aumentar verticalmente um cluster

## <a name="delete-the-cluster"></a>Eliminar o cluster

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Resolução de problemas

Caso se depare com problemas com a criação de clusters do HDInsight, veja [aceder aos requisitos de controlo](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Passos seguintes

Neste documento, aprendeu as noções básicas para trabalhar com o Apache Kafka no HDInsight. Utilize o seguinte para obter mais informações sobre como trabalhar com o Kafka:

* [Analisar os registos do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters do Kafka](apache-kafka-mirroring.md)
* [Produtor Kafka e API de Consumidor com o HDInsight](apache-kafka-producer-consumer-api.md)
* [API de Fluxos Kafka com o HDInsight](apache-kafka-streams-api.md)
* [Utilizar a transmissão em fluxo de Apache Spark (DStream) com o Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Utilizar a Transmissão em Fluxo Estruturada de Apache Spark com o Kafka no HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md)
* [Utilizar a Transmissão em Fluxo Estruturada do Apache Spark para mover dados do Kafka no HDInsight para o Cosmos DB](../apache-kafka-spark-structured-streaming-cosmosdb.md)
* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Ligar ao Kafka através de uma Rede Virtual do Azure](apache-kafka-connect-vpn-gateway.md)
* [Utilizar o Kafka com o Azure Container Service](apache-kafka-azure-container-services.md)
* [Utilizar o Kafka com as Aplicações de Funções do Azure](apache-kafka-azure-functions.md)
