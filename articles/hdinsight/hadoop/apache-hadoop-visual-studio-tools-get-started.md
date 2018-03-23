---
title: Ligar ao Azure HDInsight com as Ferramentas do Data Lake para Visual Studio | Microsoft Docs
description: Saiba como instalar e utilizar o Data Lake Tools para Visual Studio para se ligar a clusters do Hadoop no Azure HDInsight e, em seguida, executar consultas do Hive.
keywords: ferramentas do hadoop,consulta do hive,visual studio, visual studio hadoop
services: HDInsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: afd40d75bb9c5fd3170a4da215925244994d7749
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-hive-queries"></a>Utilize o Data Lake Tools para o Visual Studio ligar ao Azure HDInsight e executar consultas do Hive

Saiba como utilizar o Data Lake Tools para Visual Studio (também conhecidas como Ferramentas do Azure Data Lake e do Stream Analytics para o Visual Studio) para ligar a clusters do Hadoop no [Azure HDInsight](../hdinsight-hadoop-introduction.md) e submeter consultas do Hive. 

Para obter mais informações sobre a utilização do HDInsight, veja [Introduction to HDInsight](../hdinsight-hadoop-introduction.md) (Introdução ao HDInsight) e [Get started with HDInsight ](apache-hadoop-linux-tutorial-get-started.md) (Começar a utilizar o HDInsight). 

Para obter mais informações sobre a ligação a um cluster do Storm, veja [Develop C# topologies for Apache Storm on HDInsight using Visual Studio (Desenvolver topologias C# para o Apache Storm no HDInsight com o Visual Studio)](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

Pode utilizar o Data Lake Tools para Visual Studio para aceder ao Azure Data Lake Analytics e ao HDInsight. Para obter informações sobre o Data Lake Tools, veja [Desenvolver scripts U-SQL com o Data Lake Tools para Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial e utilizar o Data Lake Tools para Visual Studio, precisa do seguinte:

* Um cluster do Azure HDInsight. Para criar um cluster do Azure HDInsight, veja [Introdução à utilização do Hadoop no Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Para executar consultas interativas do Hive, precisa de um cluster do [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).
* Um computador que tenha o Visual Studio 2017, 2015 ou 2013 instalado.
    
    > [!NOTE]
    > Atualmente, está disponível apenas a versão em inglês do Data Lake Tools para Visual Studio.
    > 
    > 

## <a name="install-or-update-data-lake-tools-for-visual-studio"></a>Instalar ou atualizar o Data Lake Tools para Visual Studio

### <a name="install-data-lake-tools"></a>Instalar Data Lake Tools

O Data Lake Tools está instalado por predefinição no Visual Studio 2017. Para versões anteriores do Visual Studio, pode instalar o Data Lake Tools com o [Instalador de Plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx). Escolha a versão do Data Lake Tools que corresponde à sua versão do Visual Studio. 

### <a name="install-visual-studio"></a>Instalar o Visual Studio

Se não tiver o Visual Studio instalado, utilize o [Instalador de Plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx) para instalar as versões mais recentes do Visual Studio Community e o SDK do Azure:

![Captura de ecrã do Instalador de Plataforma Web do Data Lake Tools para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Utilizar o Instalador de Plataforma Web para instalar o Data Lake Tools para Visual Studio")

### <a name="update-the-tools"></a>Atualizar as ferramentas

1. Abra o Visual Studio.
2. No menu **Ferramentas**, selecione **Extensões e atualizações**.
3. Expanda **Atualizações** e, em seguida, selecione **Ferramentas do Azure Data Lake e do Stream Analytics**, (se estiver instalado).

> [!NOTE]
>
> Pode utilizar apenas o Data Lake Tools versão 2.3.0.0 ou posterior para ligar a clusters da Consulta Interativa e executar consultas interativas do Hive.

## <a name="connect-to-azure-subscriptions"></a>Ligar a subscrições do Azure
Pode utilizar o Data Lake Tools para Visual Studio para se ligar aos seus clusters do HDInsight, realizar algumas operações de gestão básicas e executar consultas do Hive.

> [!NOTE]
> Para obter informações sobre a ligação a um cluster do Hadoop genérico, veja [Escrever e submeter consultas do Hive com o Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx).
> 
> 

Para ligar à sua subscrição do Azure:

1. Abra o Visual Studio.
2. No menu **Ver**, selecione **Explorador de Servidores**.
3. No Explorador de Servidores, expanda **Azure** e, em seguida, expanda **HDInsight**.
   
   > [!NOTE]
   > A janela **Lista de Tarefas do HDInsight** deve ser apresentada. Se não vir a janela, no menu **Ver**, selecione **Outras Janelas** e, em seguida, selecione **Janela da Lista de Tarefas do HDInsight**.  
   > 
   > 
4. Introduza as credenciais da sua subscrição do Azure e selecione **Iniciar Sessão**. A autenticação só é precisa se nunca se tiver ligado à subscrição do Azure a partir do Visual Studio neste computador.
5. No Explorador de Servidores, é apresentada uma lista dos clusters do HDInsight existentes. Se não tiver clusters, pode criar um através do Portal do Azure, do Azure PowerShell ou do SDK do HDInsight. Para obter mais informações, veja [Create HDInsight clusters](../hdinsight-hadoop-provision-linux-clusters.md) (Criar clusters do HDInsight).
   
   ![Captura de ecrã da lista de clusters do Data Lake Tools para Visual Studio no Explorador de Servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Lista de clusters do Data Lake Tools para Visual Studio no Explorador de Servidores")
6. Expanda um cluster do HDInsight. São apresentadas **Bases de Dados do Hive**, uma conta de armazenamento predefinida, contas de armazenamento ligadas e o **registo do serviço do Hadoop**. Pode expandir ainda mais as entidades.

Depois de se ligar à sua subscrição do Azure, pode fazer as tarefas seguintes.

Ligar-se ao portal do Azure a partir do Visual Studio:

1. No Explorador de Servidores, selecione **Azure** > **HDInsight**.
2. Clique com o botão direito do rato num cluster do HDInsight e, em seguida, selecione **Gerir Cluster no portal do Azure**.

Colocar questões e enviar comentários a partir do Visual Studio:

1. No menu **Ferramentas**, selecione **HDInsight**.
2. Para fazer perguntas, selecione **Fórum do MSDN**. Para enviar comentários, selecione **Enviar Comentários**.

## <a name="explore-linked-resources"></a>Explorar recursos ligados
No Explorador de Servidores, pode ver a conta de armazenamento predefinida e quaisquer contas de armazenamento ligadas. Se expandir a conta do Storage predefinida, pode ver os contentores incluídos na mesma. A conta do Storage predefinida e o contentor predefinido estão marcados. Clique com o botão direito do rato em qualquer um dos contentores para ver os conteúdos do contentor.

![Captura de ecrã dos recursos associados da lista do Data Lake Tools para Visual Studio no Explorador de Servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Recursos associados da lista")

Depois de abrir um contentor, pode utilizar os botões seguintes para carregar, eliminar e transferir blobs:

![Captura de ecrã das operações de blobs do Data Lake Tools para Visual Studio no Explorador de Servidores](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Carregar, eliminar e transferir blobs no Explorador de Servidores")

## <a name="run-interactive-hive-queries"></a>Executar consultas interativas do Hive
O [Apache Hive](http://hive.apache.org) é uma infraestrutura de armazém de dados que está incorporada no Hadoop. O Hive é utilizado para resumo de dados, consultas e análises. Pode utilizar o Data Lake Tools para Visual Studio para executar consultas do Hive a partir do Visual Studio. Para obter mais informações sobre o Hive, veja [Use Hive with HDInsight](hdinsight-use-hive.md) (Utilizar o Hive com o HDInsight).

A [Consulta Interativa](../interactive-query/apache-interactive-query-get-started.md) utiliza o [Hive no LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) no Apache Hive 2.1. A Consulta Interativa traz interatividade a consultas complexas de estilo do armazém de dados em conjuntos de dados grandes e armazenados. Executar consultas do Hive no Interactive Query é muito mais rápido em comparação com as tradicionais tarefas de lote do Hive. Para obter mais informações, veja [Executar tarefas de lote do Hive](#run-hive-batch-jobs).

> [!NOTE]
>
> Pode executar consultas interativas do Hive apenas quando ligar a um cluster do [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md).

Também pode utilizar o Data Lake Tools para Visual Studio para ver o que está dentro de uma tarefa do Hive. O Data Lake Tools para Visual Studio recolhe e analisa os registos do Yarn de determinadas tarefas do Hive.

### <a name="view-hivesampletable"></a>Veja **hivesampletable**
Todos os clusters do HDInsight têm uma tabela do Hive de exemplo predefinida denominada hivesampletable. Esta tabela do Hive define como listar as tabelas do Hive, ver os esquemas de tabela e listar as linhas da tabela do Hive.

Para listar as tabelas do Hive e ver o esquema da tabela do Hive:

1. Para ver o esquema da tabela, no **Explorador de Servidores**, selecione **Azure** > **HDInsight**. Selecione o cluster e, em seguida, selecione **Bases de Dados do Hive** > **Predefinição** > **hivesampletable**.
2. Clique com o botão direito do rato em **hivesampletable** e, em seguida, clique em **Ver as Primeiras 100 Linhas** para listar as linhas. É equivalente a executar a seguinte consulta do Hive com o controlador ODBC do Hive:
   
     `SELECT * FROM hivesampletable LIMIT 100`
   
   Pode personalizar a contagem de linhas.
   
   ![Captura de ecrã de uma consulta de esquema do Hive do HDinsight para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Resultados de consulta do Hive")

### <a name="create-hive-tables"></a>Criar tabelas do Hive
Para criar uma tabela do Hive, pode utilizar a GUI ou utilizar consultas do Hive. Para obter informações sobre a utilização de consultas do Hive, consulte [Executar consultas do Hive](#run.queries).

Para criar uma tabela do Hive:

1. No **Explorador de Servidores**, selecione **Azure** > **Clusters do HDInsight**. Selecione o cluster do HDInsight e, em seguida, selecione **Bases de Dados do Hive**.
2. Clique com botão direito do rato em **predefinição** e, em seguida, selecione **Criar Tabela**.
3. Configure a tabela.  
4. Selecione **Criar Tabela** para submeter a tarefa de criação da nova tabela do Hive.
   
    ![Captura de ecrã da janela de Criação da Tabela do HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Criar tabela do Hive")

### <a name="run.queries"></a>Validar e executar consultas do Hive
Tem duas opções para criar e executar consultas do Hive:

* Criar consultas ad-hoc
* Criar uma aplicação do Hive

Para criar, validar e executar consultas ad-hoc:

1. No **Explorador de Servidores**, selecione **Azure** > **Clusters do HDInsight**.
2. Clique com o botão direito do rato no cluster no qual pretende executar a consulta e, em seguida, selecione **Escrever uma Consulta do Hive**.  
3. Introduza as consultas do Hive. 

    O editor do Hive suporta IntelliSense. O Data Lake Tools para Visual Studio suportam o carregamento de metadados remotos durante a edição do script do Hive. Por exemplo, quando escreve **SELECT * FROM**, o IntelliSense apresenta uma lista de todos os nomes de tabela sugeridos. Quando é especificado um nome de tabela, o IntelliSense lista os nomes das colunas. As ferramentas suportam quase todas as instruções DML do Hive, subconsultas e os UDFs incorporados.
   
    ![Captura de ecrã de um exemplo do IntelliSense do HDInsight Visual Studio Tools IntelliSense 1](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Captura de ecrã de um exemplo do IntelliSense do HDInsight Visual Studio Tools IntelliSense 2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > O IntelliSense sugere apenas os metadados do cluster selecionado na barra de ferramentas do HDInsight.
   > 
   
4. (Opcional) Para verificar os erros de sintaxe do script, selecione **Validar Script**.
   
    ![Captura de ecrã da validação local do Data Lake Tools para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Validar script")
5. Selecione **Submeter** ou **Submeter (Avançado)**. Se selecionar a opção de submissão avançada, configura o **Nome da Tarefa**, **Argumentos**, **Configurações Adicionais** e **Diretório de Estado** para o script:
   
    ![Captura de ecrã da consulta do Hive do Hadoop HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Submeter consultas")
   
    Depois de submeter a tarefa, é apresentada a janela **Resumo da Tarefa do Hive**.
   
    ![Captura de ecrã de um resumo de uma consulta do Hive do Hadoop HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Resumo de tarefas do Hive")
6. Utilize o botão **Atualizar** para atualizar o estado da tarefa até que este mude para **Concluído**.
7. Selecione as ligações na parte inferior para ver **Consulta de Trabalho**, **Saída do Trabalho**, **Registo do Trabalho** ou **Registo do Yarn**.

Para criar e executar uma solução do Hive:

1. No menu **Ficheiro**, selecione **Novo** e, em seguida, selecione **Projeto**.
2. No painel da esquerda, selecione **HDInsight**. No painel do meio, selecione **Aplicação do Hive**. Introduza as propriedades e, em seguida, selecione **OK**.
   
    ![Captura de ecrã de um novo projeto do Hive no HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Criar aplicações do Hive do Visual Studio")
3. No **Explorador de Soluções**, faça duplo clique em **Script.hql** para abrir o script.
4. Para validar o script do Hive, selecione o botão **Validar Script**. Em alternativa, pode clicar com o botão direito do rato no script no editor do Hive e, em seguida, selecionar **Validar Script** no menu de contexto.

### <a name="view-hive-jobs"></a>Ver tarefas do Hive
Pode ver as consultas da tarefa, a saída da tarefa, os registos da tarefa e os registos Yarn para as tarefas do Hive. Para obter mais informações, veja a captura de ecrã anterior.

Na versão mais recente das ferramentas, pode ver o conteúdo das tarefas do Hive ao recolher e analisar registos Yarn. Um registo Yarn pode ajudar a investigar problemas de desempenho. Para obter mais informações sobre a forma como o HDInsight recolhe registos Yarn, veja [Access HDInsight application logs programmatically (Aceder programaticamente aos registos de aplicações do HDInsight)](../hdinsight-hadoop-access-yarn-app-logs.md).

Para ver tarefas do Hive:

1. No **Explorador de Servidores**, expanda **Azure** e, em seguida, expanda **HDInsight**.
2. Clique com o botão direito do rato num cluster do HDInsight e, em seguida, selecione **Ver Tarefas**. É apresentada uma lista dos trabalhos do Hive executados no cluster.  
3. Selecione uma tarefa. Na janela **Resumo da Tarefa do Hive**, selecione uma das seguintes opções:
    - **Consulta da Tarefa**
    - **Resultado da Tarefa**
    - **Registo da Tarefa**  
    - **Registo do Yarn**
   
    ![Captura de ecrã da janela de Visualização das Tarefas do Hive no HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Ver tarefas do Hive")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Execução do Hive de caminho mais rápido através do HiveServer2
> [!NOTE]
> Esta funcionalidade funciona apenas num cluster no HDInsight versão 3.2 ou posterior.
 
Anteriormente, o Data Lake Tools para Visual Studio submetiam as tarefas do Hive através de [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (também conhecido como Templeton). Este método para submeter tarefas do Hive demorava muito tempo para devolver os detalhes da tarefa e de informações de erro.

Para resolver este problema de desempenho, o Data Lake Tools para Visual Studio podem ignorar o RDP/SSH e executar as tarefas do Hive diretamente no cluster através do HiveServer2.

Para além de usufruírem de um melhor desempenho, também pode ver o Hive em gráficos Apache Tez, bem como os detalhes da tarefa.

Num cluster do HDInsight versão 3.2 ou posterior, é apresentado um botão **Executar através de HiveServer2**:

![Captura de ecrã da opção Executar o Data Lake Tools para Visual Studio através do HiveServer2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Executar consultas do Hive com o HiveServer2")

Também pode ver os registos transmitidos em tempo real. Também pode ver os gráficos da tarefa, se a consulta do Hive for executada no Tez.

![Captura de ecrã da execução do Hive de caminho mais rápido do Data Lake Tools para Visual Studio com HiveServer2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Ver gráficos da tarefa")

### <a name="execute-queries-via-hiveserver2-vs-submit-queries-via-webhcat"></a>Executar consultas através do HiveServer2 ou submeter consultas através do WebHCat

Embora tenha muitos benefícios de desempenho ao executar consultas através do HiveServer2, este método tem várias limitações. Algumas das limitações tornam este método inadequado para utilização em produção. 

A tabela seguinte mostra as diferenças entre executar consultas através do HiveServer2 e submeter consultas através do WebHCat:

|  | Executar através do HiveServer2 | Submeter através do WebHCat |
| --- | --- | --- |
| Executar consultas |Elimina o overhead no WebHCat (que inicia uma Tarefa de MapReduce com o nome TempletonControllerJob). |Se uma consulta for executada através de WebHCat, este inicia um trabalho do MapReduce que cria latência adicional. |
| Transmitir os registos |Em tempo quase real. |Os registos de execução da tarefa ficam disponíveis apenas quando a tarefa estiver concluída. |
| Ver histórico de tarefas |Se uma consulta for executada através de HiveServer2, o respetivo histórico de tarefas (registo da tarefa, resultado da tarefa) não é mantido. Pode ver a aplicação na IU do Yarn com informações limitadas. |Se uma consulta for executada através de WebHCat, o respetivo histórico de tarefas (registo da tarefa, resultado da tarefa) é mantido. Pode ver o histórico de tarefas com o Visual Studio, o SDK do HDInsight ou o PowerShell. |
| Fechar janela |A execução através do HiveServer é *síncrona*. Se fechar as janelas, a execução da consulta é cancelada. |A submissão através do WebHCat é *assíncrona*. Pode submeter a consulta através do WebHCat e, em seguida, fechar o Visual Studio. Pode voltar atrás e ver os resultados em qualquer altura. |

### <a name="tez-hive-job-performance-graph"></a>Gráfico de desempenho da tarefa do Hive no Tez
No Data Lake Tools para Visual Studio, pode ver os gráficos de desempenho para as tarefas do Hive que o motor de execução Tez executa. Para obter informações sobre como ativar o Tez, veja [Use Hive in HDInsight (Utilizar o Hive no HDInsight)](hdinsight-use-hive.md). 

Depois de submeter uma tarefa do Hive no Visual Studio, o Visual Studio mostra-lhe o gráfico quando a tarefa estiver concluída. Poderá ter de selecionar o botão **Atualizar** para ver o estado mais recente da tarefa.

> [!NOTE]
> Esta funcionalidade está disponível apenas para um cluster no HDInsight versão 3.2.4.593 ou posterior. A funcionalidade funciona apenas nas tarefas concluídas. Também tem de submeter a tarefa através do WebHCat para utilizar esta funcionalidade. A imagem seguinte é apresentada ao executar a consulta através do HiveServer2: 
> 
> ![Captura de ecrã de um gráfico de desempenho do Hadoop Hive no Tez](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Estado das tarefas")

Para ajudar a compreender melhor a sua consulta do Hive, a vista Operador do Hive foi adicionada nesta versão. Para ver todos os operadores dentro do vértice, faça duplo clique nos vértices do gráfico da tarefa . Também pode apontar para um operador específico para ver mais detalhes sobre o operador.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Vista de Execução de Tarefas para tarefas do Hive no Tez
Pode utilizar a Vista de Execução de Tarefas para tarefas do Hive no Tez para obter informações estruturadas e visualizadas sobre tarefas do Hive. Também pode obter mais detalhes de tarefas. Se ocorrerem problemas de desempenho, pode utilizar a vista para obter mais detalhes sobre o problema. Por exemplo, pode obter informações sobre como funciona cada tarefa e informações detalhadas sobre cada tarefa (leitura/escrita de dados, hora de agendamento/início/fim e assim sucessivamente). Utilize as informações para otimizar as configurações da tarefa ou a arquitetura do sistema, com base nas informações visualizadas.

![Captura de ecrã da janela da Vista de Execução de Tarefas no Data Lake Tools para Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Vista de Execução de Tarefas")

## <a name="run-hive-batch-jobs"></a>Executar trabalhos de lote do Hive
Testar scripts do Hive em clusters do HDInsight (com a exceção do cluster do Interactive Quer)y pode ser uma tarefa morosa. O processo poderá demorar vários minutos ou mais. O Data Lake Tools para Visual Studio podem validar os scripts de Hive localmente, sem ligar a um cluster ativo. Para obter mais informações sobre como executar consultas interativas, veja [Executar consultas interativas do Hive](#run-interactive-hive-queries).

E pode utilizar o Data Lake Tools para Visual Studio para ver o conteúdo da tarefa do Hive ao recolher e analisar os registos Yarn de determinadas tarefas do Hive.

Para saber mais sobre como executar tarefas de lote do Hive, veja [Executar consultas interativas do Hive](#run-interactive-hive-queries). As informações nesta secção aplicam-se à execução de tarefas de lote do Hive que trabalham durante mais tempo.

## <a name="run-pig-scripts"></a>Executar scripts Pig
Pode utilizar o Data Lake Tools para Visual Studio para criar e submeter scripts Pig para clusters do HDInsight. Primeiro, crie um projeto do Pig a partir de um modelo. Em seguida, submeta o script para clusters do HDInsight.

## <a name="feedback-and-known-issues"></a>Comentários e problemas conhecidos
* Atualmente, os resultados do HiveServer2 são apresentados sob a forma de texto simples, o que não é ideal. A Microsoft está a trabalhar para corrigir este problema.
* Foi corrigido um problema no qual os resultados que são iniciados com valores nulos não são apresentados. Se estiver bloqueado neste problema, contacte a equipa de suporte.
* O script HQL criado pelo Visual Studio é codificado consoante a definição de região local do utilizador. O script não é executado corretamente se carregar o script para um cluster como um ficheiro binário.

## <a name="next-steps"></a>Passos seguintes
Neste artigo, aprendeu a utilizar o pacote do Data Lake Tools para Visual Studio para ligar a clusters do HDInsight a partir do Visual Studio. Também aprendeu a executar uma consulta do Hive. Para obter mais informações, veja estes artigos:

* [Utilizar o Hive do Hadoop no HDInsight](hdinsight-use-hive.md)
* [Introdução à utilização do Hadoop no HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Submeter tarefas do Hadoop no HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Analisar dados do Twitter com o Hadoop no HDInsight](../hdinsight-analyze-twitter-data.md)

