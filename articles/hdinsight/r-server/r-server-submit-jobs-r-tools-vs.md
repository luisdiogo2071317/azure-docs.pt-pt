---
title: "Submeter tarefas de R ferramentas do Visual Studio – Azure HDInsight | Microsoft Docs"
description: Submeta tarefas de R do seu computador local do Visual Studio para um cluster do HDInsight.
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 1a82ba7790f739768156a8bee33a74d7130e24e1
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Submeter tarefas de R ferramentas para o Visual Studio

[R Tools para Visual Studio](https://www.visualstudio.com/vs/rtvs/) (RTVS) é uma extensão gratuita de open source para a Comunidade (gratuita), Professional, edições e Enterprise de ambos [Visual Studio 2017](https://www.visualstudio.com/downloads/), e [Visual Studio 2015 Update 3](http://go.microsoft.com/fwlink/?LinkId=691129) ou superior.

RTVS melhora o fluxo de trabalho de R ao oferta ferramentas como o [janela R interativa](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL), (conclusão de código), o intellisense [desenhar visualização](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) através de bibliotecas R como ggplot2 e ggviz, [Depuração de código do R](https://docs.microsoft.com/visualstudio/rtvs/debugging)e muito mais.

## <a name="set-up-your-environment"></a>Configurar o ambiente

1. Instalar [R Tools para Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation).

    ![Instalar RTVS no Visual Studio 2017](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Selecione o *ciência de dados e aplicações analíticas* carga de trabalho, em seguida, selecione o **suporte de idiomas de R**, **suporte de tempo de execução para o desenvolvimento de R**, e  **Cliente de R Microsoft** opções.

3. Tem de ter chaves públicas e privadas para autenticação de SSH.
<!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. Instalar [R Server](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) no seu computador. R Server fornece o [ `RevoScaleR` ](https://msdn.microsoft.com/microsoft-r/scaler/scaler) e `RxSpark` funções.

5. Instalar [PuTTY](http://www.putty.org/) para fornecer um contexto de computação para executar `RevoScaleR` funções do seu cliente local ao cluster do HDInsight.

6. Tem a opção para aplicar as definições de ciência de dados ao seu ambiente do Visual Studio que fornece um novo esquema para a sua área de trabalho para as ferramentas de R.
    1. Para guardar as definições atuais do Visual Studio, utilize o **ferramentas > importar e exportar definições** comando, em seguida, selecione **exportação selecionadas definições de ambiente** e especifique um nome de ficheiro. Para restaurar essas definições, utilize o comando mesmo e selecione **selecionado de importar as definições do ambiente**.

    2. Vá para o **R ferramentas** menu item, em seguida, selecione **definições de ciência de dados...** .

        ![Definições de ciência de dados...](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

    > [!NOTE]
    > Utilizar a abordagem no passo 1, também pode guardar e restaurar a esquema de scientist dados personalizados, em vez de repetir a **definições de ciência de dados** comando.

## <a name="execute-local-r-methods"></a>Executar métodos de R locais

1. Criar o [cluster do HDInsight de servidor R](r-server-get-started.md).
2. Instalar o [extensão RTVS](https://docs.microsoft.com/visualstudio/rtvs/installation).
3. Transferir o [amostras zip ficheiro](https://github.com/Microsoft/RTVS-docs/archive/master.zip).
4. Abra `examples/Examples.sln` para iniciar a solução no Visual Studio.
5. Abra o `1-Getting Started with R.R` ficheiros o `A first look at R` pasta de solução.
6. A iniciar na parte superior do ficheiro, prima Ctrl + Enter enviar a cada linha, um de cada vez, para a janela de R interativo. Algumas linhas poderão demorar algum tempo, como a instalação de pacotes.
    * Em alternativa, pode selecionar todas as linhas no ficheiro de R (Ctrl + A), em seguida, executar todos os (Ctrl + Enter) ou selecione o ícone executar interativo na barra de ferramentas.
        ![Executar interativa](./media/r-server-submit-jobs-r-tools-vs/execute-interactive.png)

7. Depois de executar todas as linhas no script, deverá ver um resultado semelhante a isto:

    ![Definições de ciência de dados...](./media/r-server-submit-jobs-r-tools-vs/workspace.png)

## <a name="submit-jobs-to-an-hdinsight-r-cluster"></a>Submeter as tarefas para um cluster do HDInsight R

Utilizar um cliente do Microsoft R Microsoft/servidor R num computador Windows equipados com PuTTY, pode criar um contexto de computação que será executado distribuído `RevoScaleR` funções do seu cliente local ao cluster do HDInsight. Utilize `RxSpark` para criar o contexto de computação, especificando o nome de utilizador, o cluster de Hadoop nó de extremidade, comutadores SSH e assim sucessivamente.

1. Para localizar o nome de anfitrião do nó de extremidade, abra o painel do cluster de HDInsight R no Azure, em seguida, selecione **Secure Shell (SSH)** no menu superior do painel de descrição geral.

    ![Secure Shell (SSH)](./media/r-server-submit-jobs-r-tools-vs/ssh.png)

2. Copiar o **nome de anfitrião do nó de extremidade** valor.

    ![Nome de anfitrião do nó periférico](./media/r-server-submit-jobs-r-tools-vs/edge-node.png)

3. Cole o seguinte código para a janela do R interativo no Visual Studio, alterar os valores das variáveis de configuração para corresponder ao seu ambiente.

    ```R
    # Setup variables that connect the compute context to your HDInsight cluster
    mySshHostname <- 'r-cluster-ed-ssh.azurehdinsight.net ' # HDI secure shell hostname
    mySshUsername <- 'sshuser' # HDI SSH username
    mySshClientDir <- "C:\\Program Files (x86)\\PuTTY"
    mySshSwitches <- '-i C:\\Users\\azureuser\\r.ppk' # Path to your private ssh key
    myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep = "/")
    myShareDir <- paste("/var/RevoShare", mySshUsername, sep = "/")
    mySshProfileScript <- "/usr/lib64/microsoft-r/3.3/hadoop/RevoHadoopEnvVars.site"

    # Create the Spark Cluster compute context
    mySparkCluster <- RxSpark(
          sshUsername = mySshUsername,
      sshHostname = mySshHostname,
      sshSwitches = mySshSwitches,
      sshProfileScript = mySshProfileScript,
      consoleOutput = TRUE,
      hdfsShareDir = myHdfsShareDir,
      shareDir = myShareDir,
      sshClientDir = mySshClientDir
    )
    
    # Set the current compute context as the Spark compute context defined above
    rxSetComputeContext(mySparkCluster)
    ```
    
    ![Definir o contexto do Spark](./media/r-server-submit-jobs-r-tools-vs/spark-context.png)

4. Execute os seguintes comandos na janela do R interativo:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Deverá ver um resultado semelhante ao seguinte:

    ![Execução de comando de rx com êxito](./media/r-server-submit-jobs-r-tools-vs/rx-commands.png)

5. Certifique-se de que o `rxHadoopCopy` copiados com êxito o `people.json` ficheiros da pasta de dados de exemplo para recém-criada `/user/RevoShare/newUser` pasta:

    1. A partir do painel de cluster do HDInsight R no Azure, selecione **contas do Storage** no menu esquerdo.

        ![Contas de armazenamento](./media/r-server-submit-jobs-r-tools-vs/storage-accounts.png)

    2. Selecione a conta de armazenamento predefinido para o cluster, de tomar nota do nome do contentor/diretório.

    3. Selecione **contentores** no menu da esquerda no seu painel de conta de armazenamento.

        ![Contentores](./media/r-server-submit-jobs-r-tools-vs/containers.png)

    4. Selecione o nome do contentor do cluster, navegue para o **utilizador** pasta (poderá ter de clicar em *carregar mais* na parte inferior da lista), em seguida, selecione *RevoShare*, em seguida, **newUser**. O `people.json` ficheiro deve ser apresentado no `newUser` pasta.

        ![Ficheiro copiado](./media/r-server-submit-jobs-r-tools-vs/copied-file.png)

6. Depois de terminar com o contexto actual do Spark, tem de o interromper. Não é possível executar vários contextos de uma só vez.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Passos Seguintes

* [Compute context options for R Server on HDInsight (Opções do contexto de cálculo para o R Server no HDInsight)](r-server-compute-contexts.md)
* [Combinar ScaleR e SparkR](../hdinsight-hadoop-r-scaler-sparkr.md) fornece um exemplo de predições de atraso do companhia aérea voo.
<!-- * You can also submit R jobs with the [R Studio Server](hdinsight-submit-jobs-from-r-studio-server.md) -->
