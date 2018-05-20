---
title: Utilizar o Apache Kafka no HDInsight com o IoT Hub do Azure | Microsoft Docs
description: Saiba como utilizar o Apache Kafka no HDInsight com o IoT Hub do Azure. O projeto de Kafka ligar IoT Hub do Azure fornece um conector de origem e dependente para Kafka. O conector de origem possa ler dados a partir do IoT Hub e o conector do sink escreve ao IoT Hub.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/15/2018
ms.author: larryfr
ms.openlocfilehash: 33fdb5b099efc40fec94a860b21cda75ced44fe9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="use-apache-kafka-on-hdinsight-with-azure-iot-hub"></a>Utilizar o Apache Kafka no HDInsight com o IoT Hub do Azure

Saiba como utilizar o [Kafka ligar IoT Hub do Azure](https://github.com/Azure/toketi-kafka-connect-iothub) conector para mover dados entre Apache Kafka no HDInsight e IoT Hub do Azure. Neste documento, irá aprender a executar o conector do IoT Hub a partir de um nó de extremidade no cluster.

A API de ligar Kafka permite-lhe implementar os conectores que continuamente extrair dados para Kafka ou emitir dados a partir de Kafka para outro sistema. O [Kafka ligar IoT Hub do Azure](https://github.com/Azure/toketi-kafka-connect-iothub) é um conector que obtém dados a partir do IoT Hub do Azure para Kafka. -Também pode enviar dados de Kafka ao IoT Hub. 

Quando extrair do IoT Hub, utilize um __origem__ conector. Quando efetuar instalações push ao IoT Hub, utilize um __sink__ conector. O conector do IoT Hub fornece conectores de origem e dependente.

O diagrama seguinte mostra o fluxo de dados entre o IoT Hub do Azure e Kafka no HDInsight ao utilizar o conector.

![Imagem que mostra os dados que fluem do IoT Hub Kafka através do conector](./media/apache-kafka-connector-iot-hub/iot-hub-kafka-connector-hdinsight.png)

Para obter mais informações sobre a API de ligar, consulte [ https://kafka.apache.org/documentation/#connect ](https://kafka.apache.org/documentation/#connect).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* Um Kafka num cluster do HDInsight. Para obter mais informações, veja o documento [Início rápido do Kafka no HDInsight](apache-kafka-get-started.md).

* Um nó de extremidade no Kafka cluster. Para obter mais informações, consulte o [utilizar nós de limite com o HDInsight](../hdinsight-apps-use-edge-node.md) documento.

* Um IoT Hub do Azure. Para este tutorial, posso Recomendamos a [simulador de online Raspberry Pi estabelecer ligação ao IoT Hub do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started) documento.

* Um cliente SSH. Os passos neste documento utilizam SSH para se ligar ao cluster. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="install-the-connector"></a>Instalar o conector

1. Transferir a versão mais recente de ligar o Kafka para o Azure IoT Hub do [ https://github.com/Azure/toketi-kafka-connect-iothub/releases/ ](https://github.com/Azure/toketi-kafka-connect-iothub/releases).

2. Para carregar o ficheiro. JAR ao nó de extremidade da sua Kafka num cluster do HDInsight, utilize o seguinte comando:

    > [!NOTE]
    > Substitua `sshuser` com a conta de utilizador do SSH para o seu cluster HDInsight. Substitua `new-edgenode` com o nome de nó de extremidade. Substitua `clustername` com o nome do cluster. Para obter mais informações sobre o ponto final SSH para o nó de extremidade, consulte o [utilizado nós de limite com o HDInsight](../hdinsight-apps-use-edge-node.md#access-an-edge-node) documento.

    ```bash
    scp kafka-connect-iothub-assembly*.jar sshuser@new-edgenode.clustername-ssh.azurehdinsight.net:
    ```

3. Uma vez concluída a cópia de ficheiros, ligar ao nó de extremidade com o SSH:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```

    Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

4. Para instalar o conector para o Kafka `libs` diretório, utilize o seguinte comando:

    ```bash
    sudo mv kafka-connect-iothub-assembly*.jar /usr/hdp/current/kafka-broker/libs/
    ```

> [!TIP]
> Caso se depare com problemas com o resto dos passos neste documento ao utilizar um ficheiro. JAR pré-criadas, tente criar o pacote de origem.
>
> Para criar o conector, tem de ter um ambiente de desenvolvimento Scala com o [Scala criar ferramenta](http://www.scala-sbt.org/).
>
> 1. Transfira a origem para o conector do [ https://github.com/Azure/toketi-kafka-connect-iothub/ ](https://github.com/Azure/toketi-kafka-connect-iothub/) ao seu ambiente de desenvolvimento.
>
> 2. A partir de uma linha de comandos no diretório do projeto, utilize o seguinte comando para criar e o projeto do pacote:
>
>    ```bash
>    sbt assembly
>    ```
>
>    Este comando cria um ficheiro denominado `kafka-connect-iothub-assembly_2.11-0.6.jar` no `target/scala-2.11` diretório para o projeto.

## <a name="configure-kafka"></a>Configurar Kafka

A partir de uma ligação SSH ao nó de extremidade, utilize os seguintes passos para configurar Kafka para executar o conector no modo autónomo:

1. Guarde o nome do cluster para uma variável. Utilizar uma variável torna mais fácil copiar/colar outros comandos nesta secção:

    ```bash
    read -p "Enter the Kafka on HDInsight cluster name: " CLUSTERNAME
    ```

2. Instalar o `jq` utilitário. Este utilitário torna mais fácil de processamento de documentos JSON devolvidos do Ambari consultas:

    ```bash
    sudo apt -y install jq
    ```

3. Obter o endereço do Kafka mediadores. Poderão existir muitas mediadores no seu cluster, mas só precisa de fazer referência a um ou dois. Para obter o endereço de dois anfitriões de Mediador, utilize o seguinte comando:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $KAFKABROKERS
    ```

    Quando lhe for pedido, introduza a palavra-passe para o início de sessão (administrador) do cluster. O valor devolvido é semelhante ao seguinte texto:

    `wn0-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092,wn1-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092`

4. Obter o endereço de nós de Zookeeper. Existem vários nós de Zookeeper no cluster, mas só precisa de fazer referência a um ou dois. Para obter o endereço de dois nós de Zookeeper, utilize o seguinte comando:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

5. Ao executar o conector no modo autónomo, o `/usr/hdp/current/kafka-broker/config/connect-standalone.properties` ficheiro é utilizado para comunicar com os mediadores Kafka. Para editar o `connect-standalone.properties` ficheiro, utilize o seguinte comando:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-standalone.properties
    ```

    * Para configurar a configuração autónoma para o nó de extremidade localizar o Kafka mediadores, localizar a linha que começa com `bootstrap.servers=`. Substitua o `localhost:9092` valor com os anfitriões de Mediador do passo anterior.

    * Alterar o `key.converter=` e `value.converter=` linhas para os seguintes valores:

        ```text
        key.converter=org.apache.kafka.connect.storage.StringConverter
        value.converter=org.apache.kafka.connect.storage.StringConverter
        ```

        > [!IMPORTANT]
        > Esta alteração permite-lhe testar o produtor de consola incluído com Kafka a utilizar. Poderá ser necessário conversores diferentes para outros produtores e consumidores. Para obter informações sobre como utilizar outros valores de conversor, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    * Adicione uma linha no final do ficheiro que contém o seguinte texto:

        ```text
        consumer.max.poll.records=10
        ```

        > [!TIP]
        > Esta alteração é para impedir que os tempos limite no conector sink ao limitá-lo para 10 registos de cada vez. Para mais informações, consulte [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

6. Para guardar o ficheiro, utilize __Ctrl + X__, __Y__e, em seguida, __Enter__.

7. Para criar os tópicos utilizados pelo conector, utilize os seguintes comandos:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotin --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotout --zookeeper $KAFKAZKHOSTS
    ```

    Para verificar se o `iotin` e `iotout` tópicos existe, utilize o seguinte comando:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    O `iotin` tópico é utilizado para receber mensagens a partir do IoT Hub. O `iotout` tópico é utilizado para enviar mensagens ao IoT Hub.

## <a name="get-iot-hub-connection-information"></a>Obter informações de ligação do IoT Hub

Para obter informações do IoT hub utilizadas pelo conector, utilize os seguintes passos:

1. Obter o ponto final de compatível com o Event Hub e o nome do ponto final compatível com o Event Hub para o seu IoT hub. Para obter estas informações, utilize um dos seguintes métodos:

    * __Do [portal do Azure](https://portal.azure.com/)__, utilize os seguintes passos:

        1. Navegue até ao seu IoT Hub e selecione __pontos finais__.
        2. De __pontos finais incorporados__, selecione __eventos__.
        3. De __propriedades__, copie o valor dos seguintes campos:

            * __Nome compatível com o Hub de eventos__
            * __Ponto final compatível com o Hub de eventos__
            * __Partições__

        > [!IMPORTANT]
        > O valor de ponto final do portal pode conter texto adicional que não é necessária neste exemplo. Extraia o texto que corresponda a este padrão `sb://<randomnamespace>.servicebus.windows.net/`.

    * __Do [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, utilize o seguinte comando:

        ```azure-cli
        az iot hub show --name myhubname --query "{EventHubCompatibleName:properties.eventHubEndpoints.events.path,EventHubCompatibleEndpoint:properties.eventHubEndpoints.events.endpoint,Partitions:properties.eventHubEndpoints.events.partitionCount}"
        ```

        Substitua `myhubname` com o nome do seu IoT hub. A resposta é semelhante ao seguinte texto:

        ```text
        "EventHubCompatibleEndpoint": "sb://ihsuprodbnres006dednamespace.servicebus.windows.net/",
        "EventHubCompatibleName": "iothub-ehub-myhub08-207673-d44b2a856e",
        "Partitions": 2
        ```

2. Obter o __partilhado a política de acesso__ e __chave__. Neste exemplo, utilizar o __serviço__ chave. Para obter estas informações, utilize um dos seguintes métodos:

    * __Do [portal do Azure](https://portal.azure.com/)__, utilize os seguintes passos:

        1. Selecione __políticas de acesso partilhado__e, em seguida, selecione __serviço__.
        2. Copiar o __chave primária__ valor.
        3. Copiar o __cadeia de ligação - chave primária__ valor.

    * __Do [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__, utilize o seguinte comando:

        1. Para obter o valor de chave primária, utilize o seguinte comando:

            ```azure-cli
            az iot hub policy show --hub-name myhubname --name service --query "primaryKey"
            ```

            Substitua `myhubname` com o nome do seu IoT hub. A resposta é a chave primária para a `service` política para este hub.

        2. Para obter a cadeia de ligação para o `service` política, utilize o seguinte comando:

            ```azure-cli
            az iot hub show-connection-string --name myhubname --policy-name service --query "connectionString"
            ```

            Substitua `myhubname` com o nome do seu IoT hub. A resposta é a cadeia de ligação para o `service` política.

## <a name="configure-the-source-connection"></a>Configurar a ligação de origem

Para configurar a origem para funcionar com o seu IoT Hub, efetue as seguintes ações a partir de uma ligação SSH ao nó de extremidade:

1. Criar uma cópia do `connect-iot-source.properties` ficheiros o `/usr/hdp/current/kafka-broker/config/` diretório. Para transferir o ficheiro do projeto toketi kafka-connect-iothub, utilize o seguinte comando:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-source.properties
    ```

2. Para editar o `connect-iot-source.properties` de ficheiros e adicionar as informações de hub IoT, utilize o seguinte comando:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
    ```

    No editor, localizar e alterar as seguintes entradas:

    * `Kafka.Topic=PLACEHOLDER`: Substituir `PLACEHOLDER` com `iotin`. Mensagens recebidas do IoT hub são colocadas no `iotin` tópico.
    * `IotHub.EventHubCompatibleName=PLACEHOLDER`: Substituir `PLACEHOLDER` com o nome compatível com o Event Hub.
    * `IotHub.EventHubCompatibleEndpoint=PLACEHOLDER`: Substituir `PLACEHOLDER` com o ponto final compatível com o Event Hub.
    * `IotHub.Partitions=PLACEHOLDER`: Substituir `PLACEHOLDER` com o número de partições dos passos anteriores.
    * `IotHub.AccessKeyName=PLACEHOLDER`: Substituir `PLACEHOLDER` com `service`.
    * `IotHub.AccessKeyValue=PLACEHOLDER`: Substituir `PLACEHOLDER` com a chave primária do `service` política.
    * `IotHub.StartType=PLACEHOLDER`: Substituir `PLACEHOLDER` com uma data UTC. Esta data é quando o conector é iniciado a verificar mensagens. O formato de data é `yyyy-mm-ddThh:mm:ssZ`.
    * `BatchSize=100`: Substituir `100` com `5`. Esta alteração faz com que o conector permite ler mensagens na Kafka quando existem cinco novas mensagens no IoT hub.

    Para um exemplo de configuração, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

3. Para guardar as alterações, utilize __Ctrl + X__, __Y__e, em seguida, __Enter__.

Para obter mais informações sobre como configurar a origem do conector, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md).

## <a name="configure-the-sink-connection"></a>Configurar a ligação de sink

Para configurar a ligação de receptores para trabalhar com o seu IoT Hub, efetue as seguintes ações a partir de uma ligação SSH ao nó de extremidade:

1. Criar uma cópia do `connect-iothub-sink.properties` ficheiros o `/usr/hdp/current/kafka-broker/config/` diretório. Para transferir o ficheiro do projeto toketi kafka-connect-iothub, utilize o seguinte comando:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-sink.properties
    ```

2. Para editar o `connect-iothub-sink.properties` de ficheiros e adicionar as informações de hub IoT, utilize o seguinte comando:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
    ```

    No editor, localizar e alterar as seguintes entradas:

    * `topics=PLACEHOLDER`: Substituir `PLACEHOLDER` com `iotout`. Mensagens escritas para `iotout` tópico são reencaminhados para o IoT hub.
    * `IotHub.ConnectionString=PLACEHOLDER`: Substituir `PLACEHOLDER` com a cadeia de ligação para o `service` política.

    Para um exemplo de configuração, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

3. Para guardar as alterações, utilize __Ctrl + X__, __Y__e, em seguida, __Enter__.

Para obter mais informações sobre como configurar o sink de conector, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="start-the-source-connector"></a>Iniciar o conector de origem

Para iniciar o conector de origem, utilize o comando seguinte a partir de uma ligação SSH ao nó de extremidade:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
```

Assim que o conector for iniciada, envie mensagens ao IoT hub do seu dispositivo (s). Como o conector lê mensagens do IoT hub e armazena-os no tópico Kafka, regista as informações na consola:

```text
[2017-08-29 20:15:46,112] INFO Polling for data - Obtained 5 SourceRecords from IotHub (com.microsoft.azure.iot.kafka.co
nnect.IotHubSourceTask:39)
[2017-08-29 20:15:54,106] INFO Finished WorkerSourceTask{id=AzureIotHubConnector-0} commitOffsets successfully in 4 ms (
org.apache.kafka.connect.runtime.WorkerSourceTask:356)
```

> [!NOTE]
> Poderá ver várias avisos como o conector é iniciado. Estes avisos não causam problemas com a receção de mensagens do IoT hub.

Para parar o conector, utilize __Ctrl + C__.

## <a name="start-the-sink-connector"></a>Iniciar o conector do sink

A partir de uma ligação SSH ao nó de extremidade, utilize o seguinte comando para iniciar o conector do sink no modo autónomo:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
```

Como o conector é executada, são apresentadas informações semelhantes para o seguinte texto:

```text
[2017-08-30 17:49:16,150] INFO Started tasks to send 1 messages to devices. (com.microsoft.azure.iot.kafka.connect.sink.
IotHubSinkTask:47)
[2017-08-30 17:49:16,150] INFO WorkerSinkTask{id=AzureIotHubSinkConnector-0} Committing offsets (org.apache.kafka.connec
t.runtime.WorkerSinkTask:262)
```

> [!NOTE]
> Poderá reparar alguns avisos como o conector é iniciado. Pode ignorar com segurança estes avisos.

Para enviar mensagens através do conector, utilize os seguintes passos:

1. Abra outra sessão SSH para o cluster Kafka:

    ```bash
    ssh sshuser@new-edgenode.clustername-ssh.azurehdinsight.net
    ```
2. Para enviar mensagens para o `iotout` tópico, utilize o seguinte comando:

    > [!WARNING]
    > Uma vez que esta é uma nova ligação de SSH, o `$KAFKABROKERS` variável contém quaisquer informações. Para defini-lo, utilize um dos seguintes métodos:
    >
    > * Utilize os passos em três primeiros o [configurar Kafka](#configure-kafka) secção.
    > * Utilize `echo $KAFKABROKERS` a ligação de SSH anteriores para obter os valores e, em seguida, substitua `$KAFKABROKERS` no comando seguinte com os valores reais.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic iotout
    ```

    Este comando não, regressa ao pedido de Bash normal. Em vez disso, envia introdução por teclado para o `iotout` tópico.

3. Para enviar uma mensagem para o seu dispositivo, cole um documento JSON da sessão SSH para a `kafka-console-producer`.

    > [!IMPORTANT]
    > Tem de definir o valor da `"deviceId"` entrada para o ID do seu dispositivo. No exemplo seguinte, o dispositivo é denominado `fakepi`:

    ```text
    {"messageId":"msg1","message":"Turn On","deviceId":"fakepi"}
    ```

    O esquema para este documento JSON está descrito com maior detalhe em [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

    Se estiver a utilizar o dispositivo simulado Raspberry Pi e está a ser executado, a mensagem seguinte é registada pelo dispositivo:

    ```text
    Receive message: Turn On
    ```

    Reenviar o documento JSON, mas a alterar o valor da `"message"` entrada. O novo valor é registado pelo dispositivo.

Para obter mais informações sobre como utilizar o conector do sink, consulte [ https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md ](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md).

## <a name="next-steps"></a>Passos Seguintes

Neste documento, aprendeu a utilizar a API de ligar Kafka para iniciar o conector de Kafka IoT no HDInsight. Utilize as seguintes ligações para detetar outras formas de trabalhar com Kafka:

* [Utilizar o Apache Spark com Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)
