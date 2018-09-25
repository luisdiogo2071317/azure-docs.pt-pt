---
title: Elevada disponibilidade para o Hadoop - Azure HDInsight
description: Saiba como o HDInsight clusters melhorar a fiabilidade e disponibilidade, através de um nó principal adicional. Saiba como isso afeta os serviços do Hadoop, como Ambari e do Hive, bem como para ligar individualmente para cada nó principal através de SSH.
services: hdinsight
ms.reviewer: jasonh
author: jasonwhowell
keywords: elevada disponibilidade do hadoop
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.openlocfilehash: 33458794ad74b367f1278364d7b4ace30f7d13a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953594"
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a>Disponibilidade e fiabilidade dos clusters Hadoop no HDInsight.

Clusters do HDInsight fornecem dois nós principais para aumentar a disponibilidade e fiabilidade dos serviços do Hadoop e tarefas em execução.

Hadoop alcança a elevada disponibilidade e fiabilidade através da replicação de dados e serviços em vários nós num cluster. No entanto distribuições norma do Hadoop têm, normalmente, apenas um único nó principal. Qualquer interrupção do nó principal único pode fazer com que o cluster para parar de funcionar. HDInsight fornece dois nós principais para melhorar a disponibilidade e fiabilidade do Hadoop.

> [!IMPORTANT]
> O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

## <a name="availability-and-reliability-of-nodes"></a>Disponibilidade e fiabilidade de nós

Nós num cluster do HDInsight são implementados com máquinas virtuais do Azure. As secções seguintes abordam os tipos de nós individuais utilizados com o HDInsight. 

> [!NOTE]
> Nem todos os tipos de nó são utilizados para um tipo de cluster. Por exemplo, um tipo de cluster de Hadoop não tem quaisquer nós do Nimbus. Para obter mais informações sobre nós usados por tipos de cluster do HDInsight, consulte a secção de tipos de Cluster do [clusters do Hadoop de baseados em criar Linux no HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) documento.

### <a name="head-nodes"></a>Nós de cabeça

Para garantir a elevada disponibilidade dos serviços do Hadoop, o HDInsight fornece dois nós principais. Ambos os nós principais são ativa e em execução dentro do cluster do HDInsight em simultâneo. Alguns serviços, como o HDFS ou o YARN, são apenas 'active' num nó principal em qualquer momento. Outros serviços, como o HiveServer2 ou Hive MetaStore estão ativos na exceção ambos os nós principais ao mesmo tempo.

Nós principais (e outros nós no HDInsight) tem um valor numérico como parte do nome de anfitrião do nó. Por exemplo, `hn0-CLUSTERNAME` ou `hn4-CLUSTERNAME`.

> [!IMPORTANT]
> Associa o valor numérico ao se um nó é primária ou secundária. O valor numérico só está presente para fornecer um nome exclusivo para cada nó.

### <a name="nimbus-nodes"></a>Nós do Nimbus

Nós do nimbus estão disponíveis com clusters do Storm. Os nós do Nimbus fornecem funcionalidade semelhante ao JobTracker do Hadoop, distribuir e monitorizar o processamento em nós de trabalho. HDInsight fornece dois nós Nimbus para clusters do Storm

### <a name="zookeeper-nodes"></a>Nós do Zookeeper

[ZooKeeper](http://zookeeper.apache.org/) nós são utilizados para a eleição do líder de serviços principais em nós principais. Também são utilizados para assegurar que os serviços, nós de dados (trabalho) e gateways sabem qual nó principal um serviço principal está ativo no. Por predefinição, o HDInsight fornece três nós ZooKeeper.

### <a name="worker-nodes"></a>Nós de trabalho

Nós de trabalho executam a análise de dados real, quando uma tarefa for submetida para o cluster. Se falhar um nó de trabalho, a tarefa que estava a realizar é submetida para outro nó de trabalho. Por predefinição, o HDInsight cria quatro nós de trabalho. Pode alterar este número para satisfazer as suas necessidades, durante e após a criação do cluster.

### <a name="edge-node"></a>Nó periférico

Um nó de extremidade não participar ativamente de análise de dados dentro do cluster. Ele é usado por desenvolvedores ou os cientistas de dados ao trabalhar com o Hadoop. Nó de extremidade reside na mesma rede Virtual do Azure que os outros nós do cluster e pode acessar diretamente todos os outros nós. Nó de extremidade pode ser utilizado sem colocar os recursos dos serviços críticos do Hadoop ou tarefas de análises.

Atualmente, os serviços de ML no HDInsight é o único tipo de cluster, que fornece um nó de extremidade por predefinição. Nó de extremidade é utilizado para serviços de ML no HDInsight, código de teste R localmente no nó antes de enviá-lo para o cluster de processamento distribuído.

Para obter informações sobre a utilização de um nó de extremidade com outros tipos de cluster, consulte a [utilizar nós de extremidade no HDInsight](hdinsight-apps-use-edge-node.md) documento.

## <a name="accessing-the-nodes"></a>Aceder a nós

Acesso ao cluster através da internet é fornecido através de um gateway público. O acesso é limitado à ligação para os nós principais e (se existir) do nó de extremidade. Acesso a serviços em execução em nós de principais não é afetado fazendo com que vários nós principais. O gateway público encaminha os pedidos para o nó principal que aloja o serviço pedido. Por exemplo, se Ambari estar atualmente alojado num nó principal secundário, o gateway encaminha os pedidos recebidos para Ambari para esse nó.

Acesso através do gateway público está limitado a porta 443 (HTTPS), 22 e 23.

* Porta __443__ é utilizada para aceder a outro web da interface do Usuário ou de APIs de REST alojada sobre os nós principais e de Ambari.

* Porta __22__ é utilizado para aceder ao nó principal primário ou um nó de extremidade com SSH.

* Porta __23__ é utilizado para aceder ao nó principal secundário com SSH. Por exemplo, `ssh username@mycluster-ssh.azurehdinsight.net` liga-se ao nó principal primário do cluster com o nome **mycluster**.

Para obter mais informações sobre como utilizar o SSH, consulte a [utilizar o SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) documento.

### <a name="internal-fully-qualified-domain-names-fqdn"></a>Nomes de domínio completamente qualificado interno (FQDN)

Nós num cluster do HDInsight tem um endereço IP e o FQDN que só pode ser acedido a partir do cluster interno. Ao acessar serviços no cluster com o endereço IP ou FQDN, deve utilizar Ambari para verificar o IP ou FQDN para utilizar quando aceder ao serviço.

Por exemplo, o serviço de Oozie só pode executar num nó principal e a utilizar o `oozie` comando numa sessão SSH requer o URL para o serviço. Este URL pode ser recuperada do Ambari, utilizando o seguinte comando:

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Este comando devolve um valor semelhante para o seguinte comando, que contém o URL interno para utilizar com o `oozie` comando:

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

Para obter mais informações sobre como trabalhar com a API de REST do Ambari, consulte [monitorizar e gerir o HDInsight com a API de REST do Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

### <a name="accessing-other-node-types"></a>Aceder a outros tipos de nó

Pode ligar a nós que não estão diretamente acessíveis na Internet, utilize os seguintes métodos:

* **SSH**: quando estiverem ligados a um nó principal através de SSH, pode, em seguida, utilizar o SSH do nó principal para ligar a outros nós do cluster. Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* **Túnel SSH**: Se precisar de aceder a um serviço web hospedado em um de nós que não está exposta à internet, tem de utilizar um túnel SSH. Para obter mais informações, consulte a [utilizar um túnel SSH com HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) documento.

* **Rede Virtual do Azure**: se o cluster de HDInsight for parte de uma rede Virtual do Azure, qualquer recurso na mesma rede Virtual pode aceder diretamente a todos os nós do cluster. Para obter mais informações, consulte a [HDInsight expandir com rede Virtual do Azure](hdinsight-extend-hadoop-virtual-network.md) documento.

## <a name="how-to-check-on-a-service-status"></a>Como verificar um Estado de serviço

Para verificar o estado dos serviços que são executados sobre os nós principais, utilize a interface do Usuário de Web do Ambari ou a API de REST do Ambari.

### <a name="ambari-web-ui"></a>IU Web do Ambari

A interface do Usuário da Web de Ambari ser exibido em https://CLUSTERNAME.azurehdinsight.net. Substitua **CLUSTERNAME** pelo nome do cluster. Se lhe for pedido, introduza as credenciais de utilizador HTTP para o seu cluster. O nome de utilizador HTTP predefinido é **administrador** e a palavra-passe é a palavra-passe que introduziu quando criou o cluster.

Quando chegar na página do Ambari, os serviços instalados estão listados no lado esquerdo da página.

![Serviços instalados](./media/hdinsight-high-availability-linux/services.png)

Há uma série de ícones que podem aparecer ao lado de um serviço para indicar o estado. Quaisquer alertas relacionados com um serviço podem ser visualizadas através do **alertas** link na parte superior da página. Pode selecionar cada serviço para ver mais informações no mesmo.

Enquanto a página de serviço fornece informações sobre o estado e a configuração de cada serviço, ele não fornece informações no nó principal do qual o serviço está em execução. Para ver estas informações, utilize o **anfitriões** link na parte superior da página. Esta página apresenta os anfitriões dentro do cluster, incluindo os nós principais.

![lista de anfitriões](./media/hdinsight-high-availability-linux/hosts.png)

Selecionar a ligação para um de nós principais apresenta os serviços e componentes em execução nesse nó.

![Estado do componente](./media/hdinsight-high-availability-linux/nodeservices.png)

Para obter mais informações sobre a utilização do Ambari, consulte [monitorizar e gerir o HDInsight com a IU da Web de Ambari](hdinsight-hadoop-manage-ambari.md).

### <a name="ambari-rest-api"></a>API REST do Ambari

A API de REST do Ambari está disponível através da internet. O gateway público HDInsight processa o encaminhamento de pedidos para o nó principal que está atualmente a alojar a API REST.

Pode utilizar o seguinte comando para verificar o estado de um serviço através da API de REST do Ambari:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Substitua **palavra-passe** com a palavra-passe da conta do utilizador (administrador) de HTTP.
* Substitua **CLUSTERNAME** pelo nome do cluster.
* Substitua **SERVICENAME** com o nome do serviço que pretende verificar o estado do.

Por exemplo, para verificar o estado do **HDFS** serviço num cluster com o nome **mycluster**, com uma palavra-passe **palavra-passe**, usaria o seguinte comando:

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

A resposta é semelhante ao seguinte JSON:

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

O URL nos informa que o serviço está atualmente em execução num nó principal com o nome **hn0 CLUSTERNAME**.

O estado indica que o serviço está atualmente em execução, ou **iniciado**.

Se não souber que serviços estão instalados no cluster, pode utilizar o seguinte comando para obter uma lista:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

Para obter mais informações sobre como trabalhar com a API de REST do Ambari, consulte [monitorizar e gerir o HDInsight com a API de REST do Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

#### <a name="service-components"></a>Componentes do serviço

Serviços poderão conter os componentes que deseja verificar o estado de individualmente. Por exemplo, o HDFS contém o componente de NameNode. Para ver informações num componente, o comando seria:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Se não souber quais componentes são fornecidos por um serviço, pode utilizar o seguinte comando para obter uma lista:

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-to-access-log-files-on-the-head-nodes"></a>Como aceder a ficheiros de registo sobre os nós principais

### <a name="ssh"></a>SSH

Quando ligado a um nó principal através de SSH, os ficheiros de registo podem ser encontrados na **/var/do registo**. Por exemplo, **/var/log/hadoop-yarn/yarn** contêm os registos do YARN.

Cada nó principal pode ter entradas de registo exclusivo, então, deve verificar os logs em ambos.

### <a name="sftp"></a>SFTP

Também pode ligar ao nó principal a utilizar o SSH File Transfer Protocol ou proteger o protocolo SFTP (File Transfer) e transferir os ficheiros de registo diretamente.

Semelhante à utilização de um cliente SSH, quando ligar ao cluster tem de fornecer o nome de conta de utilizador SSH e o endereço SSH do cluster. Por exemplo, `sftp username@mycluster-ssh.azurehdinsight.net`. Forneça a palavra-passe da conta quando lhe for pedido, ou fornecer um utilizar chaves públicas a `-i` parâmetro.

Assim que estiver ligado, é apresentada uma `sftp>` prompt. Desta linha de comandos, pode alterar a diretórios, carregar e transferir ficheiros. Por exemplo, os seguintes comandos alteram os diretórios para o **/var/log/hadoop/hdfs** directory e, em seguida, transferir todos os ficheiros no diretório.

    cd /var/log/hadoop/hdfs
    get *

Para obter uma lista de comandos disponíveis, introduza `help` no `sftp>` prompt.

> [!NOTE]
> Há também interfaces gráficas que permitem-lhe visualizar o sistema de ficheiros quando estiver ligado através de SFTP. Por exemplo, [MobaXTerm](http://mobaxterm.mobatek.net/) permite-lhe procurar o sistema de ficheiros através de uma interface semelhante ao Windows Explorer.

### <a name="ambari"></a>Ambari

> [!NOTE]
> Para aceder a ficheiros de registo com o Ambari, tem de utilizar um túnel SSH. As interfaces da web para os serviços individuais não são expostas publicamente na Internet. Para obter informações sobre como utilizar um túnel SSH, consulte a [Use SSH Tunneling túnel](hdinsight-linux-ambari-ssh-tunnel.md) documento.

Na IU da Web do Ambari, selecione o serviço que pretende ver os registos para (por exemplo, YARN). Em seguida, utilize **ligações rápidas** para selecionar o nó principal para ver os registos para.

![Utilizar ligações rápidas para ver registos](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-to-configure-the-node-size"></a>Como configurar o tamanho de nó

O tamanho de um nó só pode ser selecionado durante a criação do cluster. Pode encontrar uma lista de tamanhos de VM diferentes disponíveis para o HDInsight no [página de preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Ao criar um cluster, pode especificar o tamanho de nós. As seguintes informações fornecem orientações sobre como especificar o tamanho a utilizar o [portal do Azure][preview-portal], [Azure PowerShell][azure-powershell]e o [CLI clássica do azure][azure-cli]:

* **Portal do Azure**: ao criar um cluster, pode definir o tamanho de nós utilizados pelo cluster:

    ![Imagem do Assistente de criação de clusters com a seleção de tamanho de nó](./media/hdinsight-high-availability-linux/headnodesize.png)

* **CLI clássica do Azure**: ao utilizar o `azure hdinsight cluster create` comando, pode definir o tamanho do cabeçalho, trabalho e nós ZooKeeper, utilizando o `--headNodeSize`, `--workerNodeSize`, e `--zookeeperNodeSize` parâmetros.

* **O Azure PowerShell**: ao utilizar o `New-AzureRmHDInsightCluster` cmdlet, pode definir o tamanho do cabeçalho, trabalho e nós ZooKeeper, utilizando o `-HeadNodeVMSize`, `-WorkerNodeSize`, e `-ZookeeperNodeSize` parâmetros.

## <a name="next-steps"></a>Passos Seguintes

Utilize as seguintes ligações para saber mais sobre as coisas mencionadas neste documento.

* [Referência REST do Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [Instalar e configurar a CLI clássica do Azure](../cli-install-nodejs.md)
* [Instale e configure o Azure PowerShell](/powershell/azure/overview).
* [Gerir o HDInsight com o Ambari](hdinsight-hadoop-manage-ambari.md)
* [Aprovisionar clusters do HDInsight baseado em Linux](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
