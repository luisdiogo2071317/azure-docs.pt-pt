---
title: Ferramentas do HDInsight do Azure - utilizar o Visual Studio Code para Hive, LLAP ou PySpark | Documentos da Microsoft
description: Saiba como utilizar o Azure HDInsight Tools para Visual Studio Code para criar e submeter consultas e scripts.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: 9603751db01eaffdf9fbe26164aed53017c5e23c
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499529"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Utilizar ferramentas do Azure HDInsight para Visual Studio Code

Saiba como utilizar o [Azure HDInsight Tools para Visual Studio Code](https://docs.microsoft.com/azure/hdinsight/hdinsight-for-vscode) (VS Code) para criar e submeter [Apache Hive](https://hive.apache.org/) tarefas, consultas interativas do Apache Hive e PySpark scripts do batch. As ferramentas do HDInsight do Azure pode ser instaladas nas plataformas suportadas pelo código VS. que incluem o Windows, o Linux e o macOS. Pode encontrar os pré-requisitos para plataformas diferentes.


## <a name="prerequisites"></a>Pré-requisitos

Os itens seguintes são necessários para concluir os passos neste artigo:

- Um cluster do HDInsight. Para criar um cluster, veja [introdução ao HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono só é necessário para Linux e macOS.

## <a name="install-the-hdinsight-tools"></a>Instalar as ferramentas do HDInsight
   
Depois de instalar os pré-requisitos, pode instalar as ferramentas do HDInsight do Azure para VS Code. 

### <a name="to-install-azure-hdinsight-tools"></a>Para instalar as ferramentas do Azure HDInsight

1. Abra o Visual Studio Code.

2. No painel esquerdo, selecione **extensões**. Na caixa de pesquisa, introduza **HDInsight**.

3. Junto a **ferramentas do Azure HDInsight**, selecione **instalar**. Após alguns segundos, o **instale** botão muda para **recarregar**.

4. Selecione **recarregar** para ativar a **Azure HDInsight Tools** extensão.

5. Selecione **janela de recarregamento** para confirmar. Pode ver **ferramentas do Azure HDInsight** no **extensões** painel.

   ![HDInsight para instalação de Python de código do Visual Studio](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Abra a área de trabalho do HDInsight

Crie uma área de trabalho no VS Code, antes de poder ligar para o Azure.

### <a name="to-open-a-workspace"></a>Para abrir uma área de trabalho

1. Sobre o **arquivo** menu, selecione **Abrir pasta**. Em seguida, designar uma pasta existente como a pasta de trabalho ou crie um novo. A pasta é apresentada no painel esquerdo.

2. No painel esquerdo, selecione o **novo ficheiro** ícone ao lado da pasta de trabalho.

   ![Novo ficheiro](./media/hdinsight-for-vscode/new-file.png)

3. Nomeie o novo arquivo com o .hql (consultas do Hive) ou a extensão de ficheiro. PY (script do Spark). 

## <a name="connect-to-hdinsight-cluster"></a>Ligar ao Cluster do HDInsight

Antes de pode enviar scripts para clusters do HDInsight a partir do código de VS, precisa ligar à sua conta do Azure ou ligar um cluster (com o Ambari nome de utilizador/palavra-passe ou domínio associado a um conta).

### <a name="to-connect-to-azure"></a>Para ligar ao Azure

1. Crie uma nova pasta de trabalho e um novo ficheiro de script, se ainda não tivê-los.

2. O editor de scripts com o botão direito e, em seguida, no menu de contexto, selecione **HDInsight: início de sessão**. Também pode introduzir **Ctrl + Shift + P**e, em seguida, introduza **HDInsight: início de sessão**.

    ![Ferramentas do HDInsight para início de sessão do Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Para iniciar sessão, siga as instruções de início de sessão no **saída** painel.
    + Para o ambiente global, início de sessão de HDInsight irá acionar Azure inicie sessão no processo.

        ![Iniciar sessão nas instruções para o azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Para outros ambientes, siga as instruções de início de sessão.

        ![Inicie sessão no instruções para outro ambiente](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

    Quando estiver ligado, o nome da sua conta do Azure é mostrado na barra de estado no canto inferior esquerdo da janela do VS Code. 

    > [!NOTE]
    > Devido a um problema conhecido de autenticação do Azure, terá de abrir um browser no modo privado ou navegação anónima. Se a sua conta do Azure tem dois fatores ativadas, recomendamos utilizar a autenticação de telefone em vez da autenticação do PIN.
  

4. Com o botão direito do editor de scripts para abrir o menu de contexto. No menu de contexto, pode executar as seguintes tarefas:

    - Terminar sessão
    - Listar clusters
    - Conjunto predefinido clusters
    - Submeter consultas interativas do Hive
    - Submeter scripts de lote do Hive
    - Submeter consultas interativas do PySpark
    - Submissão de scripts de batch do PySpark
    - Configuração de conjunto

<h3 id="linkcluster">Para ligar um cluster</h3>

Pode ligar um cluster normal, utilizando uma [Apache Ambari](https://ambari.apache.org/) geridos pelo nome de utilizador ou ao ligar a um cluster de Hadoop seguro do pacote de segurança de Enterprise utilizando um nome de utilizador de domínio (como: user1@contoso.com).
1. Abra a paleta de comandos, selecionando **CTRL + SHIFT + P**e, em seguida, introduza **HDInsight: ligar um Cluster**.

   ![comando de cluster de ligação](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Introduza o HDInsight URL do cluster -> entrada nome de utilizador -> entrada de palavra-passe -> selecione o tipo de cluster ->-mostra informações de sucesso se transmitido de verificação.
   
   ![caixa de diálogo de cluster de ligação](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > O nome de utilizador ligado e a palavra-passe são utilizados se o cluster tanto registado na subscrição do Azure e ligadas um cluster. 
   
3. Pode ver um cluster de ligado, utilizando o comando **Cluster de lista**. Agora pode submeter um script para este cluster ligado.

   ![cluster ligado](./media/hdinsight-for-vscode/linked-cluster.png)

4. Também pode desassociar um cluster ao inserir **HDInsight: desassociar um Cluster** da paleta de comandos.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>Para ligar um ponto de extremidade genérico do Apache Livy

1. Abra a paleta de comandos, selecionando **CTRL + SHIFT + P**e, em seguida, introduza **HDInsight: ligar um Cluster**.
2. Selecione **ponto final do Livy genérico**.
3. Introduza o ponto de extremidade genérico do Livy, por exemplo: http://10.172.41.42:18080.
4. Selecione **básica** quando a autorização para o ponto de extremidade genérico Livy, caso contrário, tem de selecionar **nenhum**.
5. Nome de utilizador de entrada quando selecione **básica** no step4.
6. Entrada palavra-passe quando selecione **básica** no step4.
7. O ponto de extremidade genérico do livy ligado com êxito.

   ![cluster do livy genérico ligado](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>Listar clusters do HDInsight

Para testar a ligação, pode listar os seus clusters do HDInsight:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>Listar clusters do HDInsight com a sua subscrição do Azure
1. Abra uma área de trabalho e, em seguida, ligar para o Azure. Para obter mais informações, consulte [área de trabalho do HDInsight aberto](#open-hdinsight-workspace) e [ligar ao Azure](#connect-to-hdinsight-cluster).

2. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: Cluster de lista** no menu de contexto. 

3. Os clusters do HDInsight são apresentados no **saída** painel.

    ![Definir uma configuração de cluster predefinido](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Definir um cluster de predefinição
1. Abra uma área de trabalho e ligar ao Azure. Ver [área de trabalho do HDInsight aberto](#open-hdinsight-workspace) e [ligar ao Azure](#connect-to-hdinsight-cluster).

2. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: Definir Cluster predefinido**. 

3. Selecione um cluster do cluster predefinido para o ficheiro de script atual. As ferramentas de atualizar automaticamente o ficheiro de configuração **. VSCode\settings.json**. 

   ![Configuração de cluster de predefinida do conjunto](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Defina o ambiente do Azure
1. Abra a paleta de comandos, selecionando **CTRL + SHIFT + P**.

2. Introduza **HDInsight: definir o ambiente do Azure**.

3. Selecione um ambiente, como "Azure" ou "AzureChina" como a entrada de início de sessão predefinida.

4. Enquanto isso, a ferramenta já salvou sua entrada de início de sessão predefinida na **. VSCode\settings.json**. Também atualizá-la diretamente nesse arquivo de configuração. 

   ![Configuração de entrada de início de sessão de predefinida do conjunto](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Submeter consultas interativas do Hive, scripts em lote de ramo de registo

Com as ferramentas do HDInsight para o VS Code, pode submeter consultas do Hive interativas, scripts em lote do Hive para clusters do HDInsight.

1. Crie uma nova pasta de trabalho e um novo ficheiro de script do Hive, se ainda não os tiver.

2. Ligue-se aos seus clusters de conta ou uma ligação do Azure.

3. Copie e cole o seguinte código para o ficheiro do Hive e guarde-o.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. O editor de scripts com o botão direito, selecione **HDInsight: interativas do Hive** para submeter a consulta ou utilize o atalho **Ctrl + Alt + I**. Selecione **HDInsight: lote do Hive** submeta o script ou usar o atalho **Ctrl + Alt + H**. 

5. Selecione o cluster se ainda não especificou um cluster de predefinição. As ferramentas permitem-lhe também submeter um bloco de código em vez de todo o ficheiro de script através do menu de contexto. Após alguns instantes, os resultados da consulta são apresentados num novo separador.

   ![Resultado do Hive interativo](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Painel **RESULTADOS**: pode guardar todo o resultado como ficheiro CSV, JSON ou Excel no caminho local ou apenas selecionar várias linhas.

    - Painel **MENSAGENS**: quando seleciona o número da **Linha**, passa para a primeira linha do script em execução.

## <a name="submit-interactive-pyspark-queries"></a>Submeter consultas interativas do PySpark

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>Para enviar consultas interativas do PySpark para clusters do HDInsight Spark.

1. Crie uma nova pasta de trabalho e um novo ficheiro de script com a extensão. PY se ainda não tivê-los.

2. Ligar à sua conta do Azure se ainda não tiver feito isso.

3. Copie e cole o seguinte código para o ficheiro de script:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Realce este script. Em seguida, o editor de scripts com o botão direito e selecione **HDInsight: interativo do PySpark**, ou utilize o atalho **Ctrl + Alt + I**.

5. Se ainda não instalou o **Python** extensão no VS Code, selecione a **instalar** botão conforme mostrado na ilustração seguinte:

    ![HDInsight para instalação de Python de código do Visual Studio](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Se ainda não o fez, instale o ambiente do Python no seu sistema. 
   - Para Windows, transfira e instale [Python](https://www.python.org/downloads/). Em seguida, certifique-se `Python` e `pip` estão no seu caminho do sistema.

   - Para obter instruções para macOS e Linux, veja [configurar o ambiente interativo do PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Selecione um cluster ao qual pretende enviar a sua consulta do PySpark. Em breve, afinal, o resultado da consulta é mostrado num novo separador de certo:

   ![Submeter o resultado da tarefa de Python](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. A ferramenta também suporta o **cláusula SQL** consulta.

   ![Submeter o resultado da tarefa de Python](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) o estado da submissão é apresentada à esquerda do estado na parte inferior da barra quando estiver a executar consultas. Não se submeta outras consultas quando o estado é **Kernel do PySpark (ocupado)**. 

>[!NOTE]
>Os clusters podem manter informações de sessão. A variável definida, a função e a valores correspondentes são mantidas na sessão, pelo que pode ser referenciadas em várias chamadas de serviço para o mesmo cluster. 

### <a name="to-disable-environment-check"></a>Para desativar a verificação de ambiente

Por predefinição, as ferramentas do HDInsight irão verificar o ambiente e instalar pacotes dependentes quando submeter consultas interativas do PySpark. Para desativar a verificação do ambiente, defina o **hdinsight.disablePysparkEnvironmentValidation** ao **Sim** sob **definições de utilizador**.

   ![Definir a verificação de ambiente a partir das definições](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check.png)

Em alternativa, clique em **desativar a validação** botão quando a caixa de diálogo será exibida.

   ![Definir a verificação do ambiente da caixa de diálogo](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check-dialog.png)

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 não é suportado com Spark2.2/2.3

PySpark3 já não é suportado com o cluster do Spark 2.2 e do cluster de Spark2.3, apenas "PySpark" é suportado para o Python. Problema que envia para o spark 2.2/2.3 falhar com Python3 é conhecido.

   ![Submeter para erro de obtenção de python3](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Siga os passos para utilizar Python2.x: 

1. Instale o Python 2.7 para o computador local e adicioná-lo ao caminho do sistema.

2. Reinicie o VSCode.

3. Mudar para o Python 2 ao clicar no **Python XXX** o estado da barra, em seguida, escolha o destino de Python.

   ![Selecione a versão do python](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

## <a name="submit-pyspark-batch-job"></a>Submeter a tarefa de lote do PySpark

1. Crie uma nova pasta de trabalho e um novo ficheiro de script com a extensão. PY se ainda não tivê-los.

2. Ligar à sua conta do Azure se ainda não o tiver feito.

3. Copie e cole o seguinte código para o ficheiro de script:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: PySpark Batch**, ou utilize o atalho **Ctrl + Alt + H**. 

5. Selecione um cluster ao qual pretende submeter a tarefa de PySpark. 

   ![Submeter o resultado da tarefa de Python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Depois de submeter um trabalho do Python, registos de submissão constar da **saída** janela no VS Code. O **URL de interface do Usuário do Spark** e **URL de IU do Yarn** são mostrados também. É possível abrir o URL num navegador da web para controlar o estado da tarefa.

## <a name="apache-livy-configuration"></a>Configuração de Apache Livy

[Apache Livy](https://livy.incubator.apache.org/) a configuração é suportada, ele poderia ser definido no **. VSCode\settings.json** na pasta de espaço de trabalho. Atualmente, a configuração do livy só suporta script de Python. Obter mais detalhes, veja [Leiame do Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Como acionar a configuração do livy**
   
Pode encontrar no **arquivo** menu, selecione **preferências**e escolha **definições** no menu de contexto. Clique em **definições de área de trabalho** separador, pode começar a definir a configuração do livy.

Observe que a pasta de .vscode também pode submeter um ficheiro, é adicionado automaticamente para a pasta de trabalho. Pode encontrar a configuração do livy clicando **.vscode\settings.json**.

+ As configurações do projeto:

    ![Configuração do Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Para as definições **driverMomory** e **executorMomry**, defina o valor com a unidade, por exemplo, 1 GB ou 1024 m. 

+ As configurações suportadas do Livy:   

    **POST /batches**   
    Corpo do Pedido

    | nome | descrição | tipo | 
    | :- | :- | :- | 
    | ficheiro | Ficheiro que contém o aplicativo para executar | caminho (obrigatório) | 
    | proxyUser | Utilizador representar ao executar a tarefa | cadeia | 
    | className | Classe principal do aplicativo Java/Spark | cadeia |
    | args | Argumentos de linha de comandos para a aplicação | lista de cadeias de caracteres | 
    | jars | jars a ser utilizado nesta sessão | Lista de cadeia de caracteres | 
    | pyFiles | Arquivos de Python a serem usados nesta sessão | Lista de cadeia de caracteres |
    | ficheiros | arquivos a serem usados nesta sessão | Lista de cadeia de caracteres |
    | driverMemory | Quantidade de memória a utilizar para o processo de driver | cadeia |
    | driverCores | Número de núcleos para utilizar para o processo de driver | int |
    | executorMemory | Quantidade de memória a utilizar por processo de executor | cadeia |
    | executorCores | Número de núcleos para utilizar para cada executor | int |
    | numExecutors | Número de executores para iniciar a esta sessão | int |
    | arquivos compactados | Arquivos compactados a ser utilizado nesta sessão | Lista de cadeia de caracteres |
    | fila | O nome da fila YARN para o qual submetido | cadeia |
    | nome | O nome desta sessão | cadeia |
    | Conf | Propriedades de configuração de Spark | Mapa de chave = valor |

    Corpo da Resposta   
    O objeto criado do Batch.

    | nome | descrição | tipo | 
    | :- | :- | :- | 
    | ID | O id de sessão | int | 
    | appId | O id de aplicação desta sessão |  Cadeia |
    | appInfo | As informações detalhadas de aplicação | Mapa de chave = valor |
    | registo | As linhas de registo | lista de cadeias de caracteres |
    | state |   O estado de batch | cadeia |

>[!NOTE]
>A configuração do livy atribuído serão apresentados no painel de resultados quando submeta o script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integrar com o Azure HDInsight a partir do Explorador

HDInsight do Azure foi adicionado ao painel do lado esquerdo. Pode procurar e gerir o cluster diretamente.

1. Expanda a **do AZURE HDINSIGHT**, se não início de sessão, ele mostrará **iniciar sessão no Azure...**  ligação.

    ![Inicie sessão na imagem de ligação](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Clique em **iniciar sessão no Azure**, ele exibe o início de sessão na ligação e o código na parte inferior direita.

    ![Inicie sessão no instruções para outro ambiente](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Clique em **copie e abra** botão irá abrir o navegador, cole o código, clique em **continuar** botão, em seguida, verá a sugestão sobre o início de sessão com êxito.

4. Depois de iniciar sessão, os clusters (Spark, Hadoop e HBase são suportados) e de subscrições disponíveis serão listados na **do AZURE HDINSIGHT**. 

   ![Subscrição do Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Expanda o cluster para ver o esquema de base de dados e tabela de metadados do hive.

   ![Cluster de Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Funcionalidades adicionais

HDInsight para o VS Code suporta as seguintes funcionalidades:

- **IntelliSense, conclusão automática**. Sugestões pop-up para palavra-chave, métodos, variáveis e assim por diante. Representam os ícones de diferentes tipos de objetos diferentes.

    ![Ferramentas do HDInsight para tipos de objeto do IntelliSense de código do Visual Studio](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marcador de erro do IntelliSense**. O serviço de linguagem sublinha os erros de edição para o script do Hive.     
- **Destaques de sintaxe**. O serviço de linguagem usa cores diferentes para diferenciar as variáveis, palavras-chave, tipo de dados, as funções e assim por diante. 

    ![Ferramentas do HDInsight para Visual Studio Code sintaxe destaques](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Passos Seguintes

### <a name="demo"></a>Demonstração
* HDInsight para o VS Code: [vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Ferramentas e extensões

* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações de Apache Spark remotamente através de VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações de Apache Spark remotamente através de SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Utilizar ferramentas do HDInsight para IntelliJ com a Sandbox da Hortonworks](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Utilizar ferramentas do HDInsight no Azure Toolkit para Eclipse para criar aplicações do Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Utilizar blocos de notas do Zeppelin do Apache com um cluster do Apache Spark no HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Apache Spark para HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualize os dados do Apache Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualizar dados de consulta interativa do Hive com o Power BI no Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Configurar o ambiente interativo do PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Utilizar Apache Zeppelin para executar consultas do Apache Hive no Azure HDInsight ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Cenários
* [Apache Spark com BI: efetuar análise de dados interativa com o Spark no HDInsight com ferramentas de BI](spark/apache-spark-use-bi-tools.md)
* [Apache Spark com Machine Learning: utilizar o Spark no HDInsight para analisar a temperatura de construção com dados de AVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark com Machine Learning: utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Análise de registos de Web site com o Apache Spark no HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Criar e executar aplicações
* [Criar uma aplicação autónoma com o Scala](spark/apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Apache Spark com o Apache Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Gerir recursos
* [Gerir recursos para o cluster do Apache Spark no Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](spark/apache-spark-job-debugging.md)



