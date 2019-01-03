---
title: Gerir clusters do Apache Hadoop baseado em Windows no HDInsight com o portal do Azure
description: Saiba como administrar o serviço do HDInsight. Criar um cluster do HDInsight, abra o console interativo do JavaScript e abra a consola de comandos do Apache Hadoop.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 4210528003cdb1f584bec3dea80c1aa1db2f86df
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632030"
---
# <a name="manage-windows-based-apache-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Gerir clusters do Apache Hadoop baseado em Windows no HDInsight com o portal do Azure

Utilizar o [portal do Azure][azure-portal], pode criar com base em Windows [Apache Hadoop](https://hadoop.apache.org/) clusters no Azure HDInsight, alteração de palavra-passe de utilizador do Hadoop e ativar protocolo RDP ( RDP) para que possa aceder a consola de comandos do Hadoop no cluster.

As informações neste artigo aplica-se apenas a clusters do HDInsight baseado em janela. Para obter informações sobre como gerir clusters baseados em Linux, consulte [Apache Hadoop gerir clusters no HDInsight com o portal do Azure](hdinsight-administer-use-portal-linux.md).

[!INCLUDE [windows-retirement-notice](../../includes/windows-retirement-notice.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este artigo, tem de ter o seguinte:

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Conta de armazenamento do Azure** -An HDInsight cluster utiliza um contentor de armazenamento de Blobs do Azure como o sistema de ficheiros predefinido. Para obter mais informações sobre como o armazenamento de Blobs do Azure fornece uma experiência totalmente integrada com os clusters do HDInsight, consulte [utilize armazenamento de Blobs do Azure com o HDInsight](hdinsight-hadoop-use-blob-storage.md). Para obter detalhes sobre como criar uma conta de armazenamento do Azure, consulte [como criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md).

## <a name="open-the-portal"></a>Abra o Portal
1. Inicie sessão em [https://portal.azure.com](https://portal.azure.com).
2. Depois de abrir o portal, pode:

   * Clique em **criar um recurso** no menu à esquerda para criar um novo cluster:

       ![novo botão de cluster do HDInsight](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Clique em **Clusters do HDInsight** no menu à esquerda.

       ![Botão de cluster HDInsight portal do Azure](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Se **HDInsight** não são apresentadas no menu à esquerda, clique em **procurar**.

     ![Botão de cluster procurar portal do Azure](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Criar clusters
Para as instruções de criação com o Portal, consulte [clusters do HDInsight criar](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight funciona com componentes de uma ampla gama de Apache Hadoop. Para obter a lista dos componentes que foram verificados e suportado, consulte [qual a versão do Apache Hadoop é no Azure HDInsight](hdinsight-component-versioning.md). Pode personalizar o HDInsight ao utilizar uma das seguintes opções:

* Utilize a ação de Script para executar scripts personalizados que podem personalizar um cluster para alterar a configuração de cluster ou instalar componentes personalizados, como o Giraph ou Solr. Para obter mais informações, consulte [cluster de HDInsight personalizar com ação de Script](hdinsight-hadoop-customize-cluster.md).
* Utilize os parâmetros de personalização do cluster no HDInsight .NET SDK ou do Azure PowerShell durante a criação do cluster. Estas alterações de configuração, em seguida, são mantidas por tempo de vida do cluster e não são afetadas por cluster nó recria a imagem que a plataforma Azure efetua periodicamente para manutenção. Para obter mais informações sobre como utilizar os parâmetros de personalização do cluster, consulte [clusters do HDInsight criar](hdinsight-hadoop-provision-linux-clusters.md).
* Alguns componentes nativos em Java, como [Apache Mahout](https://mahout.apache.org/) e [Cascading](https://www.cascading.org/), podem ser executadas no cluster, como ficheiros JAR. Estes ficheiros JAR podem ser distribuídos para o armazenamento de Blobs do Azure e submetidos para clusters do HDInsight através de mecanismos de submissão de tarefas do Hadoop. Para obter mais informações, consulte [Apache Hadoop submeter tarefas programaticamente](hadoop/submit-apache-hadoop-jobs-programmatically.md).

  > [!NOTE]  
  > Se tiver problemas com a implementação de ficheiros JAR a clusters do HDInsight ou chamar ficheiros JAR em clusters do HDInsight, contacte [Support da Microsoft](https://azure.microsoft.com/support/options/).
  >
  > Em cascata não é suportado pelo HDInsight e não é elegível para Support da Microsoft. Para listas de componentes suportados, consulte [quais são as novidades nas versões do cluster fornecidas pelo HDInsight](hdinsight-component-versioning.md).

Não é suportada a instalação de software personalizada no cluster, utilizando a conexão de área de trabalho remoto. Deve evitar armazenar todos os ficheiros em unidades do nó principal, como eles serão perdidos se precisar de voltar a criar os clusters. Recomendamos que o serviço de armazenamento de ficheiros no armazenamento de Blobs do Azure. Armazenamento de BLOBs é persistente.

## <a name="list-and-show-clusters"></a>Listar e Mostrar clusters
1. Inicie sessão em [https://portal.azure.com](https://portal.azure.com).
2. Clique em **Clusters do HDInsight** no menu à esquerda.
3. Clique no nome do cluster. Se a lista de cluster for longa, pode utilizar o filtro no topo da página.
4. Faça duplo clique num cluster a partir da lista para mostrar os detalhes.

    **Menu e essentials**:

    ![O Azure essentials de cluster do HDInsight portal](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * Para personalizar o menu, clique com botão direito em qualquer lugar no menu e, em seguida, clique em **personalizar**.
   * **As definições** e **todas as definições**: Apresenta o **definições** painel para o cluster, o que lhe permite aceder às informações detalhadas de configuração para o cluster.
   * **Dashboard**, **Dashboard do Cluster** e **URL**: Estas são todas as formas de aceder ao dashboard do cluster, o que é o Web do Ambari para clusters baseados em Linux.
   * **Secure Shell**: Mostra as instruções para ligar ao cluster através de ligação de Secure Shell (SSH).
   * **Dimensionar Cluster**: Permite-lhe alterar o número de nós de trabalho para este cluster.
   * **Eliminar**: Elimina o cluster.
   * **Início Rápido**: Apresenta informações que irão ajudá-lo a começar a utilizar o HDInsight.
   * **Os utilizadores**: Permite-lhe definir permissões para *portal management* deste cluster para outros utilizadores na sua subscrição do Azure.

     > [!IMPORTANT]  
     > Isso *apenas* afeta o acesso e permissões para este cluster no portal do Azure e não tem efeito em que pode ligar a ou submeter as tarefas para o cluster do HDInsight.

   * **Etiquetas**: As etiquetas permitem-lhe definir os pares de chave/valor para definir uma taxonomia personalizada dos seus serviços cloud. Por exemplo, pode criar uma chave denominada **projeto**e, em seguida, utilizar um valor para os serviços associados a um projeto específico.
   * **As vistas Ambari**: Fornece hiperligações para Web do Ambari.

     > [!IMPORTANT]  
     > Para gerir os serviços fornecidos pelo cluster do HDInsight, tem de utilizar a Web do Ambari ou a API de REST do Ambari. Para obter mais informações sobre a utilização do Ambari, consulte [clusters do HDInsight gerir com o Apache Ambari](hdinsight-hadoop-manage-ambari.md).

     **Utilização**:

     ![Utilização de cluster HDInsight portal do Azure](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Clique em **definições**.

    ![Utilização de cluster HDInsight portal do Azure](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Propriedades**: Ver as propriedades do cluster.
   * **Identidade do AAD do cluster**:
   * **Chaves de armazenamento do Azure**: Ver a conta de armazenamento predefinida e a respetiva chave. A conta de armazenamento é configuração durante o processo de criação do cluster.
   * **Início de sessão do cluster**: Altere o nome de utilizador do cluster HTTP e a palavra-passe.
   * **Metastores externas**: Ver os [Apache Hive](https://hive.apache.org/) e [Apache Oozie](https://oozie.apache.org/) metastores. As metastores só pode ser configurada durante o processo de criação do cluster.
   * **Dimensionar Cluster**: Aumentar e diminuir o número de nós de trabalho do cluster.
   * **Ambiente de trabalho remoto**: Ativar e desativar o acesso de (RDP) de área de trabalho remoto e configurar o nome de utilizador do RDP.  O nome de utilizador RDP tem de ser diferente do nome de utilizador HTTP.
   * **Parceiros de registo**:

     > [!NOTE]  
     > Isso é uma lista das funcionalidades disponíveis; nem todos estará presentes para todos os tipos de cluster.

6. Clique em **propriedades**:

    A secção de propriedades lista o seguinte:

   * **Nome de anfitrião**: Nome do cluster.
   * **URL do cluster**.
   * **Estado**: Incluir abortada, aceites, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, operacional, executar, erro, eliminar, eliminado, excedido, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **Região**: Localização do Azure. Para obter uma lista das localizações do Azure suportadas, consulte a **região** caixa de lista suspensa na [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Os dados criados**.
   * **Sistema operativo**: Qualquer um dos **Windows** ou **Linux**.
   * **Tipo de**: Hadoop, HBase, Storm, Spark.
   * **Versão**. Consulte [versões do HDInsight](hdinsight-component-versioning.md)
   * **Subscrição**: Nome da subscrição.
   * **ID de subscrição**.
   * **Origem de dados primária**. A conta de armazenamento de Blobs do Azure utilizada como predefinição sistema de ficheiros Hadoop.
   * **Escalão de preço de nós de trabalho**.
   * **Nó principal do escalão de preço**.

## <a name="delete-clusters"></a>Eliminar clusters
Eliminar um cluster não irá eliminar a conta de armazenamento predefinida ou as contas de armazenamento ligada. Pode voltar a criar o cluster ao utilizar as mesmas contas de armazenamento e o mesmo metastores.

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **eliminar** no menu superior e, em seguida, siga as instruções.

Consulte também [colocar em pausa/encerramento clusters](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Dimensionar clusters
O funcionalidade de dimensionamento do cluster permite-lhe alterar o número de nós de trabalho utilizado por um cluster que está em execução no Azure HDInsight sem ter de voltar a criar o cluster.

> [!NOTE]  
> Apenas clusters com o HDInsight versão 3.1.3 ou superior são suportadas. Se tiver a certeza de que a versão do seu cluster, consulte a página de propriedades.  Ver [listar e Mostrar clusters](#list-and-show-clusters).

O impacto de alterar o número de nós de dados para cada tipo de cluster suportada pelo HDInsight:

* Apache Hadoop

    Pode facilmente aumentar o número de nós de trabalho num cluster do Hadoop que está a ser executado sem afetar todas as tarefas em execução ou pendentes. Também podem ser submetidas a novas tarefas enquanto a operação estiver em curso. Falhas numa operação de dimensionamento são processadas corretamente para que o cluster está sempre deixado no estado funcional.

    Quando um cluster do Hadoop é reduzido, reduzindo o número de nós de dados, alguns dos serviços do cluster são reiniciados. Isso faz com que tudo em execução e tarefas pendentes a falhar após a conclusão da operação de dimensionamento. Pode, no entanto, volte a submeter as tarefas depois de concluída a operação.
* Apache HBase

    Pode facilmente adicionar ou remover nós ao cluster do HBase durante a execução. Servidores regionais são balanceadas automaticamente dentro de alguns minutos a concluir a operação de dimensionamento. No entanto, também manualmente pode balancear servidores regionais iniciando sessão no nó principal do cluster e executar os seguintes comandos a partir de uma janela de linha de comandos:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Para obter mais informações sobre como utilizar a shell de HBase, consulte]
* Apache Storm

    Pode facilmente adicionar ou remover nós de dados ao seu cluster do Storm, enquanto estiver em execução. Mas, após a conclusão bem-sucedida da operação de dimensionamento, terá de reequilibrar a topologia.

    Reequilíbrio pode ser feito de duas formas:

  * IU da web do Apache Storm
  * Ferramenta de interface de linha de comandos (CLI)

    Consulte a [documentação do Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) para obter mais detalhes.

    A IU da web de Storm está disponível no cluster do HDInsight:

    ![Reequilíbrio de dimensionamento do HDInsight storm](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Eis um exemplo como utilizar o comando da CLI para reequilibrar a topologia do Storm:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Dimensionar clusters**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **configurações** no menu superior e, em seguida, clique **Dimensionar Cluster**.
4. Introduza **nós de número de trabalho**. O limite no número de nó de cluster varia de acordo com as subscrições do Azure. Pode contactar o suporte de faturação para aumentar o limite.  As informações de custo irão refletir as alterações efetuadas para o número de nós.

    ![Dimensionamento do Storm de HBase do HDInsight Hadoop Spark](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Colocar em pausa/encerramento clusters
A maioria das tarefas do Hadoop é tarefas de lote que só são executaram ocasionalmente. Para a maioria dos clusters do Hadoop, há grandes períodos de tempo que o cluster não está a ser utilizado para processamento. Com o HDInsight, os dados são armazenados no Storage do Azure, pelo que pode eliminar um cluster em segurança quando este não está a ser utilizado.
Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados.

Existem várias formas, pode programar o processo:

* Utilizador do Azure Data Factory. Ver [serviço ligado do Azure HDInsight](../data-factory/compute-linked-services.md) e [transformar e analisar com o Azure Data Factory](../data-factory/transform-data.md) para HDInsight a pedido e Self-definido serviços ligados.
* Utilize o Azure PowerShell.  Ver [analisar dados de atraso de voo](hdinsight-analyze-flight-delay-data.md).
* Utilize a CLI clássica do Azure. Ver [clusters do HDInsight gerir com o CLI clássica do Azure](hdinsight-administer-use-command-line.md).
* Utilize o HDInsight .NET SDK. Ver [Apache Hadoop submeter tarefas](hadoop/submit-apache-hadoop-jobs-programmatically.md).

Para obter informações sobre preços, consulte [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/). Para eliminar um cluster a partir do Portal, veja [eliminar clusters](#delete-clusters)

## <a name="change-cluster-username"></a>Alterar nome de utilizador do cluster
Um cluster do HDInsight pode ter duas contas de usuário. A conta de utilizador de cluster do HDInsight é criada durante o processo de criação. Também pode criar uma conta de utilizador do RDP para aceder ao cluster através de RDP. Ver [ambiente de trabalho remoto permitem](#connect-to-hdinsight-clusters-by-using-rdp).

**Para alterar o nome de utilizador de cluster do HDInsight e a palavra-passe**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **configurações** no menu superior e, em seguida, clique **início de sessão do Cluster**.
4. Se **início de sessão do Cluster** tiver sido ativada, tem de clicar em **desativar**e, em seguida, clique em **ativar** antes de poder alterar o nome de utilizador e palavra-passe....
5. Alteração da **nome de início de sessão do Cluster** e/ou o **palavra-passe de início de sessão do Cluster**e, em seguida, clique em **guardar**.

    ![Utilizador de http de palavra-passe do cluster utilizador nome de utilizador de alterar do HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Conceder/revogar acesso
Clusters do HDInsight tem os seguintes serviços da web HTTP (todos estes serviços tem pontos de extremidade RESTful):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Por predefinição, estes serviços são concedidos para o acesso. Pode revogar/conceder o acesso do portal do Azure.

> [!NOTE]  
> Ao conceder/revogar o acesso irá repor o nome de utilizador do cluster e a palavra-passe.

**Para conceder/revogar acesso via web dos serviços HTTP**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **configurações** no menu superior e, em seguida, clique **início de sessão do Cluster**.
4. Se **início de sessão do Cluster** tiver sido ativada, tem de clicar em **desativar**e, em seguida, clique em **ativar** antes de poder alterar o nome de utilizador e palavra-passe....
5. Para **nome de utilizador de início de sessão de Cluster** e **palavra-passe de início de sessão do Cluster**, introduza o novo nome de utilizador e palavra-passe (respetivamente) para o cluster.
6. Clique em **GUARDAR**.

    ![HDInsight grand remover o acesso de serviço web de http](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a>Localizar a conta de armazenamento predefinida
Cada cluster do HDInsight tem uma conta de armazenamento predefinida. A conta de armazenamento predefinida e as chaves para um cluster é apresentado em **configurações**/**propriedades**/**chaves de armazenamento do Azure**. Ver [listar e Mostrar clusters](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Encontrar o grupo de recursos
No modo Azure Resource Manager, cada cluster do HDInsight é criado com um grupo de recursos do Azure. O grupo de recursos do Azure que pertence um cluster é apresentado no:

* A lista de cluster tem um **grupo de recursos** coluna.
* Cluster **essencial** mosaico.  

Ver [listar e Mostrar clusters](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Abra a consola do Query do HDInsight
A consola de consulta de HDInsight inclui as seguintes funcionalidades:

* **Hive Editor**: Uma interface de web de GUI para submeter tarefas do Hive.  Ver [consultas executar Apache Hive, utilizando a consola de consulta](hadoop/apache-hadoop-use-hive-query-console.md).

    ![Editor de portal de hive do HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **O histórico de tarefas**: Monitorize tarefas do Hadoop.  

    ![Histórico de tarefas de portal do HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Clique em **nome da consulta** para mostrar os detalhes incluindo as propriedades da tarefa **consulta da tarefa**, e * * resultado da tarefa. Também pode transferir a consulta e a saída para a estação de trabalho.
* **Browser de ficheiros**: Procure a conta de armazenamento predefinida e as contas de armazenamento ligada.

    ![Procura de browser de ficheiros de portal do HDInsight](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Na captura de ecrã, o **<Account>** tipo indica o item é uma conta de armazenamento do Azure.  Clique no nome de conta para procurar os ficheiros.
* **Interface do Usuário do Hadoop**.

    ![Interface do Usuário de Hadoop do HDInsight portal](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    De **IU do Hadoop*, pode procurar ficheiros e verifique os registos.
* **IU do yarn**.

    ![Portal de HDInsight IU do YARN](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Executar consultas do Hive
Para executar tarefas do Hive a partir do Portal, clique em **Editor do Hive** na consola de consulta do HDInsight. Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Monitorizar trabalhos
Para monitorizar trabalhos a partir do Portal, clique em **histórico de tarefas** na consola de consulta do HDInsight. Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

## <a name="browse-files"></a>Procurar ficheiros
Para procurar ficheiros armazenados na conta de armazenamento predefinida e as contas de armazenamento ligada, clique em **Browser de ficheiros** na consola de consulta do HDInsight. Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

Também pode utilizar o **procurar o sistema de ficheiros** utilitário a partir do **interface do Usuário do Hadoop** na consola do HDInsight.  Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>Monitorizar a utilização do cluster
O **utilização** seção do painel do cluster de HDInsight apresenta informações sobre o número de núcleos disponíveis para a sua subscrição para utilização com o HDInsight, bem como o número de núcleos alocados para este cluster e como eles são alocados para os nós dentro deste cluster. Ver [listar e Mostrar clusters](#list-and-show-clusters).

> [!IMPORTANT]  
> Para monitorizar os serviços fornecidos pelo cluster do HDInsight, tem de utilizar a Web do Ambari ou a API de REST do Ambari. Para obter mais informações sobre a utilização do Ambari, consulte [clusters do HDInsight gerir com o Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="open-hadoop-ui"></a>Abra a interface do Usuário do Hadoop
Para monitorizar o cluster, procure o sistema de arquivos e verifique os registos, clique em **IU do Hadoop** na consola de consulta do HDInsight. Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Abra a IU do Yarn
Para utilizar a interface de utilizador do Yarn, clique em **IU do Yarn** na consola de consulta do HDInsight. Ver [consola de consulta de HDInsight aberto](#open-hdinsight-query-console).

## <a name="connect-to-clusters-using-rdp"></a>Ligar a clusters através de RDP
As credenciais para o cluster que indicou durante a criação do seu conceder acesso aos serviços em cluster, mas não para o próprio cluster através do ambiente de trabalho remoto. Pode ativar o acesso de ambiente de trabalho remoto quando aprovisiona um cluster ou após aprovisionar um cluster. Para instruções sobre como ativar o ambiente de trabalho remoto durante a criação, consulte [cluster de HDInsight criar](hdinsight-hadoop-provision-linux-clusters.md).

**Para ativar o ambiente de trabalho remoto**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **configurações** no menu superior e, em seguida, clique **ambiente de trabalho remoto**.
4. Introduza **expira em**, **nome de utilizador do ambiente de trabalho remoto** e **palavra-passe do ambiente de trabalho remoto**e, em seguida, clique em **ativar**.

    ![HDInsight Ativar desativar configurar o ambiente de trabalho remoto](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    Os valores predefinidos para expira em é uma semana.

   > [!NOTE]  
   > Também pode utilizar o HDInsight .NET SDK para ativar o ambiente de trabalho remoto num cluster. Utilize o **EnableRdp** método no objeto de cliente do HDInsight, da seguinte forma: **cliente. EnableRdp (clustername, localização, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**. Da mesma forma, para desativar o ambiente de trabalho remoto no cluster, pode utilizar **cliente. DisableRdp (clustername, localização)**. Para obter mais informações sobre esses métodos, consulte [referência do SDK de .NET de HDInsight](https://go.microsoft.com/fwlink/?LinkId=529017). Isto é aplicável apenas para clusters do HDInsight em execução no Windows.

**Para ligar a um cluster através de RDP**

1. Inicie sessão para o [Portal][azure-portal].
2. Clique em **Procurar tudo** no menu à esquerda, clique em **Clusters do HDInsight**, clique no nome do cluster.
3. Clique em **configurações** no menu superior e, em seguida, clique **ambiente de trabalho remoto**.
4. Clique em **Connect** e siga as instruções. Se o Connect é desativar, tem de o ativar primeiro. Certifique-se utilizar o nome de utilizador do ambiente de trabalho remoto utilizador e palavra-passe.  Não é possível utilizar as credenciais de utilizador do Cluster.

## <a name="open-hadoop-command-line"></a>Abra a linha de comandos do Hadoop
Para ligar ao cluster com o ambiente de trabalho remoto e utilizar a linha de comandos do Hadoop, deve primeiro ativou a acesso de ambiente de trabalho remoto para o cluster, tal como descrito na secção anterior.

**Para abrir uma linha de comandos do Hadoop**

1. Ligar ao cluster através de ambiente de trabalho remoto.
2. Na área de trabalho, faça duplo clique em **linha de comandos do Hadoop**.

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Para obter mais informações sobre os comandos do Hadoop, consulte [referência de comandos do Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Captura de ecrã anterior, o nome da pasta tem o número de versão do Hadoop incorporado. O número de versão pode mudar com base na versão dos componentes do Hadoop instalada no cluster. Pode utilizar variáveis de ambiente de Hadoop para fazer referência a essas pastas. Por exemplo:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como criar um cluster do HDInsight com o Portal e como abrir a ferramenta de linha de comandos do Apache Hadoop. Para obter mais informações, consulte os artigos seguintes:

* [Administrar o HDInsight com o Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Administrar o HDInsight com a CLI clássica do Azure](hdinsight-administer-use-command-line.md)
* [Criar clusters do HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
* [Submeter tarefas do Apache Hadoop por meio de programação](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Introdução ao Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Qual a versão do Apache Hadoop é no Azure HDInsight?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Linha de comandos do Hadoop"
