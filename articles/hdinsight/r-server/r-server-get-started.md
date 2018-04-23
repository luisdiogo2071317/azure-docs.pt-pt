---
title: Introdução ao R Server no HDInsight - Azure | Microsoft Docs
description: Saiba como criar um Apache Spark num cluster do HDInsight que inclui o R Server e submeter um script R no cluster.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: d6910ab257312626ca25126721410edeed6cdeae
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="get-started-with-r-server-cluster-on-azure-hdinsight"></a>Introdução ao cluster do R Server no Azure HDInsight

O Azure HDInsight inclui uma opção para integrar o R Server no seu cluster do HDInsight. Esta opção permite que os scripts R utilizem o Spark e o MapReduce para executar cálculos distribuídos. Neste artigo, vai aprender a criar um R Server no cluster do HDInsight. Depois, vai ficar a saber como executar um script R que demonstra como utilizar o Spark para computações de R distribuídas.


## <a name="prerequisites"></a>Pré-requisitos

* **Uma subscrição do Azure**: antes de começar este tutorial, tem de ter uma subscrição do Azure. Para obter mais informações, veja [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Obter a avaliação gratuita do Microsoft Azure).
* **Um cliente Secure Shell (SSH)**: é utilizado um cliente SSH para ligar remotamente ao cluster do HDInsight e executar comandos diretamente no mesmo. Para obter mais informações, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Criar o cluster com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Clique em **Criar um recurso** > **Dados + Análise** > **HDInsight**.

3. Em **Informações Básicas**, introduza as seguintes informações:

    * **Nome do Cluster**: o nome do cluster HDInsight.
    * **Subscrição**: selecione a subscrição que pretende utilizar.
    * **Nome de utilizador de início de sessão do cluster** e **Palavra-passe de início de sessão do cluster**: o início de sessão ao aceder ao cluster através de HTTPS. Utilize estas credenciais para aceder aos serviços, como a IU Web do Ambari ou a API REST.
    * **Nome de utilizador do Secure Shell (SSH)**: o início de sessão utilizado ao aceder ao cluster através de SSH. Por predefinição, a palavra-passe é igual à palavra-passe de início de sessão do cluster.
    * **Grupo de Recursos**: o grupo de recursos em que o cluster é criado.
    * **Localização**: a região do Azure em que o cluster é criado.

        ![Detalhes básicos do cluster](./media/r-server-get-started/clustername.png)

4. Selecione **Tipo de cluster**e, em seguida, defina os seguintes valores na secção **Configuração de cluster**:

    * **Tipo de Cluster**: R Server

    * **Sistema operativo**: Linux

    * **Versão**: R Server 9.1 (HDI 3.6). Estão disponíveis no [docs.microsoft.com](https://docs.microsoft.com/machine-learning-server/whats-new-in-r-server#r-server-91) as notas de versão das versões do R Server disponíveis.

    * **R Studio Community Edition para R Server**: este IDE baseado no browser está instalado por predefinição no nó de extremidade. Desmarque a caixa de verificação, se preferir não instalá-lo. Se optar por instalar, o URL para aceder ao início de sessão do RStudio Server está disponível num painel de aplicação do portal do seu cluster, depois de ser criado.

        ![Detalhes básicos do cluster](./media/r-server-get-started/clustertypeconfig.png)

4. Depois de selecionar o tipo de cluster, utilize o botão __Selecionar__ para definir o tipo de cluster. Em seguida, utilize o botão __Seguinte__ para concluir a configuração básica.

5. Na secção **Armazenamento**, selecione ou crie uma Conta de armazenamento. Para seguir os passos neste documento, deixe os outros campos nesta secção com os valores predefinidos. Utilize o botão __Seguinte__ para guardar a configuração do armazenamento.

    ![Configurar as definições de conta de armazenamento do HDInsight](./media/r-server-get-started/cluster-storage.png)

6. Na secção **Resumo**, reveja a configuração do cluster. Utilize as ligações __Editar__ para alterar quaisquer definições que estejam incorretas. Por fim, clique no botão __Criar__ para criar o cluster.

    ![Configurar as definições de conta de armazenamento do HDInsight](./media/r-server-get-started/clustersummary.png)

    > [!NOTE]
    > A criação do cluster pode demorar até 20 minutos.

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Ligar ao RStudio Server

Se optar por instalar o R Studio Server Community Edition como parte do cluster do HDInsight, pode aceder ao início de sessão do RStudio com um dos seguintes dois métodos:

* **Opção 1** - aceda ao URL seguinte (em que **CLUSTERNAME** é o nome do cluster do R Server que criou):

        https://CLUSTERNAME.azurehdinsight.net/rstudio/

* **Opção 2** - abra o cluster do R Server no Portal do Azure, em **Ligações rápidas** clique em **Dashboards do R Server**.

     ![Configurar as definições de conta de armazenamento do HDInsight](./media/r-server-get-started/dashboard-quick-links.png)

    Em **Dashboards de Clusters**, clique em **R Studio Server**.

    ![Configurar as definições de conta de armazenamento do HDInsight](./media/r-server-get-started/r-studio-server-dashboard.png)

   > [!IMPORTANT]
   > Independentemente do método utilizado, vai ter de se autenticar duas vezes da primeira vez que iniciar sessão.  No primeiro aviso de autenticação, indique o *userID* e a *palavra-passe de administrador do cluster*. No segundo aviso de autenticação, indique o *userID* e a *palavra-passe de administrador do SSH*. Os inícios de sessão seguintes só precisam das credenciais de SSH.

Assim que estiver ligado, o ecrã deve assemelhar-se à captura de ecrã seguinte:

![Ligar ao RStudio](./media/r-server-get-started/connect-to-r-studio.png)

## <a name="run-a-sample-job"></a>Executar uma tarefa de exemplo

Pode submeter uma tarefa através das funções ScaleR. Eis um exemplo dos comandos utilizados para executar uma tarefa:

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-cluster-edge-node"></a>Ligar ao nó periférico do cluster

Nesta secção, saiba como ligar ao nó periférico de um cluster HDInsight do R Server através de SSH. Para familiaridade sobre a utilização de SSH, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

O comando SSH para ligar ao nó periférico do cluster do R Server é:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

Para localizar o comando SSH para o cluster, no Portal do Azure, clique no nome do cluster, clique em **Início de sessão de SSH + Cluster** e, em seguida, em **Hostname**, selecione o nó periférico. São apresentadas as informações do Ponto Final de SSH relativas ao nó de extremidade.

![Imagem do Ponto Final de SSH do nó de extremidade](./media/r-server-get-started/sshendpoint.png)

Se utilizou uma palavra-passe para proteger a sua conta de utilizador do SSH, é solicitado que a introduza. Se utilizou uma chave pública, poderá ter de utilizar o parâmetro `-i` para especificar a chave privada correspondente. Por exemplo:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Quando estiver ligado, vai deparar-se com uma mensagem semelhante à seguinte:

    sshuser@ed00-myrclu:~$

<a name="use-r-console"></a>
## <a name="use-the-r-server-console"></a>Utilizar a consola do R Server

1. Na sessão SSH, utilize o comando seguinte para iniciar a consola do R:  

        R

2. Deverá ver um resultado com a versão do R Server, além de outras informações.
    
3. Na linha de comandos `>`, pode introduzir o código R. O R Server no HDInsight inclui pacotes que lhe permitem interagir facilmente com o Hadoop e executar cálculos distribuídos. Por exemplo, utilize o comando seguinte para ver a raiz do sistema de ficheiros predefinido do cluster do HDInsight:

        rxHadoopListFiles("/")

4. Também pode utilizar o endereçamento de estilo WASB.

        rxHadoopListFiles("wasb:///")

5. Para sair da consola do R, utilize o seguinte comando:

        quit()

## <a name="automated-cluster-creation"></a>Criação de clusters automatizada

Pode automatizar a criação de um cluster do R Server para HDInsight, ao utilizar modelos do Azure Resource Manager, o SDK e o PowerShell.

* Para criar um cluster do R Server com um modelo do Azure Resource Manager, veja [Implementar um cluster do HDInsight no R Server](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* Para criar um cluster do R Server com o SDK do .NET, veja [Criar clusters baseados em Linux no HDInsight com o SDK do .NET.](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* Para criar um cluster do R Server com o PowerShell, consulte o artigo sobre [Criar clusters do HDInsight com o Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md).

## <a name="delete-the-cluster"></a>Eliminar o cluster

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Resolução de problemas

Caso se depare com problemas com a criação de clusters do HDInsight, veja [aceder aos requisitos de controlo](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Passos seguintes

Neste artigo, aprendeu a criar um novo cluster do R Server no Azure HDInsight e as noções básicas de utilização da consola do R numa sessão SSH. Os artigos seguintes explicam outras maneiras de gerir e trabalhar com o R Server no HDInsight:

* [Submeter tarefas a partir das Ferramentas R para o Visual Studio](r-server-submit-jobs-r-tools-vs.md)
* [Gerir um cluster do R Server no HDInsight](r-server-hdinsight-manage.md)
* [Operacionalizar o cluster do R Server no HDInsight](r-server-operationalize.md)
* [Opções do contexto de cálculo para o cluster do R Server no HDInsight](r-server-compute-contexts.md)
* [Opções do Armazenamento do Azure para o cluster do R Server no HDInsight](r-server-storage.md)
