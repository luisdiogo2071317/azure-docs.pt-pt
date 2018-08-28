---
title: Configuração de cluster de Hadoop, Spark, Kafka, HBase ou serviços de ML - Azure HDInsight
description: Configure o Hadoop, Kafka, Spark, HBase, serviços de ML ou Storm clusters para HDInsight de um navegador, a CLI do Azure, Azure PowerShell, REST ou SDK.
keywords: configuração de cluster do hadoop, instalação, configuração de cluster do spark, o que é o cluster do hadoop de cluster do kafka
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: f325e49695cab44d5c3d9fe94cd207755d136f7b
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107134"
---
# <a name="set-up-clusters-in-hdinsight-with-hadoop-spark-kafka-and-more"></a>Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Saiba como definir e configurar clusters no HDInsight com o Hadoop, Spark, Kafka, Interactive Query, HBase, serviços de ML ou Storm. Além disso, saiba como personalizar os clusters e adicionar segurança ao associá-los a um domínio.

Um cluster do Hadoop é composta por várias máquinas de virtuais (nós) que são utilizadas para processamento distribuído de tarefas. O Azure HDInsight processa os detalhes de implementação da instalação e configuração de nós individuais, para que tenha apenas fornecer informações de configuração geral. 

> [!IMPORTANT]
>A faturação do cluster do HDInsight tem início quando o cluster é criado e termina quando é eliminado. A faturação é rateada por minuto, pelo que deve sempre eliminar o cluster quando deixar de ser utilizado. Saiba como [eliminar um cluster.](hdinsight-delete-cluster.md)
>

## <a name="cluster-setup-methods"></a>Métodos de configuração de cluster
A tabela seguinte mostra os diferentes métodos que pode utilizar para configurar um cluster do HDInsight.

| Clusters criados com | Navegador da Web | Linha de comandos | API REST | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Portal do Azure](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [CLI do Azure (vidor 1.0)](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [SDK do .NET](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Modelos Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Criação rápida: configuração de cluster básico
Este artigo explica-lhe a configuração no [portal do Azure](https://portal.azure.com), onde pode criar um cluster do HDInsight com *criação rápida* ou *Custom*. 

![Opções de criação rápida personalizada de criação de hdinsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-options.png)

Siga as instruções no ecrã de uma configuração de cluster básico. Os detalhes são fornecidos abaixo para:

* [Nome do grupo de recursos](#resource-group-name)
* [Tipos de cluster e configuração](#cluster-types) 
* [Início de sessão do cluster e nome de utilizador SSH](#cluster-login-and-ssh-username)
* [Localização](#location)

> [!IMPORTANT]
> O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, consulte [descontinuação do HDInsight 3.3](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Nome do grupo de recursos 

[O Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) a trabalhar com os recursos em seu aplicativo como um grupo, referido como um grupo de recursos do Azure. Pode implementar, atualizar, monitorizar ou eliminar todos os recursos para a sua aplicação numa única operação coordenada.

## <a name="cluster-types"></a> Tipos de cluster e configuração
Atualmente, o Azure HDInsight fornece os seguintes tipos de cluster, cada um com um conjunto de componentes para fornecer determinadas funcionalidades.

> [!IMPORTANT]
> HDInsight clusters estão disponíveis em vários tipos, cada uma para uma única carga de trabalho ou tecnologia. Não existe nenhum método suportado para criar um cluster que combina vários tipos, como o Storm e HBase num cluster. Se sua solução necessitar de tecnologias que são distribuídas por vários tipos de cluster do HDInsight, um [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network) pode ligar-se os tipos de cluster necessários. 
>
>

| Tipo de cluster | Funcionalidade |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Consultas de batch e análises de dados armazenados |
| [HBase](hbase/apache-hbase-overview.md) |Processamento de grandes quantidades de dados sem esquemas, do NoSQL |
| [Interactive Query](./interactive-query/apache-interactive-query-get-started.md) |Colocação em cache de dentro da memória para consultas do Hive interativas e mais rápidas |
| [Kafka](kafka/apache-kafka-introduction.md) | Plataforma de transmissão em fluxo distribuída que pode ser utilizada para criar aplicações e pipelines de dados de transmissão em fluxo em tempo real |
| [Serviços de ML](r-server/r-server-overview.md) |Várias estatísticas de macrodados, modelação preditiva e capacidades de machine learning |
| [Spark](spark/apache-spark-overview.md) |Processamento na memória, consultas interativas, processamento de micro-lotes fluxo |
| [Storm](storm/apache-storm-overview.md) |Processamento de eventos em tempo real |


### <a name="hdinsight-version"></a>Versão do HDInsight
Escolha a versão do HDInsight para este cluster. Para obter mais informações, consulte [versões suportadas HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Pacote de segurança empresarial

Para tipos de cluster do Hadoop, Spark e Interactive Query, pode optar por ativar os **Enterprise Security Package**. Este pacote fornece a opção para ter uma configuração de cluster mais segura ao utilizar o Apache Ranger e integração com o Azure Active Directory. Para obter mais informações, consulte [Enterprise Security Package no Azure HDInsight](./domain-joined/apache-domain-joined-introduction.md).

![Opções de criação de hdinsight escolha o pacote de segurança empresarial](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-enterprise-security-package.png)

Para obter mais informações sobre a criação de HDInsight associado a um domínio do cluster, consulte [criar ambiente de sandbox de HDInsight associado a um domínio](./domain-joined/apache-domain-joined-configure.md).


## <a name="cluster-login-and-ssh-user-name"></a>Início de sessão do cluster e nome de utilizador SSH
Com os clusters do HDInsight, pode configurar duas contas de usuário durante a criação do cluster:

* Utilizador HTTP: O nome de utilizador predefinido é *administrador*. Ele usa a configuração básica no portal do Azure. Às vezes, isso é chamado de "Usuário do Cluster".
* Utilizador SSH (clusters do Linux): utilizado para ligar ao cluster através de SSH. Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

O pacote de segurança da empresa permite-lhe integrar o HDInsight no Active Directory e Apache Ranger. Vários utilizadores podem ser criados utilizando o pacote de segurança da empresa.

## <a name="location"></a>Localização (regiões) para armazenamento e os clusters

Não é necessário especificar explicitamente a localização do cluster: O cluster está na mesma localização como armazenamento predefinido. Para obter uma lista de regiões suportadas, clique nas **região** na lista pendente na [preços do HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Pontos finais de armazenamento para clusters

Embora uma instalação local do Hadoop utiliza o Hadoop Distributed File System (HDFS) para o armazenamento no cluster, na cloud é usar pontos de extremidade de armazenamento ligados ao cluster. Utilizam clusters do HDInsight [do Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) ou [blobs no armazenamento do Azure](hdinsight-hadoop-use-blob-storage.md). Utilizar o armazenamento do Azure ou o Data Lake Store, significa que pode eliminar em segurança os clusters do HDInsight utilizados para o cálculo ao mesmo tempo retendo os dados. 

> [!WARNING]
> Não é suportada através de uma conta de armazenamento adicional numa localização diferente do HDInsight cluster.

Durante a configuração, para o ponto final de armazenamento de predefinido Especifique um contentor de BLOBs de uma conta de armazenamento do Azure ou um Store de Lake de dados. O armazenamento predefinido contém o aplicativo e sistema de registos. Opcionalmente, pode especificar contas de armazenamento do Azure ligadas adicionais e contas de Data Lake Store que o cluster pode aceder. O cluster do HDInsight e as contas de armazenamento dependente tem de ser na mesma localização do Azure.

![As definições de armazenamento de cluster: pontos finais de armazenamento compatível com HDFS](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>Metastores opcional
Pode criar as metastores Hive ou Oozie opcionais. No entanto, nem todos os tipos de cluster suportam metastores e Azure SQL Data Warehouse não é compatível com metastores. 

Para obter mais informações, consulte [utilizar arquivos de metadados externos no Azure HDInsight](./hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]
> Quando cria um metastore personalizado, não utilize travessões, hífenes ou espaços no nome da base de dados. Isto pode fazer com que o processo de criação de cluster efetuar a ativação.

### <a name="use-hiveoozie-metastore"></a>Hive metastore

Se quiser manter suas tabelas do Hive, depois de eliminar um cluster do HDInsight, utilize um metastore personalizado. Em seguida, pode anexar o metastore para outro cluster de HDInsight.

Não é possível partilhar um metastore de HDInsight é criado para uma versão de cluster do HDInsight nas diferentes versões de cluster do HDInsight. Para obter uma lista das versões do HDInsight, consulte [versões suportadas HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie metastore

Para aumentar o desempenho ao utilizar o Oozie, utilize um metastore personalizado. Um metastore também pode fornecer acesso aos dados da tarefa de Oozie depois de eliminar o cluster. 

> [!IMPORTANT]
> Não é possível reutilizar um metastore Oozie personalizado. Para utilizar um metastore Oozie personalizado, tem de fornecer uma base de dados de SQL do Azure vazio ao criar o cluster do HDInsight.

## <a name="configure-cluster-size"></a>Configurar o tamanho do cluster

É faturado pela utilização de nó para, desde que o cluster existe. A faturação tem início quando um cluster é criado e termina quando o cluster é eliminado. Clusters não não possível remover a atribuição ou colocar em espera.


### <a name="number-of-nodes-for-each-cluster-type"></a>Número de nós para cada tipo de cluster
Cada tipo de cluster tem seu próprio número de nós, a terminologia para nós e o tamanho da VM predefinida. Na tabela a seguir, o número de nós para cada tipo de nó é parênteses.

| Tipo | Nós | Diagrama |
| --- | --- | --- |
| Hadoop |Nó principal (2), do nó de dados (1 +) |![Nós de cluster de Hadoop do HDInsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Servidor de cabeça (2), servidor de região (1 +), o nó de mestre/ZooKeeper (3) |![Nós de cluster do HBase do HDInsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nó nimbus (2), servidor de supervisor (1 +), o nó do ZooKeeper (3) |![Nós de cluster do HDInsight Storm](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Nó principal (2), o nó de trabalho (1 +), nós ZooKeeper (3) (gratuito para o tamanho da VM de ZooKeeper A1) |![Nós de cluster do HDInsight Spark](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Para obter mais informações, consulte [predefinido de tamanhos de máquina virtual e configuração de nó para clusters](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) em "Quais são os componentes do Hadoop e versões no HDInsight?"

O custo de clusters do HDInsight é determinado pelo número de nós e os tamanhos de máquinas virtuais para os nós. 

Tipos de clusters diferentes têm tipos de nó diferente, número de nós e tamanhos de nó:
* Predefinição de tipo de cluster do Hadoop: 
    * Dois *nós principais*  
    * Quatro *nós de dados*
* Predefinição de tipo de cluster do Storm: 
    * Dois *nós do Nimbus*
    * Três *nós ZooKeeper*
    * Quatro *nós de supervisor* 

Se estiver apenas a experimentar HDInsight, recomendamos que utilize um nó de dados. Para obter mais informações sobre os preços do HDInsight, consulte [preços do HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> O limite de tamanho de cluster varia de acordo com as subscrições do Azure. Contacte [suporte de faturação do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) para aumentar o limite.
>

Quando utiliza o portal do Azure para configurar o cluster, o tamanho do nó está disponível através da **escalões de preço de nó** painel. No portal, também pode ver o custo associado os tamanhos de nó diferente. 

![Tamanhos de nó de VM do HDInsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Tamanhos de máquinas virtuais 
Quando implementa clusters, escolha os recursos de computação com base na solução que pretende implementar. As VMs seguintes são utilizadas para clusters do HDInsight:
* A e VMs de série de D1-4: [tamanhos de VM do Linux General-purpose](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* VM da série de D11 14: [tamanhos de VM do Linux com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Para saber o que valor que deve utilizar para especificar um tamanho de VM ao criar um cluster com os SDKs diferentes ou ao utilizar o Azure PowerShell, consulte [tamanhos de VM a utilizar para clusters do HDInsight](../cloud-services/cloud-services-sizes-specs.md#size-tables). A partir deste artigo ligado, utilize o valor na **tamanho** coluna das tabelas.

> [!IMPORTANT]
> Se precisar de mais do que 32 nós de trabalho num cluster, tem de selecionar um tamanho de nó principal com, pelo menos, 8 núcleos e 14 GB de RAM.
>
>

Para obter mais informações, consulte [tamanhos de máquinas virtuais](../virtual-machines/windows/sizes.md). Para obter informações sobre os preços de vários tamanhos, veja [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Configuração de cluster personalizado
Compilações de configuração de cluster personalizado no rápido criar definições e adiciona as seguintes opções:
- [Aplicações do HDInsight](#hdinsight-applications)
- [Tamanho do cluster](#cluster-size)
- Definições avançadas
  - [Ações de script](#customize-clusters-using-script-action)
  - [Rede virtual](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Instalar aplicações do HDInsight em clusters

Uma aplicação HDInsight é uma aplicação que os utilizadores podem instalar num cluster do HDInsight baseado em Linux. Pode usar aplicativos fornecidos pela Microsoft, terceiros ou que desenvolva-se. Para obter mais informações, consulte [instalar aplicações do Hadoop de terceiros no Azure HDInsight](hdinsight-apps-install-applications.md).

A maioria dos aplicativos HDInsight é instalada num nó de extremidade em branco.  Um nó de extremidade vazio é uma máquina virtual do Linux com as mesmas ferramentas de cliente instalado e configurado como no nó principal. Pode utilizar o nó de extremidade para aceder ao cluster, testar as suas aplicações de cliente e hospedar seus aplicativos de cliente. Para obter mais informações, consulte [utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Definições avançadas: ações de Script

Pode instalar componentes adicionais ou personalizar a configuração de cluster através de scripts durante a criação. Esses scripts são invocados por meio **ação de Script**, que é uma opção de configuração que pode ser utilizada a partir do portal do Azure, cmdlets do PowerShell do Windows HDInsight ou o SDK de .NET do HDInsight. Para obter mais informações, consulte [cluster de HDInsight personalizar com ação de Script](hdinsight-hadoop-customize-cluster-linux.md).

Alguns componentes de Java nativos, como o Mahout e Cascading, podem ser executados no cluster como ficheiros de arquivo do Java (JAR). Estes ficheiros JAR podem ser distribuídos para o armazenamento do Azure e submetidos a clusters do HDInsight com mecanismos de submissão de tarefas do Hadoop. Para obter mais informações, consulte [Hadoop submeter tarefas programaticamente](hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Se tiver problemas com a implementação de ficheiros JAR a clusters do HDInsight, ou chamar ficheiros JAR em clusters do HDInsight, contacte [Support da Microsoft](https://azure.microsoft.com/support/options/).
>
> Em cascata, não é suportado pelo HDInsight e não é elegível para Support da Microsoft. Para listas de componentes suportados, consulte [quais são as novidades nas versões do cluster fornecidas pelo HDInsight](hdinsight-component-versioning.md).
>
>

Às vezes, pretende configurar os seguintes ficheiros de configuração durante o processo de criação:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Para obter mais informações, consulte [clusters do HDInsight personalizar com o arranque de](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Definições avançadas: Expandir clusters com uma rede virtual
Se sua solução necessitar de tecnologias que são distribuídas por vários tipos de cluster do HDInsight, um [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network) pode ligar-se os tipos de cluster necessários. Esta configuração permite que os clusters e qualquer código que implementar aos mesmos, para comunicar diretamente entre si.

Para obter mais informações sobre como utilizar uma rede virtual do Azure com o HDInsight, consulte [estender o HDInsight com redes virtuais do Azure](hdinsight-extend-hadoop-virtual-network.md).

Para obter um exemplo do uso de dois tipos de cluster dentro de uma rede virtual do Azure, veja [utilização Spark transmissão em fluxo estruturada com o Kafka](hdinsight-apache-kafka-spark-structured-streaming.md). Para obter mais informações sobre como utilizar o HDInsight com uma rede virtual, incluindo requisitos de configuração específicos para a rede virtual, consulte [capacidades de estender o HDInsight utilizando a rede Virtual do Azure](hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Resolver problemas de controlo de acesso

Caso se depare com problemas com a criação de clusters do HDInsight, veja [aceder aos requisitos de controlo](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Passos Seguintes

- [O que são o HDInsight, o ecossistema do Hadoop, e clusters do Hadoop?](hadoop/apache-hadoop-introduction.md)
- [Introdução à utilização do Hadoop no HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Trabalhar no Hadoop no HDInsight a partir de um PC do Windows](hdinsight-hadoop-windows-tools.md)
