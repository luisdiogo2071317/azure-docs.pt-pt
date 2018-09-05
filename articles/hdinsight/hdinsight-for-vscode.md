---
title: Ferramentas do Azure HDInsight - Use Visual Studio Code para Hive, LLAP ou pySpark
description: Saiba como utilizar o Azure HDInsight Tools para Visual Studio Code para criar e submeter consultas e scripts.
keywords: Código VS, ferramentas do Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interativas do Hive, Interactive Query
services: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: 58f930b7bb1dee8f8f95b6627ebf70fe095126c0
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697854"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Utilizar ferramentas do Azure HDInsight para Visual Studio Code

Saiba como utilizar o Azure HDInsight Tools para Visual Studio Code (código de VS) para criar e submeter tarefas de lote do Hive, consultas interativas do Hive e pySpark scripts. As ferramentas do HDInsight do Azure pode ser instaladas nas plataformas suportadas pelo código VS. que incluem o Windows, o Linux e o macOS. Pode encontrar os pré-requisitos para plataformas diferentes.


## <a name="prerequisites"></a>Pré-requisitos

Os itens seguintes são necessários para concluir os passos neste artigo:

- Um cluster do HDInsight. Para criar um cluster, veja [introdução ao HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono só é necessário para Linux e macOS.

## <a name="install-the-hdinsight-tools"></a>Instalar as ferramentas do HDInsight
   
Depois de instalar os pré-requisitos, pode instalar as ferramentas do HDInsight do Azure para VS Code. 

**Para instalar o Azure HDInsight tools**

1. Abra o Visual Studio Code.

2. No painel esquerdo, selecione **extensões**. Na caixa de pesquisa, introduza **HDInsight**.

3. Junto a **ferramentas do Azure HDInsight**, selecione **instalar**. Após alguns segundos, o **instale** botão muda para **recarregar**.

4. Selecione **recarregar** para ativar a **ferramentas do Azure HDInsight** extensão.

5. Selecione **janela de recarregamento** para confirmar. Pode ver **ferramentas do Azure HDInsight** no **extensões** painel.

   ![HDInsight para instalação de Python de código do Visual Studio](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Abra a área de trabalho do HDInsight

Crie uma área de trabalho no VS Code, antes de poder ligar para o Azure.

**Para abrir uma área de trabalho**

1. Sobre o **arquivo** menu, selecione **Abrir pasta**. Em seguida, designar uma pasta existente como a pasta de trabalho ou crie um novo. A pasta é apresentada no painel esquerdo.

2. No painel esquerdo, selecione o **novo ficheiro** ícone ao lado da pasta de trabalho.

   ![Novo ficheiro](./media/hdinsight-for-vscode/new-file.png)

3. Nomeie o novo arquivo com o .hql (consultas do Hive) ou a extensão de ficheiro. PY (script do Spark). Tenha em atenção que uma **XXXX_hdi_settings.json** ficheiro de configuração é adicionado automaticamente para a pasta de trabalho.

4. Open **XXXX_hdi_settings.json** partir **EXPLORER**, ou com o botão direito do editor de scripts para selecionar **definir configuração**. Pode configurar a entrada de início de sessão e parâmetros de submissão da tarefa, conforme mostrado no exemplo no arquivo predefinido do cluster. Também pode deixar os parâmetros restantes vazio.

## <a name="connect-to-hdinsight-cluster"></a>Ligar ao Cluster do HDInsight

Antes de pode enviar scripts para clusters do HDInsight a partir do código de VS, precisa ligar à sua conta do Azure ou ligar um cluster (com o Ambari nome de utilizador/palavra-passe ou domínio associado a um conta).

**Para ligar ao Azure**

1. Crie uma nova pasta de trabalho e um novo ficheiro de script, se ainda não tivê-los.

2. O editor de scripts com o botão direito e, em seguida, no menu de contexto, selecione **HDInsight: início de sessão**. Também pode introduzir **Ctrl + Shift + P**e, em seguida, introduza **HDInsight: início de sessão**.

    ![Ferramentas do HDInsight para Visual Studio Code, iniciar sessão](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Para iniciar sessão, siga as instruções de início de sessão no **saída** painel.

    **Azure:** ![ferramentas HDInsight para informações de início de sessão do Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

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
    - Configurações de conjunto

<a id="linkcluster"></a>**Para ligar um cluster**

Pode ligar um cluster do normal com o nome de utilizador do Ambari gerida, também ligar um cluster de hadoop de segurança com o nome de utilizador de domínio (como: user1@contoso.com).
1. Abra a paleta de comandos, selecionando **CTRL + SHIFT + P**e, em seguida, introduza **HDInsight: ligar um cluster**.

   ![comando de cluster de ligação](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Introduza o HDInsight URL do cluster -> entrada nome de utilizador -> entrada de palavra-passe -> selecione o tipo de cluster ->-mostra informações de sucesso se transmitido de verificação.
   
   ![caixa de diálogo de cluster de ligação](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > O nome de utilizador ligado e a palavra-passe são utilizados se o cluster tanto registado na subscrição do Azure e ligadas um cluster. 
   
3. Pode ver um cluster de ligado, utilizando o comando **cluster de lista**. Agora pode submeter um script para este cluster ligado.

   ![cluster ligado](./media/hdinsight-for-vscode/linked-cluster.png)

4. Também pode desassociar um cluster ao inserir **HDInsight: desassociar um cluster** da paleta de comandos.

## <a name="list-hdinsight-clusters"></a>Listar clusters do HDInsight

Para testar a ligação, pode listar os seus clusters do HDInsight:

**Listar clusters do HDInsight com a sua subscrição do Azure**
1. Abra uma área de trabalho e, em seguida, ligar para o Azure. Para obter mais informações, consulte [área de trabalho do HDInsight aberto](#open-hdinsight-workspace) e [ligar ao Azure](#connect-to-azure).

2. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: Cluster de lista** no menu de contexto. 

3. Os clusters do Hive e do Spark constar da **saída** painel.

    ![Definir uma configuração de cluster predefinido](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Definir um cluster de predefinição
1. Abra uma área de trabalho e ligar ao Azure. Ver [área de trabalho do HDInsight aberto](#open-hdinsight-workspace) e [ligar ao Azure](#connect-to-azure).

2. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: Definir Cluster predefinido**. 

3. Selecione um cluster do cluster predefinido para o ficheiro de script atual. As ferramentas de atualizar automaticamente o ficheiro de configuração **XXXX_hdi_settings.json**. 

   ![Configuração de cluster de predefinida do conjunto](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>Defina o ambiente do Azure 
1. Abra a paleta de comandos, selecionando **CTRL + SHIFT + P**.

2. Introduza **HDInsight: definir o ambiente do Azure**.

3. Selecione uma forma do Azure e AzureChina como sua participação de início de sessão predefinido.

4. Enquanto isso, a ferramenta já salvou sua entrada de início de sessão predefinida na **XXXX_hdi_settings.json**. Também atualizá-la diretamente nesse arquivo de configuração. 

   ![Configuração de entrada de início de sessão de predefinida do conjunto](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Submeter consultas interativas do Hive

Com as ferramentas do HDInsight para o VS Code, pode submeter consultas interativas do Hive para clusters do HDInsight interactive query.

1. Crie uma nova pasta de trabalho e um novo ficheiro de script do Hive, se ainda não os tiver.

2. Ligue-se à sua conta do Azure e, em seguida, configure o cluster predefinido, se ainda não o tiver feito.

3. Copie e cole o seguinte código para o ficheiro do Hive e guarde-o.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Clique com o botão direito do rato no editor de scripts e, em seguida, selecione **HDInsight: Hive Interativo** para submeter a consulta. As ferramentas permitem-lhe também submeter um bloco de código em vez de todo o ficheiro de script através do menu de contexto. Logo depois, os resultados da consulta são apresentados num novo separador.

   ![Resultado do Hive interativo](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Painel **RESULTADOS**: pode guardar todo o resultado como ficheiro CSV, JSON ou Excel no caminho local ou apenas selecionar várias linhas.

    - Painel **MENSAGENS**: quando seleciona o número da **Linha**, passa para a primeira linha do script em execução.

A execução da consulta interativa demora muito menos tempo que [executar uma tarefa de lote do Hive](#submit-hive-batch-scripts).

## <a name="submit-hive-batch-scripts"></a>Submeter scripts de lote do Hive

1. Crie uma nova pasta de trabalho e um novo ficheiro de script do Hive, se ainda não os tiver.

2. Ligue-se à sua conta do Azure e, em seguida, configure o cluster predefinido, se ainda não o tiver feito.

3. Copie e cole o seguinte código para o ficheiro do Hive e guarde-o.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Clique com o botão direito do rato no editor de scripts e, em seguida, selecione **HDInsight: Lote do Hive** para submeter a tarefa do Hive. 

4. Selecione o cluster para o qual pretende submeter.  

    Depois de submeter uma tarefa do Hive, as informações de êxito de submissão e o ID da tarefa são apresentados no painel **SAÍDA**. A tarefa do Hive também abre o **BROWSER**, que mostra o estado e os registos da tarefa em tempo real.

   ![submeter o resultado da tarefa do Hive](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

[Submeter consultas interativas do Hive](#submit-interactive-hive-queries) demora muito menos tempo que submeter uma tarefa de lote.

## <a name="submit-interactive-pyspark-queries"></a>Submeter consultas interativas do PySpark
Ferramentas do HDInsight para o VS Code também permite-lhe submeter consultas interativas do PySpark para clusters do Spark.
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
4. Realce esses scripts. Em seguida, o editor de scripts com o botão direito e selecione **HDInsight: interativo do PySpark**.

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
 

## <a name="submit-pyspark-batch-job"></a>Submeter a tarefa de lote do PySpark

1. Crie uma nova pasta de trabalho e um novo ficheiro de script com a extensão. PY se ainda não tivê-los.

2. Ligar à sua conta do Azure, se ainda não o tiver feito.

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
4. O editor de scripts com o botão direito e, em seguida, selecione **HDInsight: PySpark Batch**. 

5. Selecione um cluster ao qual pretende submeter a tarefa de PySpark. 

   ![Submeter o resultado da tarefa de Python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Depois de submeter um trabalho do Python, registos de submissão constar da **saída** janela no VS Code. O **URL de interface do Usuário do Spark** e **URL de IU do Yarn** são mostrados também. É possível abrir o URL num navegador da web para controlar o estado da tarefa.

>[!NOTE]
>PySpark3 já não é suportada no Livy 0.4 (que é o cluster do HDI spark 2.2). Apenas "PySpark" é suportado para o python. Problema que submeter para o spark 2.2 falhar com python3 é conhecido.
   
## <a name="livy-configuration"></a>Configuração do Livy
Configuração do Livy é suportada, foi possível definir as configurações de projeto na pasta de espaço de trabalho. Obter mais detalhes, veja [Leiame do Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

+ As configurações do projeto:

    ![Configuração do Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

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
    | driverCores | Número de núcleos para utilizar para o processo de driver | Int |
    | executorMemory | Quantidade de memória a utilizar por processo de executor | cadeia |
    | executorCores | Número de núcleos para utilizar para cada executor | Int |
    | numExecutors | Número de executores para iniciar a esta sessão | Int |
    | arquivos compactados | Arquivos compactados a ser utilizado nesta sessão | Lista de cadeia de caracteres |
    | fila | O nome da fila YARN para o qual submetido | cadeia |
    | nome | O nome desta sessão | cadeia |
    | Conf | Propriedades de configuração de Spark | Mapa de chave = valor |

    Corpo da Resposta   
    O objeto criado do Batch.

    | nome | descrição | tipo | 
    | :- | :- | :- | 
    | ID | O id de sessão | Int | 
    | appId | O id de aplicação desta sessão |  Cadeia |
    | appInfo | As informações detalhadas de aplicação | Mapa de chave = valor |
    | registo | As linhas de registo | lista de cadeias de caracteres |
    | state |   O estado de batch | cadeia |


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

* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Utilizar ferramentas do HDInsight para IntelliJ com a Sandbox da Hortonworks](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Utilizar ferramentas do HDInsight no Azure Toolkit para Eclipse para criar Spark aplicações](spark/apache-spark-eclipse-tool-plugin.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualizar dados do Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Visualizar dados de consulta interativa do Hive com o Power BI no Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Configurar o ambiente interativo do PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Utilizar o Zeppelin para executar consultas do Hive no HDInsight do Azure ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Cenários
* [Spark com BI: Efetuar uma análise de dados interativa com o Spark no HDInsight com ferramentas do BI](spark/apache-spark-use-bi-tools.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Análise de registos de sites com o Spark no HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Criar e executar aplicações
* [Criar uma aplicação autónoma com o Scala](spark/apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Gerir recursos
* [Gerir recursos para o cluster do Apache Spark no Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](spark/apache-spark-job-debugging.md)



