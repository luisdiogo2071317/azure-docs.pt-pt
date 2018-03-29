---
title: Gerir o cluster de servidor R no HDInsight - Azure | Microsoft Docs
description: Saiba como gerir um cluster de servidor R no Azure HDInsight.
services: HDInsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: c0a996555e35a99a6025e92bcb41fa192b18eece
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-r-server-cluster-on-azure-hdinsight"></a>Gerir o cluster de servidor R no Azure HDInsight

Neste artigo, irá aprender a gerir um cluster de servidor R existente no Azure HDInsight para executar tarefas como adicionar utilizadores em simultâneo mulitiple, ligar remotamente a um cliente ou servidor R (Microsoft ML Server), a alteração do contexto de computação, etc.

## <a name="prerequisites"></a>Pré-requisitos

* **Um cluster de servidor R no HDInsight**: para obter instruções, consulte [começar com o servidor R no HDInsight](r-server-get-started.md).

* **Um cliente Secure Shell (SSH)**: é utilizado um cliente SSH para ligar remotamente ao cluster do HDInsight e executar comandos diretamente no mesmo. Para obter mais informações, consulte [utilizar o SSH com o HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="enable-multiple-concurrent-users"></a>Permitir vários utilizadores em simultâneo

Pode ativar vários utilizadores em simultâneo para o cluster de servidor R no HDInsight ao adicionar mais utilizadores para o nó de extremidade no qual a versão de Comunidade do RStudio é executada. Quando criar um cluster do HDInsight, tem de fornecer dois utilizadores -- um utilizador HTTP e outro SSH:

![Utilizador simultâneo 1](./media/r-server-hdinsight-manage/concurrent-users-1.png)

- **Nome de utilizador do cluster**: um utilizador HTTP utilizado para autenticação através do gateway do HDInsight que é utilizado para proteger os clusters do HDInsight que criou. Este utilizador HTTP é utilizado para aceder à IU do Ambari, à IU do YARN, bem como a outros componentes da IU.
- **Nome de utilizador Secure Shell (SSH)**: um utilizador SSH para aceder ao cluster através de secure shell. Este utilizador é um utilizador no sistema Linux para todos os nós principais, nós de trabalho e nós de extremidade. Desta forma, pode utilizar o secure shell para aceder a qualquer nó num cluster remoto.

A versão de Comunidade do R Studio servidor utilizada no cluster de servidor R no HDInsight aceita apenas o nome de utilizador de Linux e a palavra-passe como um mecanismo de início de sessão. Não suporta a transmissão de tokens. Por isso, quando tentar aceder R Studio pela primeira vez num cluster do servidor R, terá de início de sessão duas vezes.

- Primeiro inicie sessão com as credenciais de utilizador HTTP através do Gateway do HDInsight. 

- Em seguida, utilize as credenciais de utilizador do SSH para iniciar sessão RStudio.
  
Atualmente, só é possível criar uma conta de utilizador SSH quando são aprovisionados clusters do HDInsight. Por isso, para ativar vários utilizadores a aceder ao cluster de servidor R no HDInsight, tem de criar outros utilizadores no sistema Linux.

Uma vez RStudio é executado no nó de extremidade do cluster, existem vários passos aqui:

1. Utilizar o utilizador do SSH existente para iniciar sessão nó de extremidade
2. Adicionar mais utilizadores do Linux ao nó de extremidade
3. Utilizar a versão de comunidade do RStudio com o utilizador criado

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Passo 1: Iniciar sessão no nó de extremidade com o utilizador SSH criado

Siga as instruções apresentadas em [ligar para o HDInsight (Hadoop) através do SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) para aceder ao nó de extremidade. O endereço do nó de extremidade para o cluster de servidor R no HDInsight é `CLUSTERNAME-ed-ssh.azurehdinsight.net`.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>Passo 2: Adicionar mais utilizadores do Linux ao nó de extremidade

Para adicionar um utilizador ao nó de extremidade, execute os comandos:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user 
    sudo passwd <yournewusername>

A seguinte captura de ecrã mostra as saídas.

![Utilizador simultâneo 3](./media/r-server-hdinsight-manage/concurrent-users-2.png)

Quando for pedida a “Palavra-passe de Kerberos atual:”, basta premir **Enter** para ignorar. A opção `-m` no comando `useradd` indica que o sistema vai criar uma pasta raiz para o utilizador, que é necessária na versão de comunidade do RStudio.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Passo 3: Utilizar a versão de comunidade do RStudio com o utilizador criado

Aceder RStudio de https://CLUSTERNAME.azurehdinsight.net/rstudio/. Se estão a iniciar sessão pela primeira vez depois de criar o cluster, introduza as credenciais de administrador de cluster seguidas as credenciais de utilizador SSH que acabou de criar. Apenas se não for o primeiro início de sessão, introduza as credenciais para o utilizador SSH que criou.

Também pode iniciar sessão com as credenciais originais (por predefinição, é o *sshuser*) simultaneamente a partir de outra janela do browser.

Repare também que os utilizadores adicionados recentemente não têm privilégios de raiz no sistema Linux, mas têm acesso a todos os ficheiros no armazenamento remoto do HDFS e do WASB.

## <a name="connect-remotely-to-microsoft-ml-server-or-client"></a>Ligar remotamente à Microsoft ML Server ou o cliente

Pode configurar acesso no contexto de computação do Spark do HDInsight Hadoop, de uma instância remota do Microsoft ML Server ou o cliente do ML da Microsoft em execução no seu ambiente de trabalho. Para tal, tem de especificar as opções (hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches e sshProfileScript) quando definir o RxSpark contexto no ambiente de trabalho de computação: por exemplo:

    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )

Para obter mais informações, consulte a secção "Utilizando o Microsoft R Server como um cliente Hadoop" [criar um contexto de computação para Spark](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios)

## <a name="use-a-compute-context"></a>Utilizar um contexto de cálculo

O contexto de cálculo permite-lhe controlar se a computação é feita localmente no nó de extremidade ou distribuída pelos nós do cluster do HDInsight.

1. A partir do RStudio Server ou da consola de R (numa sessão SSH), utilize o código seguinte para carregar dados de exemplo para o armazenamento predefinido do HDInsight:

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
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

        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Em seguida, criar algumas informações de dados e definir duas origens de dados para que iremos possam funcionar com os dados.

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Execute um regressão logística da sobre os dados utilizando o contexto de computação local.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Deverá ver um resultado que acaba com linhas semelhantes às seguintes:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Execute o regressão logística da mesma, utilizando o contexto de Spark. O contexto do Spark distribui o processamento por todos os nós de trabalho do cluster do HDInsight.

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Também pode utilizar o MapReduce para distribuir a computação por vários nós dos cluster. Para obter mais informações sobre o contexto de cálculo, veja [Compute context options for R Server on HDInsight (Opções do contexto de cálculo para o R Server no HDInsight)](r-server-compute-contexts.md).


## <a name="distribute-r-code-to-multiple-nodes"></a>Distribuir o código R por vários nós

Com o servidor R no HDInsight, pode executar código do R existente e executá-la em vários nós no cluster utilizando `rxExec`. Esta função é útil para fazer varrimentos ou simulações de parâmetros. O código abaixo é um exemplo de como utilizar `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Se ainda estiver a utilizar o contexto do Spark ou do MapReduce, este comando devolve o valor nodename para os nós de trabalho em que o código `(Sys.info()["nodename"])` é executado. Por exemplo, num cluster de quatro nós, é de esperar que vai receber um resultado semelhante ao seguinte:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="access-data-in-hive-and-parquet"></a>Aceder a dados no Hive e no Parquet

Está disponível uma funcionalidade no R Server 9.1 que permite acesso direto a dados no Hive e no Parquet, para utilização por funções ScaleR no contexto de cálculo do Spark. Estas capacidades estão disponíveis através de novas funções de origem de dados de ScaleR chamadas RxHiveData e RxParquetData, as quais funcionam com a utilização do Spark SQL para carregar dados diretamente para um DataFrame do Spark para análise por parte do ScaleR.  

O código abaixo disponibiliza alguns códigos de exemplos de utilização das funções novas:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Para informações adicionais sobre utilização destas novas funções, consulte a ajuda online no servidor de ML através da utilização do `?RxHivedata` e `?RxParquetData` comandos.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Instalar pacotes de R adicionais no cluster

### <a name="to-install-r-packages-on-the-edge-node"></a>Para instalar pacotes de R no nó de extremidade

Se pretender instalar pacotes de R adicionais no nó de extremidade, pode utilizar `install.packages()` diretamente a partir do consola do R, uma vez ligado ao nó de extremidade através de SSH. 

### <a name="to-install-r-packages-on-the-worker-node"></a>Para instalar pacotes de R no nó de trabalho

Para instalar pacotes de R em nós de trabalho do cluster, tem de utilizar uma ação de Script. As Ações de Script são scripts de Bash utilizadas para fazer alterações de configuração ao cluster do HDInsight ou para instalar mais software, como pacotes de R adicionais. 

> [!IMPORTANT]
> Só é possível utilizar as Ações de Script para instalar pacotes de R adicionais depois de o cluster ter sido criado. Não utilize este procedimento durante a criação do cluster, pois o script depende de o R Server estar completamente instalado e configurado.
>
>

1. Siga os passos indicados em [personalizar clusters através da ação de Script](../hdinsight-hadoop-customize-cluster-linux.md).

3. Para **submeter ação de script**, forneça as seguintes informações:

   * Para **tipo de Script**, selecione **personalizada**.

   * Para **nome**, forneça um nome para a ação de script.

    * Para **Bash script URI**, introduza `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`. Este é o script que instala os pacotes de R adicionais no nó de trabalho

   * Selecione a caixa de verificação só **trabalho**.

   * **Parâmetros**: os pacotes de R que vão ser instalados. Por exemplo, `bitops stringr arules`

   * Selecione a caixa de verificação para **manter esta ação de script**.  

   > [!NOTE]
   > 1. Por predefinição, todos os pacotes de R são instalados a partir de um instantâneo do repositório Microsoft MRAN consistente com a versão do R Server que foi instalada. Se quiser instalar versões novas dos pacotes, existe algum risco de incompatibilidades. Contudo, este tipo de instalação é possível mediante a especificação de `useCRAN` como o primeiro elemento da lista do pacote, como, por exemplo, `useCRAN bitops, stringr, arules`.  
   > 2. Alguns pacotes de R requerem bibliotecas do sistema Linux adicionais. Para sua comodidade, pré-instalámos as dependências de que os cem pacotes de R mais populares precisam. No entanto, se o pacote ou pacotes de R que instalar requeiram mais bibliotecas para além destas, tem de transferir o script de base utilizado aqui e adicionar passos para instalar as bibliotecas do sistema. Em seguida, tem de carregar o script modificado para um contentor de blobs público no armazenamento do Azure e utilizá-lo para instalar os pacotes.
   >    Para obter mais informações sobre como programar Ações de Script, veja [Script Action development (Programação de Ações de Script)](../hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Adicionar uma ação de script](./media/r-server-hdinsight-manage/submitscriptaction.png)

4. Selecione **Criar** para executar o script. Após a conclusão do script, os pacotes de R estão disponíveis em todos os nós de trabalho.

## <a name="next-steps"></a>Passos Seguintes

* [Operacionalizar o cluster de servidor R no HDInsight](r-server-operationalize.md)
* [Opções de contexto do cluster de servidor R no HDInsight de computação](r-server-compute-contexts.md)
* [Opções de armazenamento do Azure para o cluster de servidor R no HDInsight](r-server-storage.md)
