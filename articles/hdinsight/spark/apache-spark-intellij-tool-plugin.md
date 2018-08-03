---
title: 'Azure Toolkit para IntelliJ: criar Spark aplicações para um cluster do HDInsight | Documentos da Microsoft'
description: Utilizar o Azure Toolkit para IntelliJ para desenvolver aplicações do Spark escritas no Scala e enviá-los para um cluster do Spark do HDInsight.
services: hdinsight
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/25/2017
ms.author: maxluk
ms.openlocfilehash: 891a568af7892048eb84646acbf495f32ddd00b2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427426"
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Utilizar o Azure Toolkit para IntelliJ para criar Spark aplicações para um cluster do HDInsight

Utilizar o Azure Toolkit para IntelliJ Plug-in para desenvolver aplicações do Spark escritas no Scala e depois submetê-las para um cluster do Spark do HDInsight diretamente a partir do ambiente de desenvolvimento integrado (IDE) de IntelliJ. Pode utilizar o plug-in de diversas formas:

* Programe e envie um aplicação Scala Spark num cluster do HDInsight Spark.
* Aceder aos seus recursos de cluster do Azure HDInsight Spark.
* Desenvolva e execute uma aplicação Scala Spark localmente.

Para criar o seu projeto, veja a [criar aplicações do Spark com o Azure Toolkit para IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) vídeo.

> [!IMPORTANT]
> Pode utilizar este plug-in para criar e submeter aplicações apenas para um cluster do Spark do HDInsight no Linux.
> 

## <a name="prerequisites"></a>Pré-requisitos

- Um cluster do Apache Spark no HDInsight Linux. Para obter instruções, veja [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).
- Oracle Java Development Kit. Pode instalá-lo do [Web site do Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEIA. Este artigo utiliza a versão 2017.1. Pode instalá-lo do [Web site da JetBrains](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Instalar o Azure Toolkit para IntelliJ
Para obter instruções de instalação, consulte [instalar o Azure Toolkit para IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="get-started"></a>Introdução
O utilizador pode optar por [inicie sessão na subscrição do Azure](#sign-in-to-your-azure-subscription), ou [ligar um cluster do HDInsight](#link-a-cluster) com o Ambari nome de utilizador/palavra-passe ou domínio associado a um credential para iniciar.


## <a name="sign-in-to-your-azure-subscription"></a>Inicie sessão na sua subscrição do Azure

1. Iniciar o IDE IntelliJ e abra o Explorador do Azure. Sobre o **View** menu, selecione **ferramenta Windows**e, em seguida, selecione **Explorador do Azure**.
       
   ![A ligação Explorador do Azure](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

1. Com o botão direito a **Azure** nó e, em seguida, selecione **sessão**.

1. Na **do Azure de sessão** caixa de diálogo, selecione **iniciar sessão**e, em seguida, introduza as credenciais do Azure.

    ![A caixa de diálogo início de sessão no Azure](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

1. Depois de iniciar sessão, o **selecionar subscrições** caixa de diálogo apresenta uma lista de todas as subscrições do Azure que estão associadas com as credenciais. Selecione o **selecione** botão.

    ![A caixa de diálogo Selecionar subscrições](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

1. Sobre o **Azure Explorer** separador, expanda **HDInsight** para ver os clusters do Spark do HDInsight que estão na sua subscrição.
   
    ![Clusters do Spark do HDInsight no Explorador do Azure](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

1. Para ver os recursos (por exemplo, contas de armazenamento) que estão associados com o cluster, pode expandir ainda mais um nó de nome do cluster.
   
    ![Um nó expandido do nome do cluster](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Ligar um cluster
Pode ligar um cluster de HDInsight normal usando o nome de utilizador do Ambari gerido. Da mesma forma, para um cluster de HDInsight associado a um domínio, pode ligar com o domínio e o nome de utilizador, como user1@contoso.com.

1. Selecione **ligar um cluster** partir **Explorador do Azure**.

   ![menu de contexto do cluster de ligação](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)


1. Introduza **nome do Cluster**, **nome de utilizador** e **palavra-passe**. Precisa verificar o nome de utilizador e palavra-passe se a falha de autenticação. Opcionalmente, adicione a conta de armazenamento, a chave de armazenamento, em seguida, selecione um contentor do contentor de armazenamento. Informações de armazenamento são para o Explorador de armazenamento na árvore à esquerda
   
   ![caixa de diálogo de cluster de ligação](./media/apache-spark-intellij-tool-plugin/link-a-cluster-dialog.png)

   > [!NOTE]
   > Usamos a chave de armazenamento ligada, o nome de utilizador e a palavra-passe se o cluster tanto registado na subscrição do Azure e ligadas um cluster.
   > ![Explorador de armazenamento no IntelliJ](./media/apache-spark-intellij-tool-plugin/storage-explorer-in-IntelliJ.png)

   
1. Pode ver um cluster de ligado no **HDInsight** nó se as informações de entrada estão corretas. Agora pode enviar uma aplicação a este cluster ligado.

   ![cluster ligado](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

1. Também pode desassociar um cluster a partir **Explorador do Azure**.
   
   ![Desassociar de cluster](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Executar uma aplicação Spark Scala num cluster do HDInsight Spark

1. Inicie o IntelliJ IDEA e, em seguida, crie um projeto. Na caixa de diálogo **Novo Projeto**, faça o seguinte: 

   a. Selecione **HDInsight** > **Spark no HDInsight (Scala)**.

   b. Na lista **Ferramenta de compilação**, selecione uma das seguintes opções, de acordo com as suas necessidades:

      * **Maven**, para o suporte de assistente de criação do projeto Scala
      * **SBT**, para gerir as dependências e a complicação do projeto Scala

    ![Caixa de diálogo O Novo Projeto](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

1. Selecione **Seguinte**.

1. O assistente de criação do projeto Scala deteta automaticamente se instalou o plug-in do Scala. Selecione **Instalar**.

   ![Verificação do Plug-in do Scala](./media/apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

1. Para transferir o Plug-in do Scala, selecione **OK**. Siga as instruções para reiniciar o IntelliJ. 

   ![A caixa de diálogo de instalação do plug-in do Scala](./media/apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

1. Na janela **Novo Projeto**, faça o seguinte:  

    ![Selecionar o SDK do Spark](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Introduza um nome do projeto e localização.

   b. Na lista pendente **SDK do Projeto**, selecione **Java 1.8** para o cluster do Spark 2.x ou selecione **Java 1.7** para o cluster do Spark 1.x.

   c. Na lista pendente **versão do Spark**, o assistente de criação do projeto Scala integra a versão correta para o SDK do Spark e o SDK do Scala. Se a versão do cluster do Spark for anterior à 2.0, selecione **Spark 1.x**. Caso contrário, selecione **Spark2.x**. Este exemplo utiliza o **Spark 2.0.2 (Scala 2.11.8)**.

1. Selecione **Concluir**.

1. O projeto de Spark cria automaticamente um artefacto por si. Para ver o artefacto, faça o seguinte:

   a. Sobre o **arquivo** menu, selecione **estrutura do projeto**.

   b. Na **estrutura do projeto** caixa de diálogo, selecione **artefactos** para ver o artefacto de predefinição que é criado. Também pode criar seu próprio artefacto selecionando o sinal de adição (**+**).

      ![Informações de artefacto na caixa de diálogo](./media/apache-spark-intellij-tool-plugin/default-artifact.png)
      
1. Adicione o código de origem da aplicação, fazendo o seguinte:

   a. No Explorador de projeto, clique com botão direito **src**, aponte para **New**e, em seguida, selecione **Scala classe**.
      
      ![Comandos para criar uma classe Scala a partir do Explorador de projeto](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. Na **criar uma nova classe de Scala** caixa de diálogo caixa, forneça um nome, selecione **objeto** no **tipo** caixa e, em seguida, selecione **OK**.
      
      ![Criar caixa de diálogo nova classe Scala](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. Na **MyClusterApp.scala** de ficheiros, cole o seguinte código. O código lê os dados de Hvac (disponível em todos os clusters do HDInsight Spark), devolve as linhas que têm apenas um dígito na coluna sétima no ficheiro CSV e grava a saída para **/HVACOut** sob o contentor de armazenamento predefinido para o cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

1. Execute a aplicação num cluster do HDInsight Spark, fazendo o seguinte:

   a. No Explorador de projeto, faça duplo clique no nome do projeto e, em seguida, selecione **submeter a aplicação Spark para HDInsight**.
      
      ![A aplicação de Spark submeter ao comando do HDInsight](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. São-lhe pedido que introduza as credenciais de subscrição do Azure. Na **Spark submissão** caixa de diálogo, forneça os valores seguintes e, em seguida, selecione **submeter**.
      
      * Para **(apenas Linux) de clusters do Spark**, selecione o cluster do Spark do HDInsight no qual pretende executar a sua aplicação.

      * Selecione um artefato do projeto IntelliJ ou selecione um na unidade de disco rígido.

      * Na **nome da classe principal** caixa, selecione as reticências (**...** ), selecione a classe principal no código de origem da aplicação e, em seguida, selecione **OK**.

        ![A caixa de diálogo Selecionar classe de principal](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Uma vez que o código da aplicação neste exemplo não necessita de argumentos da linha de comandos ou referenciar JARs ou ficheiros, pode deixar as restantes caixas vazio. Depois de fornecer todas as informações, a caixa de diálogo deve assemelhar-se a imagem seguinte.
        
        ![A caixa de diálogo de envio do Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. O **Spark submissão** separador na parte inferior da janela deve começar a exibir o progresso. Também pode parar a aplicação ao selecionar o botão vermelho no **Spark submissão** janela.
      
     ![A janela de envio do Spark](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      Para saber como acessar o resultado da tarefa, consulte o "acesso e gerir clusters do HDInsight Spark com o Azure Toolkit para IntelliJ" seção mais adiante neste artigo.

## <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Depurar aplicações do Spark localmente ou remotamente num cluster do HDInsight 
Recomendamos também que outra forma de submeter a aplicação Spark no cluster. Pode fazê-lo ao definir os parâmetros nos **configurações de execução/depuração** IDE. Para obter mais informações, consulte [depurar aplicações do Spark localmente ou remotamente num cluster do HDInsight com o Azure Toolkit para IntelliJ através de SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).



## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Aceder e gerir clusters do HDInsight Spark com o Azure Toolkit para IntelliJ
Pode executar várias operações, utilizando o Azure Toolkit para IntelliJ.

### <a name="access-the-job-view"></a>Aceda à vista de tarefa
1. No Explorador do Azure, expanda **HDInsight**, expanda o nome de cluster do Spark e, em seguida, selecione **tarefas**.  

    ![Nó de vista de tarefa](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

1. No painel da direita, o **vista de tarefas do Spark** separador apresenta todos os aplicativos que foram executados no cluster. Selecione o nome da aplicação para o qual pretende ver mais detalhes.

    ![Detalhes da aplicação](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)
    >Nota
    >

1. Para apresentar informações básicas de tarefa em execução, Paire o rato sobre o gráfico da tarefa. Para ver o gráfico de fases e as informações que gera todas as tarefas, selecione um nó no gráfico da tarefa.

    ![Detalhes da fase de tarefa](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

1. Para ver frequentemente utilizados registos, como *Driver Stderr*, *Stdout do controlador*, e *informações de diretório*, selecione o **Log** separador.

    ![Detalhes do registo](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

1. Também pode ver o histórico de Spark da interface do Usuário e a IU do YARN (no nível do aplicativo) ao selecionar um link na parte superior da janela.

### <a name="access-the-spark-history-server"></a>Aceder ao servidor de histórico do Spark
1. No Explorador do Azure, expanda **HDInsight**, o nome do cluster do Spark com o botão direito e, em seguida, selecione **IU do histórico do Spark Open**. 

1. Quando lhe for pedido, introduza as credenciais de administrador do cluster, o que especificou quando configurou o cluster.

1. No dashboard de servidor do histórico do Spark, pode utilizar o nome da aplicação para procurar a aplicação que já concluiu apenas em execução. No código anterior, define o nome do aplicativo usando `val conf = new SparkConf().setAppName("MyClusterApp")`. Por conseguinte, é o nome da sua aplicação de Spark **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Iniciar o portal do Ambari
1. No Explorador do Azure, expanda **HDInsight**, o nome do cluster do Spark com o botão direito e, em seguida, selecione **abrir Portal de gestão de Cluster (Ambari)**. 

1. Quando lhe for pedido, introduza as credenciais de administrador para o cluster. Estas credenciais que especificou durante o processo de configuração de cluster.

### <a name="manage-azure-subscriptions"></a>Gerir subscrições do Azure
Por predefinição, o Azure Toolkit para IntelliJ lista os clusters do Spark de todas as suas subscrições do Azure. Se necessário, pode especificar as subscrições que pretende aceder. 

1. No Explorador do Azure, com o botão direito a **Azure** nó raiz e, em seguida, selecione **gerir subscrições**. 

1. Na caixa de diálogo, desmarque as caixas de verificação junto as subscrições que não pretende aceder e, em seguida, selecione **fechar**. Também pode selecionar **terminar sessão** se de que pretende terminar a sua subscrição do Azure.

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Converter aplicações existentes do IntelliJ IDEA para utilizar o Azure Toolkit para IntelliJ
Pode converter o Scala Spark existentes que criou no IntelliJ IDEA para ser compatível com o Azure Toolkit para IntelliJ de aplicativos. Pode, em seguida, utilizar o plug-in para submeter os aplicativos a um cluster do Spark do HDInsight.

1. Para uma aplicação Spark Scala existente que foi criada por meio do IntelliJ IDEA, abra o ficheiro de .iml associados.

1. Na raiz nível é uma **módulo** elemento semelhante ao seguinte:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Edite o elemento para adicionar `UniqueKey="HDInsightTool"` , para que o **módulo** elemento é semelhante ao seguinte:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Guarde as alterações. A aplicação deverá estar agora compatível com o Azure Toolkit para IntelliJ. Pode testá-lo clicando com o nome do projeto no Explorador de projeto. O menu de pop-up tem agora a opção **submeter a aplicação Spark para HDInsight**.

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Erro na execução local: *utilize um tamanho de heap maior*
No Spark 1.6, se estiver a utilizar um SDK de Java de 32 bits durante a execução local, pode encontrar os seguintes erros:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Estes erros acontecem porque o tamanho de heap não é suficientemente grande para o Spark executar. Spark requer, pelo menos, 471 MB. (Para obter mais informações, consulte [SPARK 12081](https://issues.apache.org/jira/browse/SPARK-12081).) Uma solução simple é usar um SDK de Java de 64 bits. Também pode alterar as definições de JVM no IntelliJ, adicionando as seguintes opções:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Adicionar opções para a caixa "Opções de VM" no IntelliJ](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>FAQ
Quando ligar um cluster, sugiro que forneça a credencial de armazenamento.

![Ligar o cluster, forneça a credencial de armazenamento](./media/apache-spark-intellij-tool-plugin/link-cluster-with-storage-credential-intellij.png)

Existem dois modos para submeter as tarefas. Se a credencial de armazenamento for fornecida, modo de lote será utilizado para submeter a tarefa. Caso contrário, será utilizado modo interativo. Se o cluster estiver ocupado, poderá receber o erro abaixo.

![Erro de obtenção de Intellij quando ocupado do cluster](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-upload.png)

![Erro de obtenção de Intellij quando ocupado do cluster](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-submit.png)

## <a name="feedback-and-known-issues"></a>Comentários e problemas conhecidos
Atualmente, a visualização de saídas de Spark diretamente não é suportada.

Se tiver comentários ou sugestões, ou se tiver quaisquer problemas ao utilizar este plug-in, envie um e-mail para hdivstool@microsoft.com.

## <a name="seealso"></a>Passos seguintes
* [Descrição geral: Apache Spark no Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demonstração
* Criar projeto de Scala (vídeo): [criar aplicações do Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Depuração remota (vídeo): [utilizar o Azure Toolkit para IntelliJ depurar aplicações do Spark remotamente num Cluster do HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Cenários
* [Spark com BI: efetuar análise de dados interativa com o Spark no HDInsight com ferramentas de BI](apache-spark-use-bi-tools.md)
* [Spark com Machine Learning: utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de registos de sites com o Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Criar e executar aplicações
* [Criar uma aplicação autónoma com o Scala](apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Utilizar ferramentas do HDInsight para IntelliJ com a Sandbox da Hortonworks](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Utilizar ferramentas do HDInsight no Azure Toolkit para Eclipse para criar Spark aplicações](apache-spark-eclipse-tool-plugin.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Gerir recursos
* [Gerir recursos para o cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](apache-spark-job-debugging.md)
