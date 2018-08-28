---
title: Criar clusters do Hadoop usando um navegador da web - Azure HDInsight
description: Saiba como criar clusters do Hadoop, HBase, Storm ou Spark no Linux para o HDInsight com um navegador da web e o portal de pré-visualização do Azure.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: 77f4b8e8826dab014b81fdb6847755630ac44508
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43104947"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Criar clusters baseados em Linux no HDInsight com o portal do Azure
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

O portal do Azure é uma ferramenta de gestão baseado na web para serviços e recursos alojados na cloud do Microsoft Azure. Neste artigo, saiba como criar clusters do HDInsight baseado em Linux através do portal.

## <a name="prerequisites"></a>Pré-requisitos
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Um navegador da web modernos**. O portal do Azure utiliza o HTML5 e Javascript e pode não funcionar corretamente em navegadores mais antigos.

## <a name="create-clusters"></a>Criar clusters
O portal do Azure expõe a maior parte das propriedades do cluster. Utilizar o modelo Azure Resource Manager, pode ocultar muitos detalhes. Para obter mais informações, consulte [clusters do Hadoop de baseados em criar Linux no HDInsight utilizando modelos Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Clique em **+**, clique em **inteligência + análise**e, em seguida, clique em **HDInsight**.
   
    ![Criar um novo cluster no portal do Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "criar um novo cluster no portal do Azure")

3. Na **HDInsight** painel, clique em **personalizado (tamanho, definições, aplicações)**, clique em **Noções básicas**e, em seguida, introduza as seguintes informações.

    ![Criar um novo cluster no portal do Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "criar um novo cluster no portal do Azure")

    * Introduza o **Nome do Cluster**: este nome tem de ser globalmente exclusivo.

    * Partir do **subscrição** pendente, selecione a subscrição do Azure que é utilizada para o cluster.

    * Clique em **tipo de Cluster**e, em seguida, selecione o tipo de cluster (Hadoop, Spark, etc.) que pretende criar. Para **sistema operativo**, clique em **Linux** e, em seguida, selecione uma versão. Utilize a versão predefinida se não souber o que escolher. Para obter mais informações, veja [HDInsight cluster versions (Versões de clusters HDInsight)](hdinsight-component-versioning.md).

        Para tipos de cluster do Hadoop, Spark e Interactive Query, também pode optar por instalar o **Enterprise Security Package**. Enterprise Security Package permite que os recursos de segurança, como a integração do Azure Active Directory e Apache Ranger para os clusters. Para obter mais informações, consulte [Enterprise Security Package no Azure HDInsight](./domain-joined/apache-domain-joined-introduction.md).

        ![Ativar o Enterprise Security Package](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-enable-enterprise-security-package.png "ativar Enterprise Security Package")
     
        > [!IMPORTANT]
        > HDInsight clusters são fornecidos numa variedade de tipos, que correspondem à carga de trabalho ou tecnologia concebido para o cluster. Não existe nenhum método suportado para criar um cluster que combina vários tipos, como o Storm e HBase num cluster. 
        > 
        > 
        
    * Para **nome de utilizador de início de sessão de Cluster** e **palavra-passe de início de sessão do Cluster**, forneça o nome de utilizador e palavra-passe para o utilizador administrador.

    * Introduza um **nome de utilizador SSH** e, se quiser que a palavra-passe SSH mesma que a palavra-passe de administrador que especificou anteriormente, selecione a **utilizar a mesma palavra-passe como início de sessão do cluster** caixa de verificação. Se não estiver, fornecer uma **palavra-passe** ou **chave pública**, que será utilizado para autenticar o utilizador SSH. A abordagem recomendada é utilizar uma chave pública. Clique em **Selecionar**, na parte inferior, para guardar a configuração das credenciais.
   
    Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Em **Grupo de recursos**, especifique se pretende criar um novo grupo de recursos ou utilizar um existente.

    * Especifique um Datacenter **localização** onde o cluster ser criado.

    * Clique em **Seguinte**.

4. Para **armazenamento**, especifique se pretende o armazenamento do Azure (WASB) ou o armazenamento do Data Lake como o armazenamento predefinido. Veja a tabela abaixo para obter mais informações.

    ![Criar um novo cluster no portal do Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "criar um novo cluster no portal do Azure")

    | Armazenamento                                      | Descrição |
    |----------------------------------------------|-------------|
    | **Blobs de armazenamento do Azure como armazenamento predefinido**   | <ul><li>Para **tipo de armazenamento primário**, selecione **armazenamento do Azure**. Depois disso, para **método de seleção**, pode escolher **minhas assinaturas** se pretender especificar uma conta de armazenamento que faz parte da sua subscrição do Azure e, em seguida, selecione a conta de armazenamento. Caso contrário, clique em **chave de acesso** e forneça as informações para a conta de armazenamento que pretende que escolha a partir de fora da sua subscrição do Azure.</li><li>Para **contentor predefinido**, pode optar por vão, com o nome de contentor predefinido sugerido pelo portal ou especificar a sua própria.</li><li>Se estiver a utilizar WASB como armazenamento predefinido, pode (opcionalmente) clicar **contas de armazenamento adicionais** especificar contas de armazenamento adicional para associar o cluster. Para **chaves de armazenamento do Azure**, clique em **adicionar uma chave de armazenamento**, e, em seguida, pode fornecer uma conta de armazenamento das suas subscrições do Azure ou de outras subscrições (fornecendo a chave de acesso da conta de armazenamento).</li><li>Se estiver a utilizar WASB como armazenamento predefinido, pode (opcionalmente) clicar **acesso do Data Lake Store** para especificar o armazenamento do Azure Data Lake como armazenamento adicional. Para obter mais informações, consulte [início rápido: configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | **Armazenamento do Azure Data Lake como armazenamento predefinido** | Para **tipo de armazenamento primário**, selecione **Gen1 de armazenamento do Azure Data Lake** ou **Gen2 de armazenamento do Azure Data Lake (pré-visualização)** e, em seguida, veja o artigo [início rápido : Configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) para obter instruções. |
    | **Metastores externas**                      | Opcionalmente, pode especificar uma base de dados SQL para guardar os metadados do Hive e Oozie associado ao cluster. Para **Selecione uma base de dados do SQL para Hive** Selecione uma base de dados SQL e, em seguida, forneça o nome de utilizador/palavra-passe para a base de dados. Repita estes passos para os metadados do Oozie.<br><br>Algumas considerações ao utilizar a base de dados SQL do Azure para metastores. <ul><li>A base de dados SQL do Azure utilizada para o metastore têm de permitir conectividade a outros serviços do Azure, incluindo o Azure HDInsight. No dashboard de base de dados SQL do Azure, no lado direito, clique no nome do servidor. Este é o servidor no qual a instância de base de dados SQL está em execução. Assim que estiver na vista de servidores, clique em **configurar**e, em seguida, para **dos serviços do Azure**, clique em **Sim**e, em seguida, clique em **guardar**.</li><li>Ao criar um metastore, não use um nome de base de dados que contenha barras ou hífenes, pois isso pode fazer com que o processo de criação de cluster efetuar a ativação.</li></ul> |

    Clique em **Seguinte**. 

    > [!WARNING]
    > Não é suportado utilizar uma conta de armazenamento adicional numa localização diferente do cluster do HDInsight.

5. Opcionalmente, clique em **aplicativos** para instalar aplicações que funcionam com clusters do HDInsight. Estas aplicações podem ser desenvolvidas pela Microsoft, por fornecedores independentes de software (ISV) ou por si. Para obter mais informações, consulte [aplicações do HDInsight instalar](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Clique em **tamanho do Cluster** para apresentar informações sobre os nós que são utilizados para este cluster. Defina o número de nós de trabalho que precisa para o cluster. Também é mostrado o custo estimado de execução do cluster.
   
    ![Escalões de preço de nó](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "especificar o número de nós de cluster")
   
   > [!IMPORTANT]
   > Se pretender mais do que 32 nós de trabalho, durante a criação de cluster ou ao dimensionar o cluster após a criação, tem de selecionar um tamanho de nó principal com, pelo menos, 8 núcleos e 14 GB de RAM.
   > 
   > Para obter mais informações sobre tamanhos de nós e custos associados, veja os [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Clique em **seguinte** para guardar o configuração do preço do nó.

7. Clique em **definições avançadas** para configurar outras definições opcionais, como o uso **ações de Script** para personalizar um cluster para instalar componentes personalizados ou ao associar um **deredeVirtual**. Veja a tabela abaixo para obter mais informações.

    ![Escalões de preço de nó](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "especificar o número de nós de cluster")

    | Opção | Descrição |
    |--------|-------------|
    | **Ações de script** | Utilize esta opção se pretender utilizar um script personalizado para personalizar um cluster, como o cluster está a ser criado. Para obter mais informações sobre as ações de script, consulte [HDInsight personalizar clusters com ação de Script](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Rede Virtual** | Selecione uma rede virtual do Azure e a sub-rede, se quiser colocar o cluster numa rede virtual. Para obter informações sobre como utilizar o HDInsight com uma rede Virtual, incluindo requisitos de configuração específicos para a rede Virtual, consulte [capacidades de estender o HDInsight, com uma rede Virtual do Azure](hdinsight-extend-hadoop-virtual-network.md). |

    Clique em **Seguinte**.

8. Para **resumo**, verifique as informações que introduziu anteriormente e, em seguida, clique em **criar**.

    ![Escalões de preço de nó](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "especificar o número de nós de cluster")
    
    > [!NOTE]
    > Demora algum tempo para o cluster ser criado, normalmente, cerca de 15 minutos. Utilize o mosaico no Startboard, ou o **notificações** entrada à esquerda da página para verificar o processo de aprovisionamento.
    > 
    > 
12. Assim que concluir o processo de criação, clique no mosaico para o cluster a partir do Startboard. A janela do cluster fornece as seguintes informações.
    
    ![Interface de clusters](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "propriedades do Cluster")
    
    Utilize o seguinte para compreender os ícones na parte superior.
    
    * **Descrição geral** separador fornece todas as informações essenciais sobre o cluster, como o nome, o grupo de recursos a que pertence, a localização, o sistema operativo, o URL para o dashboard do cluster, etc.
    * **Dashboard** direciona-o para o portal de Ambari associado ao cluster.
    * **Secure Shell**: informações necessárias para aceder ao cluster através de SSH.
    * **Dimensionar cluster** permite que aumenta o número de nós de trabalho associada ao cluster.
    * **Eliminar**: elimina o cluster do HDInsight.
    

## <a name="customize-clusters"></a>Personalizar clusters
* Ver [clusters do HDInsight personalizar com o arranque de](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Ver [HDInsight personalizar clusters com ação de Script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Eliminar o cluster
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Resolução de problemas

Caso se depare com problemas com a criação de clusters do HDInsight, veja [aceder aos requisitos de controlo](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Passos Seguintes
Agora que criou com êxito um cluster do HDInsight, utilize o seguinte para aprender a trabalhar com o seu cluster:

### <a name="hadoop-clusters"></a>Clusters do Hadoop
* [Utilizar o Hive com o HDInsight](hadoop/hdinsight-use-hive.md)
* [Utilizar o Pig com o HDInsight](hadoop/hdinsight-use-pig.md)
* [Utilizar o MapReduce com o HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Clusters de HBase
* [Introdução ao HBase no HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Desenvolver aplicações de Java para o HBase no HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Clusters do Storm
* [Desenvolver topologias de Java para Storm no HDInsight](storm/apache-storm-develop-java-topology.md)
* [Utilizar componentes de Python no Storm no HDInsight](storm/apache-storm-develop-python-topology.md)
* [Implementar e monitorizar topologias Storm no HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Clusters do Spark
* [Criar uma aplicação autónoma com o Scala](spark/apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](spark/apache-spark-livy-rest-interface.md)
* [Spark com BI: Efetuar uma análise de dados interativa com o Spark no HDInsight com ferramentas do BI](spark/apache-spark-use-bi-tools.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](spark/apache-spark-machine-learning-mllib-ipython.md)

