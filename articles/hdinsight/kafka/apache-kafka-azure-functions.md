---
title: Utilizar as funções do Azure para enviar dados para Kafka no HDInsight | Microsoft Docs
description: Saiba como utilizar uma função do Azure para escrever dados Kafka do HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/08/2018
ms.author: larryfr
ms.openlocfilehash: c657cda7dce0611cb357a0a2063a154f2f6f25f2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33939809"
---
# <a name="use-kafka-on-hdinsight-from-an-azure-function-app"></a>Utilizar o Kafka no HDInsight a partir de uma aplicação de função do Azure

Saiba como criar uma aplicação de função do Azure que envia dados para Kafka no HDInsight.

[As funções do Azure](https://docs.microsoft.com/azure/azure-functions/) é um serviço de computação sem servidor. Permite-lhe executar código a pedido sem ter de aprovisionar ou gerir a infraestrutura explicitamente.

O [Apache Kafka](https://kafka.apache.org) é uma plataforma de transmissão em fluxo distribuída de código aberto que pode ser utilizada para criar aplicações e pipelines de dados de transmissão em fluxo em tempo real. O Kafka também fornece a funcionalidade de mediador de mensagem semelhante a uma fila de mensagens, onde pode publicar e subscrever fluxos de dados nomeados. O Kafka no HDInsight fornece um serviço gerido, altamente escalável e disponível na cloud do Microsoft Azure.

Kafka no HDInsight não fornece uma API na internet pública. Para publicar ou consumir dados de Kafka, tem de ligar ao cluster Kafka através de uma rede Virtual do Azure. As funções do Azure fornece uma maneira conveniente para criar um ponto de final público que envia dados em Kafka no HDInsight através de um gateway de rede Virtual.

> [!NOTE]
> O foco deste documento é os passos necessários para ativar as funções do Azure comunicar com Kafka no HDInsight. O exemplo em si é apenas um produtor Kafka básico para demonstrar que a configuração funciona.

## <a name="prerequisites"></a>Pré-requisitos

* Uma aplicação de função do Azure. Para obter mais informações, consulte o [criar a sua primeira função](../../azure-functions/functions-create-first-azure-function.md) documentação.

* Uma rede Virtual do Azure. Para trabalhar com as funções do Azure, a rede virtual tem de utilizar um dos seguintes intervalos IP:

    * 10.0.0.0-10.255.255.255
    * 172.16.0.0-172.31.255.255
    * 192.168.0.0-192.168.255.255

    Para obter mais informações, consulte o [integrar a sua aplicação com uma Azure Virtual Network](../../app-service/web-sites-integrate-with-vnet.md) documento.

* Um Kafka num cluster do HDInsight. Para obter informações sobre como criar um Kafka num cluster do HDInsight, consulte o [criar um cluster de Kafka](apache-kafka-get-started.md) documento.

    > [!IMPORTANT]
    > Durante a configuração de cluster, tem de utilizar o __definições avançadas__ passo para selecionar a rede Virtual do Azure e a sub-rede que utiliza o HDInsight. Selecione a rede virtual e sub-rede criada no passo anterior.

    Para obter mais informações sobre Kafka e redes virtuais, consulte o [ligar ao Kafka através de uma rede virtual](apache-kafka-connect-vpn-gateway.md) documento.

## <a name="architecture"></a>Arquitetura

Kafka no HDInsight está contido numa rede Virtual do Azure. As funções do Azure podem comunicar com a rede virtual, utilizando um gateway de ponto a Site. A imagem seguinte é um diagrama desta topologia de rede:

![Arquitetura das funções do Azure, ligar para o HDInsight](./media/apache-kafka-azure-functions/kafka-azure-functions.png)

## <a name="configure-kafka"></a>Configurar Kafka

As informações nesta secção prepara o cluster Kafka para aceitar dados da função do Azure. Abrange as seguintes ações de configuração:

* Publicidade de IP
* Endereços de IP do Mediador de Kafka de recolha
* Criar um tópico Kafka

### <a name="configure-kafka-for-ip-advertising"></a>Configurar Kafka de publicidade de IP

Por predefinição, o Zookeeper devolve o nome de domínio de mediadores de Kafka aos clientes. Esta configuração não funciona sem um servidor DNS, como o cliente (funções do Azure) não é possível resolver nomes para a rede virtual. Para esta configuração, utilize os seguintes passos para configurar Kafka anunciará endereços IP em vez de nomes de domínio:

1. Um web browser, aceda ao https://CLUSTERNAME.azurehdinsight.net. Substitua __CLUSTERNAME__ com o nome de Kafka num cluster do HDInsight.

    Quando lhe for pedido, utilize HTTPS nome de utilizador e palavra-passe para o cluster. É apresentado a IU da Web do Ambari para o cluster.

2. Para ver informações no Kafka, selecione __Kafka__ na lista à esquerda.

    ![Lista de serviço com Kafka realçado](./media/apache-kafka-azure-functions/select-kafka-service.png)

3. Para ver a configuração de Kafka, selecione __folhas__ do meio superior.

    ![Ligações de folhas de Kafka](./media/apache-kafka-azure-functions/select-kafka-config.png)

4. Para localizar o __kafka env__ configuração, introduza `kafka-env` no __filtro__ campo no canto superior direito.

    ![Configuração de Kafka, para kafka env](./media/apache-kafka-azure-functions/search-for-kafka-env.png)

5. Para configurar Kafka anunciará endereços IP, adicione o seguinte texto para o fim do __modelo de env kafka__ campo:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Para configurar a interface de escuta Kafka, introduza `listeners` no __filtro__ campo no canto superior direito.

7. Para configurar Kafka escutar em todas as interfaces de rede, altere o valor no __os serviços de escuta__ campo para `PLAINTEXT://0.0.0.0:9092`.

8. Para guardar as alterações de configuração, utilize o __guardar__ botão. Introduza uma mensagem de texto que descreve as alterações. Selecione __OK__ depois das alterações foram guardadas.

    ![Configuração botão Guardar](./media/apache-kafka-azure-functions/save-button.png)

9. Para evitar erros ao reiniciar Kafka, utilize o __serviço ações__ botão e selecione __ativar no modo de manutenção__. Selecione OK para concluir esta operação.

    ![Ações de serviço, com ative manutenção realçada](./media/apache-kafka-azure-functions/turn-on-maintenance-mode.png)

10. Para reiniciar Kafka, utilize o __reiniciar__ botão e selecione __afetadas todas reinicie__. Confirme o reinício e, em seguida, utilize o __OK__ botão após a conclusão da operação.

    ![Reinicie o botão ao reiniciar todos os afetados realçado](./media/apache-kafka-azure-functions/restart-button.png)

11. Para desativar o modo de manutenção, utilize o __serviço ações__ botão e selecione __Ativar desativar modo de manutenção__. Selecione **OK** para concluir esta operação.

### <a name="get-the-kafka-broker-ip-address"></a>Obter o Mediador de Kafka endereço IP

Utilize um dos seguintes métodos para obter os endereços IP de nós no Kafka cluster e nome de domínio completamente qualificado (FQDN):

```powershell
$resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

$clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

$nodes = @()
foreach($nic in $clusterNICs) {
    $node = new-object System.Object
    $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
    $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
    $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
    $nodes += $node
}
$nodes | sort-object Type
```

```azurecli
az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
```

Este comando assume que `<resourcegroupname>` é o nome do grupo de recursos do Azure que contém a rede virtual.

Na lista de nomes devolvido, selecione o endereço IP de um workernode. O nome de domínio completamente qualificado interno do nó começa com as letras `wn`. Este endereço IP é utilizado pela função para comunicar com Kafka.

### <a name="create-a-kafka-topic"></a>Criar um tópico Kafka

Kafka armazena dados em __tópicos__. Antes de enviar dados para Kafka a partir de uma função do Azure, terá de criar a função.

Para criar um tópico, utilize os passos a [criar um cluster de Kafka](apache-kafka-get-started.md) documento.

## <a name="create-a-function-that-sends-to-kafka"></a>Criar uma função que envia para Kafka

> [!NOTE]
> Os passos nesta secção criar uma função de JavaScript que utiliza o [kafka nó](https://www.npmjs.com/package/kafka-node) pacote para publicar dados Kafka:

1. Crie um novo __WebHook + API__ funcionar e selecione __JavaScript__ como o idioma. Para obter mais informações sobre a criação de novas funções, consulte o [criar a sua primeira função](../../azure-functions/functions-create-first-azure-function.md#create-function) documento.

2. Utilize o seguinte código como o corpo da função:

    ```javascript
    var kafka = require('kafka-node');

    // The topic and a Kafka broker host for your HDInsight cluster
    var topic = 'test';
    var brokerHost = '10.1.0.7:9092';
    // Create the client
    var client= new kafka.KafkaClient({kafkaHost: brokerHost});

    module.exports = function (context, req) {
        // Create the producer
        var producer = new kafka.Producer(client, {requireAcks: 1});

        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            var name = req.query.name || req.body.name
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + name
            };
            // Create the payload, using the name as the body
            var payloads = [
                    { topic: topic, messages: [name]}
            ];
            context.log("calling producer.send");
            // Send the data to Kafka
            producer.send(payloads, function(err, data) {
                if(err) {
                    context.log(err);
                } else {
                    context.log(data);
                }
            });
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    Substitua `'test'` com o nome do tópico Kafka criado no cluster do HDInsight.

    Substitua `10.1.0.7` com o endereço IP que obteve anteriormente. Deixe o `:9092` valor. 9092 é a porta que Kafka escuta.

    Utilize o __guardar__ botão para guardar as alterações.

    ![Editor com o botão de gravação](./media/apache-kafka-azure-functions/function-editor.png)

3. A partir da direita do editor de função, selecione __ver ficheiros__. Selecione __+ adicionar__ e adicione um novo ficheiro designado `package.json`. Este ficheiro é utilizado para especificar a dependência no `kafka-node` pacote.

    ![Adicione um ficheiro](./media/apache-kafka-azure-functions/add-files.png)

4. Para editar o novo ficheiro, selecione `package.json` do __ver ficheiros__ lista. Utilize o seguinte texto como o conteúdo do ficheiro:

    ```json
    {
    "name": "kafkatest",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "author": "",
    "license": "ISC",
    "dependencies": {
            "kafka-node": "^2.4.1"
        }
    }
    ```

    Utilize o __guardar__ botão para guardar as alterações.

5. Para instalar o `kafka-node` do pacote, utilize o _versão e o pacote de gestão do nó_ secção o [guia para programadores do Azure funções JavaScript](../../azure-functions/functions-reference-node.md#node-version-and-package-management).

    > [!NOTE]
    > Poderá receber vários erros que o pacote de nó kafka está instalado. Pode ignorar com segurança estes erros.

## <a name="run-the-function"></a>Executar a função

A partir da direita do editor de função, selecione __teste__. Deixe as predefinições para o teste e, em seguida, selecione __executar__. Como é executado o teste, transmite um `name` parâmetro da função. Este parâmetro contém um valor de `Azure`, que a função insere na Kafka.

![Captura de ecrã de caixa de diálogo de teste](./media/apache-kafka-azure-functions/function-test-dialog.png)

Para ver informações registadas pela função enquanto executa o teste, selecione __registos__ na parte inferior da página. Execute o teste novamente para gerar informações de registo.

![Exemplo de saída de registo de função](./media/apache-kafka-azure-functions/function-log.png)

## <a name="verify-the-data-is-in-kafka"></a>Certifique-se de que os dados estão no Kafka

Para verificar que os dados chegaram no tópico Kafka, utilize as informações de _produzir e consumir registos_ secção o [criar um cluster de Kafka](apache-kafka-get-started.md#produce-and-consume-records) documento. O `kafka-console-consumer` lê os dados do tópico e apresenta uma lista de mensagens armazenados no tópico.

## <a name="next-steps"></a>Passos Seguintes

Utilize as seguintes ligações para saber como utilizar o Apache Kafka no HDInsight:

* [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md)

* [Utilizar MirrorMaker para criar uma réplica de Kafka no HDInsight](apache-kafka-mirroring.md)

* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Utilizar o Apache Spark com Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Ligar ao Kafka através de uma Rede Virtual do Azure](apache-kafka-connect-vpn-gateway.md)
