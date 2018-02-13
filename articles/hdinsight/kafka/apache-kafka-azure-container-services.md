---
title: "Utilizar o serviço de contentor do Azure com Kafka no HDInsight | Microsoft Docs"
description: "Saiba como utilizar Kafka no HDInsight a partir de imagens do contentor alojadas no serviço de contentor do Azure (AKS)."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2018
ms.author: larryfr
ms.openlocfilehash: 53342e11476a307bb6af356eb40fe51928041822
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="use-azure-container-services-with-kafka-on-hdinsight"></a>Utilizar os serviços de contentor do Azure com Kafka no HDInsight

Saiba como utilizar os serviços de contentor do Azure (AKS) com Kafka num cluster do HDInsight. Os passos neste documento utilizam uma aplicação Node.js alojada no AKS para verificar a conectividade com Kafka. Esta aplicação utiliza o [kafka nó](https://www.npmjs.com/package/kafka-node) pacote para comunicar com Kafka. Utiliza [Socket.io](https://socket.io/) evento suscitada pelo departamento de processamento de mensagens entre o browser cliente e o back-end alojadas no AKS.

O [Apache Kafka](https://kafka.apache.org) é uma plataforma de transmissão em fluxo distribuída de código aberto que pode ser utilizada para criar aplicações e pipelines de dados de transmissão em fluxo em tempo real. Serviço de contentor do Azure gere o seu ambiente alojado do Kubernetes e faz com que um processo rápido e fácil de implementar aplicações de. Utilizar uma rede Virtual do Azure, pode ligar os dois serviços.

> [!NOTE]
> O foco deste documento é os passos necessários para ativar os serviços de contentor do Azure comunicar com Kafka no HDInsight. O exemplo em si é apenas um cliente Kafka básica para demonstrar que a configuração funciona.

## <a name="prerequisites"></a>Pré-requisitos

* [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Uma subscrição do Azure.

Este documento parte do princípio de que está familiarizado com a criar e utilizar os seguintes serviços do Azure:

* Kafka no HDInsight
* Azure Container Service
* Redes Virtuais do Azure

Este documento também parte do princípio de que tem walked de [tutorial de serviços de contentor do Azure](../../aks/tutorial-kubernetes-prepare-app.md). Este tutorial cria um serviço de contentor, cria um cluster de Kubernetes, um registo de contentor e configura o `kubectl` utilitário.

## <a name="architecture"></a>Arquitetura

### <a name="network-topology"></a>Topologia da rede

HDInsight e AKS utilizar uma rede Virtual do Azure como um contentor de recursos de computação. Para ativar a comunicação entre o HDInsight e AKS, tem de ativar a comunicação entre as respetivas redes. Os passos neste documento utilizam o Peering de rede Virtual para as redes. Outras ligações, por exemplo, VPN, também deverão funcionar. Para obter mais informações sobre peering, consulte o [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md) documento.


O diagrama seguinte ilustra a topologia de rede utilizada neste documento:

![HDInsight numa rede virtual, AKS noutra e as redes ligadas através do peering](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]
> Resolução de nomes não está ativada entre as redes em modo de peering, pelo que é utilizado o endereçamento IP. Por predefinição, Kafka no HDInsight está configurado para devolver os nomes de anfitrião em vez de endereços IP quando os clientes se liguem. Os passos neste documento modificar Kafka utilizar IP publicidade em vez disso.

## <a name="create-an-azure-container-service-aks"></a>Criar um serviço de contentor do Azure (AKS)

Se ainda não tiver um cluster AKS, utilize um dos seguintes documentos para saber como criar um:

* [Implementar um cluster do serviço de contentor do Azure (AKS) - Portal](../../aks/kubernetes-walkthrough-portal.md)
* [Implementar um cluster do serviço de contentor do Azure (AKS) - CLI](../../aks/kubernetes-walkthrough.md)

> [!NOTE]
> AKS cria uma rede virtual durante a instalação. Esta rede estiver em modo de peering para criado para o HDInsight na secção seguinte.

## <a name="configure-virtual-network-peering"></a>Configure o peering de rede virtual

1. Do [portal do Azure](https://portal.azure.com), selecione __grupos de recursos__e, em seguida, localize o grupo de recursos que contém a rede virtual para o seu cluster AKS. O nome do grupo de recursos é `MC_<resourcegroup>_<akscluster>_<location>`. O `resourcegroup` e `akscluster` entradas são o nome do grupo de recursos que criou o cluster e o nome do cluster. O `location` é a localização que o cluster foi criado no.

2. No grupo de recursos, selecione o __rede Virtual__ recursos.

3. Selecione __espaço de endereços__. Tenha em atenção de que o espaço de endereços listados.

4. Para criar uma rede virtual para o HDInsight, selecione __+ criar um recurso__, __redes__e, em seguida, __rede Virtual__.

    > [!IMPORTANT]
    > Quando os introduzir os valores para a nova rede virtual, tem de utilizar um espaço de endereço que não se sobreponha utilizada pela rede de cluster AKS.

    Utilizar o mesmo __localização__ para a rede virtual que utilizou para o cluster AKS.

    Aguarde até que a rede virtual foi criada antes de avançar para o passo seguinte.

5. Para configurar o peering entre a rede do HDInsight e a rede de cluster AKS, selecione a rede virtual e, em seguida, selecione __Peerings__. Selecione __+ adicionar__ e utilize os seguintes valores para preencher o formulário:

    * __Nome__: introduza um nome exclusivo para esta configuração de peering.
    * __Rede virtual__: Utilize este campo para selecionar a rede virtual para o **AKS cluster**.

    Deixe todos os outros campos de que o valor predefinido, em seguida, selecione __OK__ para configurar o peering.

6. Para configurar o peering entre a rede de cluster AKS e a rede do HDInsight, selecione o __rede virtual de cluster AKS__e, em seguida, selecione __Peerings__. Selecione __+ adicionar__ e utilize os seguintes valores para preencher o formulário:

    * __Nome__: introduza um nome exclusivo para esta configuração de peering.
    * __Rede virtual__: Utilize este campo para selecionar a rede virtual para o __cluster do HDInsight__.

    Deixe todos os outros campos de que o valor predefinido, em seguida, selecione __OK__ para configurar o peering.

## <a name="install-kafka-on-hdinsight"></a>Instalar Kafka no HDInsight

Ao criar o Kafka num cluster do HDInsight, tem de associar a rede virtual que criou anteriormente para o HDInsight. Para obter mais informações sobre como criar um cluster de Kafka, consulte o [criar um cluster de Kafka](apache-kafka-get-started.md) documento.

> [!IMPORTANT]
> Ao criar o cluster, tem de utilizar o __definições avançadas__ para associar a rede virtual que criou para o HDInsight.

## <a name="configure-kafka-ip-advertising"></a>Configurar Kafka IP publicidade

Utilize os seguintes passos para configurar Kafka anunciará endereços IP em vez de nomes de domínio:

1. Um web browser, aceda ao https://CLUSTERNAME.azurehdinsight.net. Substitua __CLUSTERNAME__ com o nome de Kafka num cluster do HDInsight.

    Quando lhe for pedido, utilize HTTPS nome de utilizador e palavra-passe para o cluster. É apresentado a IU da Web do Ambari para o cluster.

2. Para ver informações no Kafka, selecione __Kafka__ na lista à esquerda.

    ![Lista de serviço com Kafka realçado](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Para ver a configuração de Kafka, selecione __folhas__ do meio superior.

    ![Ligações de folhas de Kafka](./media/apache-kafka-azure-container-services/select-kafka-config.png)

4. Para localizar o __kafka env__ configuração, introduza `kafka-env` no __filtro__ campo no canto superior direito.

    ![Configuração de Kafka, para kafka env](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

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

    ![Configuração botão Guardar](./media/apache-kafka-azure-container-services/save-button.png)

9. Para evitar erros ao reiniciar Kafka, utilize o __serviço ações__ botão e selecione __ativar no modo de manutenção__. Selecione OK para concluir esta operação.

    ![Ações de serviço, com ative manutenção realçada](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Para reiniciar Kafka, utilize o __reiniciar__ botão e selecione __afetadas todas reinicie__. Confirme o reinício e, em seguida, utilize o __OK__ botão após a conclusão da operação.

    ![Reinicie o botão ao reiniciar todos os afetados realçado](./media/apache-kafka-azure-container-services/restart-button.png)

11. Para desativar o modo de manutenção, utilize o __serviço ações__ botão e selecione __Ativar desativar modo de manutenção__. Selecione **OK** para concluir esta operação.

## <a name="test-the-configuration"></a>Testar a configuração

Neste momento, Kafka e o serviço de contentor do Azure estão em comunicação através de redes virtuais em modo de peering. Para testar esta ligação, utilize os seguintes passos:

1. Crie um tópico de Kafka que é utilizado pela aplicação de teste. Para obter informações sobre como criar Kafka tópicos, consulte o [criar um cluster de Kafka](apache-kafka-get-started.md) documento.

2. Transferir a aplicação de exemplo do [https://github.com/Blackmist/Kafka-AKS-Test](https://github.com/Blackmist/Kafka-AKS-Test). 

3. Editar o `index.js` de ficheiros e alterar as seguintes linhas:

    * `var topic = 'mytopic'`: Substituir `mytopic` com o nome do tópico Kafka utilizado por esta aplicação.
    * `var brokerHost = '176.16.0.13:9092`: Substituir `176.16.0.13` com o endereço IP de um dos anfitriões do Mediador para o cluster.

        Para localizar o endereço IP interno do Mediador do anfitriões (workernodes) no cluster, consulte o [API de REST do Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) documento. Escolha uma das entradas em que o nome de domínio começa com o endereço IP `wn`.

4. Numa linha de comandos no `src` diretório, as dependências de instalar e utilizar o Docker para criar uma imagem para a implementação:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]
    > Pacotes exigidos desta aplicação são verificados no repositório, pelo que não necessita de utilizar o `npm` utilitário para instalá-los.

5. Início de sessão para o registo de contentor do Azure (ACR) e localizar o nome de loginServer:

    ```bash
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]
    > Se não souber o nome do registo de contentor do Azure, ou são familiarizado com a utilização da CLI do Azure para trabalhar com o serviço de contentor do Azure, consulte o [tutoriais AKS](../../aks/tutorial-kubernetes-prepare-app.md).

6. Etiqueta local `kafka-aks-test` imagem com o loginServer do ACR. Também adicionar `:v1` ao fim para indicar a versão de imagem:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Envie a imagem para o registo:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```
    Esta operação demora vários minutos a concluir.

8. Edite o ficheiro de manifesto Kubernetes (`kafka-aks-test.yaml`) e substitua `microsoft` com o nome de loginServer ACR obtido no passo 4.

9. Utilize o seguinte comando para implementar as definições da aplicação do manifesto:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. Utilize o seguinte comando a observar a `EXTERNAL-IP` da aplicação:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Assim que é atribuído um endereço IP externo, utilize __CTRL + C__ para sair da veja

11. Abra um browser e introduza o endereço IP externo para o serviço. Chegam a uma página semelhante para a imagem seguinte:

    ![Imagem da página web](./media/apache-kafka-azure-container-services/test-web-page.png)

12. Introduza o texto no campo e, em seguida, selecione o __enviar__ botão. Os dados são enviados para Kafka. O consumidor Kafka na aplicação lê a mensagem e adiciona-o para o __mensagens de Kafka__ secção.

    > [!WARNING]
    > Poderá receber várias cópias de uma mensagem. Normalmente, este problema ocorre quando o utilizador atualiza o seu browser depois de ligar ou abrir várias ligações de browser para a aplicação.

## <a name="next-steps"></a>Passos Seguintes

Utilize as seguintes ligações para saber como utilizar o Apache Kafka no HDInsight:

* [Introdução ao Kafka no HDInsight](apache-kafka-get-started.md)

* [Utilizar MirrorMaker para criar uma réplica de Kafka no HDInsight](apache-kafka-mirroring.md)

* [Utilizar o Apache Storm com Kafka no HDInsight](../hdinsight-apache-storm-with-kafka.md)

* [Utilizar o Apache Spark com Kafka no HDInsight](../hdinsight-apache-spark-with-kafka.md)

* [Ligar ao Kafka através de uma Rede Virtual do Azure](apache-kafka-connect-vpn-gateway.md)