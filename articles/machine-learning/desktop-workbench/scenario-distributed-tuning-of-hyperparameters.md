---
title: Distribuído de otimização de Hiperparâmetros com o Azure Machine Learning Workbench | Documentos da Microsoft
description: Este cenário mostra como fazer otimização distribuída de hiperparâmetros com o Azure Machine Learning Workbench
services: machine-learning
author: pechyony
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.author: dmpechyo
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: f74889cdf727bc132723d16df295849769001ce9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951972"
---
# <a name="distributed-tuning-of-hyperparameters-using-azure-machine-learning-workbench"></a>Otimização distribuída de hiperparâmetros com o Azure Machine Learning Workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Este cenário mostra como utilizar o Azure Machine Learning Workbench para aumentar horizontalmente o ajuste de hiperparâmetros de algoritmos de machine learning que implementam scikit-saiba API. Vamos mostrar como configurar e utilizar um cluster do Spark e um contentor do Docker remoto como um back-end de execução para o ajuste de hiperparâmetros.

## <a name="link-of-the-gallery-github-repository"></a>Ligação do repositório do GitHub de galeria
Segue-se a ligação para o repositório do GitHub público: 

[https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning)

## <a name="use-case-overview"></a>Descrição geral de caso de utilização

Muitos algoritmos de machine learning tem uma ou mais botões, chamados hiperparâmetros. Esses botões permitem a otimização de algoritmos para otimizar o seu desempenho sobre dados futuros, medidos de acordo com as métricas especificadas pelo utilizador (por exemplo, a precisão, AUC, RMSE). Cientista de dados tem de fornecer valores de hiperparâmetros quando criar um modelo de dados de treinamento e antes de ver os dados de teste futuras. Como com base na lata de dados de treinamento conhecidos, configuramos os valores de hiperparâmetros para que o modelo tem um bom desempenho, os dados de teste desconhecido? 
    
Uma técnica popular para o ajuste de hiperparâmetros é um *pesquisa de grade* combinado com *validação cruzada*. A validação cruzada é uma técnica que avalie o quão bem um modelo, com base em com um conjunto de treinamento, prevê sobre o conjunto de teste. Usando essa técnica, vamos primeiro dividir o conjunto de dados em subconjuntos K e, depois, prepara os tempos de K de algoritmo de uma forma round robin. Podemos fazê-lo em todos, mas uma dos subconjuntos chamada "dobra mantidos-out". O valor médio das métricas de modelos de K, computamos ao longo de subconjuntos de escalamento mantidos K. Este valor médio, chamado *estimativa de desempenho entre validados*, depende dos valores de hiperparâmetros utilizados durante a criação de modelos de K. Quando o ajuste hiperparâmetros, estamos a pesquisar o espaço de valores de hiper-parâmetros Release candidate para encontrar aqueles que otimizar o desempenho de validação cruzada estimar. Pesquisa de grade é uma técnica comum de pesquisa. Na pesquisa de grade, o espaço de valores de candidatos de hiperparâmetros vários é um produto cruzado de conjuntos de valores de candidatos de hiperparâmetros individuais. 

Pesquisa de grade com a validação cruzada pode ser demorada. Se um algoritmo tem cinco hiperparâmetros com cinco valores de candidatos, usamos subconjuntos K = 5. Podemos, em seguida, concluir uma pesquisa de grade por 5 de treinamento<sup>6</sup>= 15625 modelos. Felizmente, a grade-pesquisa usando a validação cruzada é um procedimento de constrangedoramente paralelo e todos os esses modelos podem ser treinados em paralelo.

## <a name="prerequisites"></a>Pré-requisitos

* Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
* Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [instalar e criar o guia de introdução](quickstart-installation.md) para instalar o Workbench e criar contas.
* Este cenário pressupõe que está a executar Azure ML Workbench no Windows 10 ou MacOS com o motor de Docker instalado localmente. 
* Para executar o cenário com um contentor do Docker remoto, aprovisionar a Máquina Virtual de ciência de dados do Ubuntu (DSVM) ao seguir a [instruções](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-provision-vm). Recomendamos que utilize uma máquina virtual com, pelo menos, 8 núcleos e memória de 28 Gb. D4 instâncias de máquinas virtuais têm essa capacidade. 
* Para executar este cenário com um cluster do Spark, Aprovisione o cluster do HDInsight Spark através destas [instruções](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters). Recomendamos ter um cluster com a seguinte configuração em nós de cabeçalho e de trabalho:
    - quatro nós de trabalho
    - oito núcleos
    - 28 Gb de memória  
      
  D4 instâncias de máquinas virtuais têm essa capacidade. 

     **Resolução de problemas**: sua subscrição do Azure pode ter uma quota no número de núcleos que podem ser utilizados. O portal do Azure não permite a criação de cluster com o número total de núcleos que excede a quota. Para encontrá-lo de quota, aceda a no portal do Azure para a secção de subscrições, clique na subscrição utilizada para implementar um cluster e, em seguida, clique em **utilização + quotas**. Normalmente, as quotas são definidas por região do Azure e pode optar por implementar o cluster do Spark numa região em que tem núcleos suficientes gratuitos. 

* Crie uma conta de armazenamento do Azure que é utilizada para armazenar o conjunto de dados. Siga os [instruções](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) para criar uma conta de armazenamento.

## <a name="data-description"></a>Descrição de dados

Usamos [conjunto de dados de TalkingData](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data). Este conjunto de dados tem eventos das aplicações em celulares. O objetivo é prever a categoria de idade e género de usuário de telefone celular, Considerando o tipo de telefone e os eventos que o usuário gerado recentemente.  

## <a name="scenario-structure"></a>Estrutura do cenário
Este cenário tem várias pastas no repositório do GitHub. Ficheiros de código e a configuração estão na **código** pasta, toda a documentação está no **Docs** pasta e todas as imagens são **imagens** pasta. A pasta de raiz tem o ficheiro Leia-me que contém um breve resumo deste cenário.

### <a name="getting-started"></a>Introdução
Clique no ícone do Azure Machine Learning Workbench para executar o Azure Machine Learning Workbench e criar um projeto a partir do modelo de "Distributed ajuste de Hiperparâmetros". Pode encontrar instruções detalhadas sobre como criar um novo projeto no [instalar e criar o guia de introdução](quickstart-installation.md).   

### <a name="configuration-of-execution-environments"></a>Configuração de ambientes de execução
Vamos mostrar como executar nosso código num contentor do Docker remoto e, num cluster do Spark. Vamos começar com a descrição das definições que são comuns para os dois ambientes. 

Usamos [scikit-Saiba](https://anaconda.org/conda-forge/scikit-learn), [xgboost](https://anaconda.org/conda-forge/xgboost), e [azure-storage](https://pypi.python.org/pypi/azure-storage) pacotes que não são fornecidos no contentor de Docker de padrão do Azure Machine Learning Workbench. pacote de armazenamento do Azure requer a instalação do [criptografia](https://pypi.python.org/pypi/cryptography) e [azure](https://pypi.python.org/pypi/azure) pacotes. Para instalar estes pacotes no contentor do Docker e em nós de cluster do Spark, podemos modificar conda_dependencies.yml ficheiro:

    name: project_environment
    channels:
      - conda-forge
    dependencies:
      - python=3.5.2
      - scikit-learn
      - xgboost
      - pip:
        - cryptography
        - azure
        - azure-storage

O conda modificado\_dependencies.yml ficheiro é armazenado no diretório de aml_config do tutorial. 

Os passos seguintes, vamos ligar o ambiente de execução para a conta do Azure. Clique em ficheiro Menu do canto superior esquerdo da bancada de trabalho de AML. E escolha "linha de comandos aberta". Em seguida, execute na CLI

    az login

Receberá uma mensagem

    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code <code> to authenticate.

Vá para essa página da web, introduza o código e iniciar sessão na sua conta do Azure. Após este passo, executar na CLI

    az account list -o table

e localizar o ID de subscrição do Azure que tenha a sua conta da área de trabalho do AML Bancada de trabalho. Por fim, execute na CLI

    az account set -s <subscription ID>

para concluir a ligação à sua subscrição do Azure.

As duas secções seguintes, vamos mostrar como concluir a configuração do docker remoto e o cluster do Spark.

#### <a name="configuration-of-remote-docker-container"></a>Configuração de contentor do Docker remoto

 Para configurar o contentor do Docker remoto, execute na CLI

    az ml computetarget attach remotedocker --name dsvm --address <IP address> --username <username> --password <password> 

com o IP endereço, nome de utilizador e palavra-passe na DSVM. Pode encontrar o endereço IP da DSVM na seção de visão geral da página da DSVM no portal do Azure:

![IP da VM](media/scenario-distributed-tuning-of-hyperparameters/vm_ip.png)

#### <a name="configuration-of-spark-cluster"></a>Configuração de cluster do Spark

Para configurar o ambiente de Spark, execute na CLI

    az ml computetarget attach cluster --name spark --address <cluster name>-ssh.azurehdinsight.net  --username <username> --password <password> 

com o nome do cluster, o nome de utilizador SSH do cluster e palavra-passe. O valor predefinido do nome de utilizador SSH é `sshuser`, a menos que tenha alterado durante o aprovisionamento do cluster. O nome do cluster pode ser encontrado na seção de propriedades da sua página de cluster no portal do Azure:

![Nome do cluster](media/scenario-distributed-tuning-of-hyperparameters/cluster_name.png)

Utilizamos o pacote do spark-sklearn ter Spark como ambiente de execução de otimização distribuída de hiperparâmetros. Modificamos spark_dependencies.yml arquivo para instalar este pacote quando o ambiente de execução do Spark é usado:

    configuration: 
      #"spark.driver.cores": "8"
      #"spark.driver.memory": "5200m"
      #"spark.executor.instances": "128"
      #"spark.executor.memory": "5200m"  
      #"spark.executor.cores": "2"
  
    repositories:
      - "https://mmlspark.azureedge.net/maven"
      - "https://spark-packages.org/packages"
    packages:
      - group: "com.microsoft.ml.spark"
        artifact: "mmlspark_2.11"
        version: "0.7.91"
      - group: "databricks"
        artifact: "spark-sklearn"
        version: "0.2.0"

O spark modificado\_dependencies.yml ficheiro é armazenado no diretório de aml_config do tutorial. 

### <a name="data-ingestion"></a>Ingestão de dados
O código neste cenário pressupõe que os dados são armazenados no armazenamento de Blobs do Azure. Vamos mostrar inicialmente como transferir dados do site de Kaggle para o seu computador e carregue-o para o armazenamento de Blobs. Em seguida, vamos mostrar como ler os dados de armazenamento de Blobs. 

Para transferir dados da Kaggle, aceda a [página do conjunto de dados](https://www.kaggle.com/c/talkingdata-mobile-user-demographics/data) e clique em botão Download. Será solicitado para iniciar sessão no Kaggle. Após iniciar sessão, será redirecionado para a página de conjunto de dados. Em seguida, transferir os ficheiros de sete em primeiro lugar na coluna esquerda selecionando-os e clicar no botão Transferir. O tamanho total dos ficheiros transferidos é 289 Mb. Para carregar esses arquivos para armazenamento de BLOBs, crie contentor de armazenamento de BLOBs "conjunto de dados" na conta de armazenamento. Pode fazê-lo acedendo à página do Azure da sua conta de armazenamento, clicar em Blobs e, em seguida, clicando em + contentor. Introduza "conjunto de dados" como nome e clique em OK. As capturas de ecrã seguintes mostram estes passos:

![Open blob](media/scenario-distributed-tuning-of-hyperparameters/open_blob.png)
![contentor aberto](media/scenario-distributed-tuning-of-hyperparameters/open_container.png)

Depois disso, selecione o contentor de conjunto de dados da lista e clique em botão de carregamento. Portal do Azure permite-lhe carregar vários ficheiros ao mesmo tempo. Na secção "Carregar blob" clique botão de pasta, selecione todos os ficheiros do conjunto de dados, clique em abrir e, em seguida, clique em carregar. Captura de ecrã seguinte ilustra estes passos:

![Carregar blob](media/scenario-distributed-tuning-of-hyperparameters/upload_blob.png) 

Carregamento dos ficheiros demora vários minutos, consoante a ligação à Internet. 

No nosso código, utilizamos [SDK de armazenamento do Azure](https://docs.microsoft.com/python/azure/) para transferir o conjunto de dados do armazenamento de BLOBs para o ambiente de execução atual. A transferência é efetuada na carga\_função de data () do ficheiro de load_data.py. Para utilizar este código, é necessário substituir < nome_conta > e < ACCOUNT_KEY > com o nome e chave primária da conta de armazenamento que aloja o conjunto de dados. Pode ver o nome da conta no canto superior esquerdo da página da conta de armazenamento do Azure. Para obter a conta de chave, selecione as chaves de acesso na página de armazenamento do Azure (veja a captura de ecrã primeiro na secção de ingestão de dados) da conta e, em seguida, copie a cadeia de caracteres longa na primeira linha da coluna chave:
 
![Chave de acesso](media/scenario-distributed-tuning-of-hyperparameters/access_key.png)

O código seguinte da função de load_data() transfere um único arquivo:

    from azure.storage.blob import BlockBlobService

    # Define storage parameters 
    ACCOUNT_NAME = "<ACCOUNT_NAME>"
    ACCOUNT_KEY = "<ACCOUNT_KEY>"
    CONTAINER_NAME = "dataset"

    # Define blob service     
    my_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # Load blob
    my_service.get_blob_to_path(CONTAINER_NAME, 'app_events.csv.zip', 'app_events.csv.zip')

Tenha em atenção que não é necessário executar o ficheiro de load_data.py manualmente. Ele é chamado de outros ficheiros mais tarde.

### <a name="feature-engineering"></a>Com engenharia
O código para todos os recursos de computação está em funcionalidade\_engineering.py ficheiro. Não é necessário executar o ficheiro de feature_engineering.py manualmente. Mais tarde será chamado de outros ficheiros.

Podemos criar vários conjuntos de recursos:
* Frequente de uma codificação de marca e modelo do telefone celular (um\_frequente\_brand_model função)
* Fração de eventos gerados pelo utilizador em cada dia da semana (dia da semana\_hour_features função)
* Fração de eventos gerados pelo utilizador em cada hora (dia da semana\_hour_features função)
* Fração de eventos gerados pelo utilizador em cada combinação de dia da semana e hora (dia da semana\_hour_features função)
* Fração de eventos gerados pelo utilizador em cada aplicação (um\_frequente\_app_labels função)
* Fração de eventos gerados pelo utilizador em cada etiqueta de aplicação (um\_frequente\_app_labels função)
* Fração de eventos gerados pelo utilizador em cada categoria de aplicação (texto\_category_features função)
* Recursos de indicador categorias de aplicações que foram utilizadas para geraram eventos (um\_hot_category função)

Esses recursos foram inspirados pelo Kaggle kernel [um modelo linear em aplicações e as etiquetas](https://www.kaggle.com/dvasyukova/a-linear-model-on-apps-and-labels).

A computação destas funcionalidades requer uma quantidade significativa de memória. Inicialmente tentar calcular recursos no ambiente local com 16 GB de RAM. Podemos conseguiram-se de que os primeiros quatro conjuntos de recursos de computação, mas foram recebidos erros 'sem memória' ao computar o quinto conjunto de recursos. A computação dos conjuntos de funcionalidades de quatro primeiras está no arquivo de singleVMsmall.py e pode ser executado no ambiente local através da execução 

     az ml experiment submit -c local .\singleVMsmall.py   

na janela da CLI.

Uma vez que o ambiente local é demasiado pequeno para todos os conjuntos de recursos de computação, vamos mudar para DSVM remoto que tenha mais memória. A execução no interior da DSVM é feita no interior do contentor do Docker que é gerenciado pela Bancada de trabalho de AML. Usando este DSVM é possível para todos os recursos de computação e preparar modelos e otimizar hiperparâmetros (veja a secção seguinte). o ficheiro de singleVM.py tem o código de modelagem e de computação de recurso completo. Na próxima seção, mostraremos como executar singleVM.py na DSVM remoto. 

### <a name="tuning-hyperparameters-using-remote-dsvm"></a>Ajuste hiperparâmetros usando DSVM remoto
Usamos [xgboost](https://anaconda.org/conda-forge/xgboost) [1] de implementação de árvore gradação adaptativo. Também utilizamos [scikit-Saiba](http://scikit-learn.org/) pacote para otimizar hiperparâmetros de xgboost. Embora xgboost não faz parte do scikit-saiba pacote, ele implementa scikit-saiba API e, por conseguinte, podem ser utilizados em conjunto com as funções de scikit de otimização de hiper-parâmetros-Saiba mais. 

Xgboost tem oito hiperparâmetros, descritos [aqui](https://github.com/dmlc/xgboost/blob/master/doc/parameter.md):
* n_estimators
* max_depth
* reg_alpha
* reg_lambda
* colsample\_by_tree
* learning_rate
* colsample\_by_level
* subsample
* Objetivo  
 
Inicialmente, nós usamos DSVM remoto e otimizar hiperparâmetros de uma pequena grade valores candidatos:

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1],'learning_rate': [0.1], 'colsample_bylevel': [0.1,], 'subsample': [0.5]}]  

Essa grade tem quatro combinações de valores de hiperparâmetros. Usamos 5-fold validação cruzada, resultando em 4 x 5 = 20 execuções do xgboost. Para medir o desempenho dos modelos, usamos de métrica de perda de log negativo. O código a seguir encontra os valores de hiperparâmetros da grelha que maximizem a perda de validados entre log negativo. O código também utiliza estes valores para preparar o modelo final sobre o conjunto completo de treinamento:

    clf = XGBClassifier(seed=0)
    metric = 'neg_log_loss'
    
    clf_cv = GridSearchCV(clf, tuned_parameters, scoring=metric, cv=5, n_jobs=8)
    model = clf_cv.fit(X_train,y_train)

Depois de criar o modelo, vamos salvar os resultados de otimização de hiper-parâmetros. Podemos utilizar o registo API de AML Bancada de trabalho para guardar os melhores valores de hiperparâmetros e correspondente estimativa entre validados de perda de log negativo:

    from azureml.logging import get_azureml_logger

    # initialize logger
    run_logger = get_azureml_logger()

    ...

    run_logger.log(metric, float(clf_cv.best_score_))

    for key in clf_cv.best_params_.keys():
        run_logger.log(key, clf_cv.best_params_[key]) 

Também podemos criar o ficheiro de sweeping_results.txt com perdas de registo entre validados, negativo de todas as combinações de valores de hiper-parâmetros na grade.

    if not path.exists('./outputs'):
        makedirs('./outputs')
    outfile = open('./outputs/sweeping_results.txt','w')

    print("metric = ", metric, file=outfile)
    for i in range(len(model.grid_scores_)):
        print(model.grid_scores_[i], file=outfile)
    outfile.close()

Esse arquivo é armazenado num especial. / produz o diretório. Mais tarde, vamos mostrar como transferi-la.  

 Antes de executar singleVM.py na DSVM remoto pela primeira vez, criamos um contentor de Docker lá, executando 

    az ml experiment prepare -c dsvm

no windows CLI. Contentor de criação de Docker demora vários minutos. Depois disso, execute singleVM.py DSVM:

    az ml experiment submit -c dsvm .\singleVM.py

Este comando ser concluído em 1 hora de 38 minutos quando a DSVM tem 8 núcleos e memória de 28 Gb. Os valores com sessão iniciada podem ser visualizados na janela do histórico de execuções de AML Bancada de trabalho:

![Histórico de execuções](media/scenario-distributed-tuning-of-hyperparameters/run_history.png)

Por predefinição janela do histórico de execuções mostra valores e gráficos dos primeiros valores de 1 a 2 com sessão iniciada. Para ver a lista completa dos valores escolhidos de hiperparâmetros, clique no ícone das definições marcado com um círculo vermelho na captura de ecrã anterior. Em seguida, selecione os hiperparâmetros a serem apresentados na tabela. Além disso, para selecionar os gráficos que são apresentados na parte superior da janela do histórico de execuções, clique no ícone de definição marcado com o círculo azul e selecione os gráficos da lista. 

Os valores escolhidos de hiperparâmetros também podem ser examinados na janela de propriedades da execução: 

![Propriedades de execução](media/scenario-distributed-tuning-of-hyperparameters/run_properties.png)

No canto superior direito da janela de propriedades da execução, há uma seção arquivos de saída com a lista de todos os ficheiros que foram criados no '. \output "pasta. varrimento\_results.txt pode ser transferido a partir daí, selecionando-lo e clicar no botão de transferência. sweeping_results.txt deve ter a seguinte saída:

    metric =  neg_log_loss
    mean: -2.29096, std: 0.03748, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28712, std: 0.03822, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 3}
    mean: -2.28706, std: 0.03863, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 300, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}
    mean: -2.28530, std: 0.03927, params: {'colsample_bytree': 1, 'learning_rate': 0.1, 'subsample': 0.5, 'n_estimators': 400, 'reg_alpha': 1, 'objective': 'multi:softprob', 'colsample_bylevel': 0.1, 'reg_lambda': 1, 'max_depth': 4}

### <a name="tuning-hyperparameters-using-spark-cluster"></a>Ajuste hiperparâmetros com o cluster do Spark
Usamos o cluster do Spark para aumentar horizontalmente o ajuste hiperparâmetros e utilizar de grade maior. É a nossa nova grade

    tuned_parameters = [{'n_estimators': [300,400], 'max_depth': [3,4], 'objective': ['multi:softprob'], 'reg_alpha': [1], 'reg_lambda': [1], 'colsample_bytree': [1], 'learning_rate': [0.1], 'colsample_bylevel': [0.01, 0.1], 'subsample': [0.5, 0.7]}]

Essa grade tem 16 combinações de valores de hiperparâmetros. Uma vez que utilizamos 5-fold validação cruzada, executamos xgboost 16 x 5 = 80 horas.

scikit-saiba pacote não tem um suporte nativo de ajuste de hiperparâmetros com o cluster do Spark. Felizmente, [spark sklearn](https://spark-packages.org/package/databricks/spark-sklearn) pacote do Databricks preenche essa lacuna. Este pacote fornece a função de GridSearchCV com quase a mesma API como função GridSearchCV no scikit-Saiba mais. Para utilizar o spark sklearn e otimizar hiperparâmetros com o Spark, precisamos criar um contexto de Spark

    from pyspark import SparkContext
    sc = SparkContext.getOrCreate()

Em seguida, substituímos 

    from sklearn.model_selection import GridSearchCV

com 

    from spark_sklearn import GridSearchCV

Também foi substituir a chamada para GridSearchCV de scikit-Saiba como aquele do spark-sklearn:

    clf_cv = GridSearchCV(sc = sc, param_grid = tuned_parameters, estimator = clf, scoring=metric, cv=5)

É o código final para o ajuste de hiperparâmetros com o Spark no distribuído\_sweep.py ficheiro. É a diferença entre singleVM.py e distributed_sweep.py na definição da grade e adicionais quatro linhas de código. Observe também que devido a serviços de AML Bancada de trabalho, o código de registo não é alterada quando a alteração do ambiente de execução da DSVM remoto para o cluster do Spark.

Antes de executar distributed_sweep.py num cluster do Spark pela primeira vez, é necessário instalar pacotes de Python lá. Isso pode ser conseguido através da execução 

    az ml experiment prepare -c spark

no windows CLI. Esta instalação demora vários minutos. Depois que executamos distributed_sweep.py num cluster do Spark:

    az ml experiment submit -c spark .\distributed_sweep.py

Este comando ser concluído em 1 hora 6 minutos quando o cluster do Spark tem 6 nós de trabalho com 28 Gb de memória. Os resultados de otimização de hiper-parâmetros podem ser acessados da mesma forma que a execução remota da DSVM no Azure Machine Learning Workbench. (ou seja, registos, melhores valores de hiperparâmetros e sweeping_results.txt ficheiro)

### <a name="architecture-diagram"></a>Diagrama da arquitetura

O diagrama seguinte mostra o fluxo de trabalho geral: ![arquitetura](media/scenario-distributed-tuning-of-hyperparameters/architecture.png) 

## <a name="conclusion"></a>Conclusão 

Neste cenário, mostramos como utilizar o Azure Machine Learning Workbench para executar a otimização de hiperparâmetros em máquinas virtuais remotas e os clusters do Spark. Vimos que o Azure Machine Learning Workbench fornece ferramentas para uma configuração fácil de ambientes de execução. Ela também permite alternar facilmente entre eles. 

## <a name="references"></a>Referências

[1] T. Chen e C. Guestrin. [XGBoost: Árvore dimensionável adaptativo sistema](https://arxiv.org/abs/1603.02754). KDD 2016.




