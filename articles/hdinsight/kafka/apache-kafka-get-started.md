---
title: "Começar com o Apache Kafka - Azure HDInsight | Microsoft Docs"
description: "Saiba como criar um cluster do Apache Kafka no Azure HDInsight. Saiba como criar tópicos, subscritores e consumidores."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/18/2018
ms.author: larryfr
ms.openlocfilehash: 639adb2fdc5a7d76c11397b5027199626a0a4016
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="start-with-apache-kafka-on-hdinsight"></a>Introdução ao Apache Kafka no HDInsight

Saiba como criar e utilizar um cluster do [Apache Kafka](https://kafka.apache.org) no Azure HDInsight. O Kafka é uma plataforma open source de transmissão em fluxo distribuída, disponível com o HDInsight. É frequentemente utilizado como mediador de mensagens, uma vez que fornece funcionalidades semelhantes a uma fila de mensagens de publicação-subscrição. Kafka, muitas vezes, é utilizado com o Apache Spark e o Apache Storm.

> [!NOTE]
> Atualmente, existem duas versões do Kafka disponíveis com o HDInsight: 0.9.0 (HDInsight 3.4) e 0.10.0 (HDInsight 3.5 e 3.6). Os passos neste documento partem do princípio de que está a utilizar o Kafka no HDInsight 3.6.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Criar um cluster Kafka

Utilize os seguintes passos para criar um Kafka num cluster HDInsight:

1. No [portal do Azure](https://portal.azure.com), selecione **+ Criar um recurso**, **Dados + Análise** e, em seguida, selecione **HDInsight**.
   
    ![Criar um cluster HDInsight](./media/apache-kafka-get-started/create-hdinsight.png)

2. Em **Informações Básicas**, introduza as seguintes informações:

    * **Nome do Cluster**: o nome do cluster HDInsight.
    * **Subscrição**: selecione a subscrição que pretende utilizar.
    * **Nome de utilizador de início de sessão do cluster** e **Palavra-passe de início de sessão do cluster**: o início de sessão ao aceder ao cluster através de HTTPS. Utilize estas credenciais para aceder aos serviços, como a IU Web do Ambari ou a API REST.
    * **Nome de utilizador do Secure Shell (SSH)**: o início de sessão utilizado ao aceder ao cluster através de SSH. Por predefinição, a palavra-passe é igual à palavra-passe de início de sessão do cluster.
    * **Grupo de Recursos**: o grupo de recursos em que o cluster é criado.
    * **Localização**: a região do Azure em que o cluster é criado.

        > [!IMPORTANT]
        > Para obter uma elevada disponibilidade de dados, recomendamos que selecione uma localização (região) que contenha __três domínios de falha__. Para obter mais informações, consulte a secção [Elevada disponibilidade de dados](#data-high-availability).
   
 ![Selecionar subscrição](./media/apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Selecione **Tipo de cluster** e, em seguida, defina os seguintes valores em **Configuração de cluster**:
   
    * **Tipo de Cluster**: Kafka
    * **Versão**: Kafka 0.10.0 (HDI 3.6)

    Por fim, utilize o botão **Selecionar** para guardar as definições.
     
 ![Selecionar tipo de cluster](./media/apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. Depois de selecionar o tipo de cluster, utilize o botão __Selecionar__ para definir o tipo de cluster. Em seguida, utilize o botão __Seguinte__ para concluir a configuração básica.

5. Em **Armazenamento**, selecione ou crie uma Conta de armazenamento. Para seguir os passos neste documento, deixe os outros campos com os valores predefinidos. Utilize o botão __Seguinte__ para guardar a configuração do armazenamento.

    ![Configurar as definições de conta de armazenamento do HDInsight](./media/apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Em __Aplicações (opcional)__, selecione __Seguinte__ para continuar. Não são necessárias aplicações para este exemplo.

7. Em __Tamanho do cluster__ , selecione __Seguinte__ para continuar.

    > [!WARNING]
    > Para garantir a disponibilidade do Kafka no HDInsight, o cluster tem de conter pelo menos três nós de trabalho. Para obter mais informações, consulte a secção [Elevada disponibilidade de dados](#data-high-availability).

    ![Definir o tamanho do cluster do Kafka](./media/apache-kafka-get-started/kafka-cluster-size.png)

    > [!IMPORTANT]
    > A entrada **discos por nó de trabalho** controla a escalabilidade do Kafka no HDInsight. O Kafka no HDInsight utiliza o disco local das máquinas virtuais no cluster. O Kafka recebe um fluxo intensivo de dados de E/S, pelo que o [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) é utilizado para proporcionar um elevado débito de transferência e fornecer uma maior capacidade de armazenamento por nó. O tipo de disco gerido pode ser __Standard__ (HDD) ou __Premium__ (SSD). Os discos Premium são utilizados com as VMs das séries DS e GS. Todos os outros tipos de VM utilizam discos Standard.

8. Em __Definições avançadas__, selecione __Seguinte__ para continuar.

9. Em **Resumo**, reveja a configuração do cluster. Utilize as ligações __Editar__ para alterar quaisquer definições que estejam incorretas. Por fim, utilize o botão the__Create__ para criar o cluster.
   
    ![Resumo da configuração do cluster](./media/apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > A criação do cluster pode demorar até 20 minutos.

## <a name="connect-to-the-cluster"></a>Ligar ao cluster

> [!IMPORTANT]
> Quando realizar os passos seguintes, tem de utilizar um cliente SSH. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

A partir do seu cliente, utilize SSH para ligar ao cluster:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Substitua **SSHUSER** pelo nome de utilizador de SSH que indicou durante a criação do cluster. Substitua **CLUSTERNAME** pelo nome do cluster.

Quando lhe for pedido, introduza a palavra-passe que utilizou para a conta SSH.

Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="getkafkainfo"></a>Obter as informações dos anfitriões Zookeeper e Mediador

Ao trabalhar com o Kafka, tem de saber dois valores de anfitrião: dos anfitriões *Zookeeper* e dos anfitriões do *Mediador*. Estes anfitriões são utilizados com a API Kafka e com muitos dos utilitários que são enviados com o Kafka.

Utilize os seguintes passos para criar variáveis de ambiente que contêm as informações do anfitrião. Estas variáveis de ambiente são utilizadas nos passos deste documento.

1. A partir de uma ligação SSH ao cluster, utilize o comando seguinte para instalar o utilitário `jq`. Este utilitário é utilizado para analisar documentos JSON e é útil para obter as informações do anfitrião mediador:
   
    ```bash
    sudo apt -y install jq
    ```

2. Para definir as variáveis de ambiente com informações obtidas a partir do Ambari, utilize os seguintes comandos:

    ```bash
    CLUSTERNAME='your cluster name'
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Defina `CLUSTERNAME=` com o nome do cluster do Kafka. Quando lhe for pedido, introduza a palavra-passe para o início de sessão (administrador) do cluster.

    O texto seguinte é um exemplo dos conteúdos dos `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    O texto seguinte é um exemplo dos conteúdos dos `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > O comando `cut` é utilizado para limitar a lista de anfitriões a duas entradas de anfitrião. Não precisa de fornecer a lista completa de anfitriões quando cria um consumidor ou produtor do Kafka.
   
    > [!WARNING]
    > As informações devolvidas nesta sessão poderão não ser sempre precisas. Se dimensionar o cluster, novos mediadores serão adicionados ou removidos. Se ocorrer uma falha e um nó for substituído, o nome do anfitrião do nó poderá mudar.
    >
    > Deve obter as informações de anfitriões Zookeeper e do mediador pouco tempo antes de as utilizar, para certificar-se de que as informações são válidas.

## <a name="create-a-topic"></a>Criar um tópico

O Kafka armazena fluxos de dados em categorias chamadas *tópicos*. A partir de uma ligação SSH a um nó principal do cluster, utilize um script fornecido com Kafka para criar um tópico:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Este comando liga ao Zookeeper, utilizando as informações de anfitrião armazenadas nos `$KAFKAZKHOSTS` e, em seguida, cria um tópico Kafka com o nome **teste**. Pode verificar se o tópico foi criado, utilizando o seguinte script para tópicos de lista:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

O resultado deste comando apresenta uma lista de tópicos Kafka, que contém o tópico **teste**.

## <a name="produce-and-consume-records"></a>Produzir e consumir registos

O Kafka armazena *registos* nos tópicos. Os registos são produzidos por *produtores* e consumidos por *consumidores*. Os produtores produzem registos para *mediadores* Kafka. Cada nó de trabalho no cluster HDInsight é um mediador Kafka.

Utilize os seguintes passos para armazenar registos no tópico de teste que criou anteriormente e, em seguida, leia-os utilizando um consumidor:

1. A partir da sessão SSH, utilize um script fornecido com Kafka para escrever registos no tópico:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Não é reencaminhado para o pedido após este comando. Em vez disso, escreva algumas mensagens de texto e, em seguida, utilize **Ctrl + C** para deixar de enviar para o tópico. Cada linha é enviada como um registo separado.

2. Utilize um script fornecido com Kafka para ler registos do tópico:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Este comando obtém os registos do tópico e apresenta-os. A utilização de `--from-beginning` indica ao consumidor para iniciar a partir do início da transmissão em fluxo, para que todos os registos sejam obtidos.

    > [!NOTE]
    > Se estiver a utilizar uma versão antiga do Kafka, poderá ter de substituir `--bootstrap-server $KAFKABROKERS` por `--zookeeper $KAFKAZKHOSTS`.

3. Utilize __Ctrl + C__ para parar o consumidor.

Também podem criar programaticamente produtores e consumidores. Para obter um exemplo de utilização desta API, veja o documento [Produtor Kafka e a API de Consumidor com o HDInsight](apache-kafka-producer-consumer-api.md).

## <a name="data-high-availability"></a>Elevada disponibilidade de dados

Cada região do Azure (localização) fornece _domínios de falha_. Um domínio de falha é um agrupamento lógico de hardware subjacente num centro de dados do Azure. Cada domínio de falha partilha um comutador de rede e uma fonte de alimentação. As máquinas virtuais e os discos geridos que implementam os nós num cluster HDInsight são distribuídos por esses domínios de falha. Esta arquitetura limita o possível impacto de falhas físicas de hardware.

Para obter informações sobre o número de domínios de falha numa região, consulte o documento [Disponibilidade das máquinas virtuais Linux](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).

> [!IMPORTANT]
> É recomendável utilizar uma região do Azure que contenha três domínios de falha e um fator de replicação de 3.

Se tiver de utilizar uma região que contenha apenas dois domínios de falha, utilize um fator de replicação de 4 para distribuir as réplicas uniformemente entre os dois domínios de falha.

### <a name="kafka-and-fault-domains"></a>Kafka e os domínios de falha

O Kafka não está ciente dos domínios de falha. Durante a criação de réplicas de partição para tópicos, poderá não distribuir as réplicas corretamente para fins de elevada disponibilidade. Para garantir a elevada disponibilidade, utilize a [ferramenta de rebalanceamento de partições de Kafka](https://github.com/hdinsight/hdinsight-kafka-tools). Esta ferramenta deve ser executada numa sessão SSH para o nó principal do cluster do Kafka.

Para garantir a maior disponibilidade dos seus dados do Kafka, deve reequilibrar as réplicas de partições do seu tópico nos seguintes momentos:

* Quando é criado um novo tópico ou partição

* Quando expandir um cluster

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
