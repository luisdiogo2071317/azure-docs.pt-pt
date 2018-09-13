---
title: Aprendizagem profunda para cenários do mundo real de manutenção preditiva - Azure | Documentos da Microsoft
description: Aprenda a replicar o tutorial sobre a aprendizagem profunda para manutenção Preditiva com o Azure Machine Learning Workbench.
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.devlang: ''
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 83e1f14db317f59ab2063a9d020adbdb6fe78e5f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650453"
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Aprendizagem profunda para cenários do mundo real de manutenção preditiva

Aprendizagem profunda é uma das tendências mais populares no machine learning e tem aplicações em diversas áreas, incluindo:
- Carros de frotas e robótica.
- Reconhecimento de voz e imagem.
- Previsão financeiros.

Também conhecido como redes neurais profundas (DNN), esses métodos são inspirados pelos neurônios individuais que estão dentro do cérebro (redes neurais biológicos).

O impacto do período de indisponibilidade não agendado do equipamento pode ser prejudicial para qualquer negócio. É essencial para manter os equipamentos de campo em execução para maximizar o desempenho e a utilização e minimizar o período de indisponibilidade dispendioso, não agendado. A identificação precoce de problemas pode ajudar a alocar recursos limitados de manutenção de uma forma rentável, melhorar a qualidade e fornecer os processos de cadeia. 

Uma estratégia de manutenção preditiva (horas) utiliza métodos de aprendizagem de máquina para determinar a condição de equipamentos preventivamente realizar a manutenção para evitar o desempenho da máquina adversos. No PM, os dados são recolhidos ao longo do tempo para monitorizar o estado da máquina e, em seguida, analisados para encontrar padrões para prever falhas. [Muito curto prazo memória (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) redes são atraentes para esta definição, uma vez que foram concebidos para aprender com seqüências de dados.

### <a name="cortana-intelligence-gallery-github-repository"></a>Repositório do GitHub de galeria do Cortana Intelligence

Galeria da Cortana Intelligence para o tutorial de PM é um repositório do GitHub público ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) onde pode comunicar problemas e fazer contribuições.


## <a name="use-case-overview"></a>Descrição geral de caso de utilização

Este tutorial utiliza o exemplo de eventos de falha de execução do motor de aeronave simulado para demonstrar a processo de modelagem de manutenção preditiva. O cenário é descrito em [manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3).

A pressuposição principal nesta definição é o mecanismo progressivamente degradado durante sua vida útil. A degradação pode ser detetada no medidas de sensor do motor. PM tenta modelar a relação entre as alterações nestes valores de sensor e as falhas de históricas. O modelo, em seguida, pode prever quando e motor podem falhar no futuro com base no estado atual das medidas de sensor.

Este cenário cria uma rede LSTM para prever a vida útil remanescente (RUL) de motores das aeronaves com valores de históricos do sensor. O cenário utiliza a [Keras](https://keras.io/) biblioteca com o [Tensorflow](https://www.tensorflow.org/) framework de aprendizagem profunda como um motor de computação. O cenário prepara o LSTM com um conjunto de motores e testa a rede num conjunto de mecanismo que não foram vistos.

## <a name="prerequisites"></a>Pré-requisitos
- Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
- O Azure Machine Learning Workbench, com uma área de trabalho criada.
- Da operacionalização do modelo: Operacionalização de Aprendizado de máquina do Azure com um ambiente de implantação local configurado e um [conta de gestão de modelos do Azure Machine Learning](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:

1. Abra o Machine Learning Workbench.
2. Sobre o **projetos** página, selecione **+** e, em seguida, selecione **novo projeto**.
3. Na **criar novo projeto** painel, introduza as informações para o novo projeto.
4. Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Manutenção preditiva" e selecione o **aprendizagem profunda para o cenário de manutenção preditiva** modelo.
5. Selecione **Criar**.

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de computação de servidor do bloco de notas

Para executar no seu computador local, a partir do Machine Learning Workbench **arquivo** menu, selecione **linha de comandos aberta** ou **CLI do PowerShell aberta**. A interface da CLI permite-lhe aceder aos seus serviços do Azure através do `az` comandos. Primeiro, inicie sessão sua conta do Azure com o comando:

```
az login
``` 

Este comando fornece uma chave de autenticação a ser utilizado com o https:\\aka.ms\devicelogin URL. A CLI tem de aguardar até que a operação de início de sessão de dispositivo retorna e fornece algumas informações de ligação. Em seguida, se tiver um local [Docker](https://www.docker.com/get-docker) instalação, preparar o ambiente de computação local com o comando:

```
az ml experiment prepare --target docker --run-configuration docker
```

É preferível executar numa [Máquina Virtual de ciência de dados (DSVM) para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) requisitos de memória e disco. Depois da DSVM estiver configurada, prepare o ambiente do Docker remoto com os dois comandos seguintes:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Depois que estiver ligado ao contentor do Docker remoto, prepare o ambiente de computação DSVM Docker com o comando: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Com o ambiente de computação de Docker preparado, abra o servidor de bloco de notas do Jupyter do Machine Learning Workbench **blocos de notas** separador ou um servidor baseado no browser com o comando: 

```
az ml notebook start
```

Os blocos de notas de exemplo são armazenados no diretório do código. Os blocos de notas são configurados de forma são executados sequencialmente, a partir do primeiro bloco de notas (Code\1_data_ingestion.ipynb). Quando abre cada bloco de anotações, lhe for pedido para selecionar o kernel de computação. Escolha o kernel de modelo [nome_da_conexão] [Project_Name] para executar em DSVM configurado anteriormente.

## <a name="data-description"></a>Descrição de dados

O modelo utiliza três conjuntos de dados como entradas nos ficheiros de PM_train.txt PM_test.txt e PM_truth.txt. 

- **Preparar dados**: A monitorização de motores de dados de falha de execução. Os dados de treinamento (PM_train.txt) consiste em várias, séries de tempo multivariate com *ciclo* como a unidade de tempo. Ele inclui as leituras dos sensores de 21 para cada ciclo. 

    - Cada série de tempo é gerado a partir de um mecanismo de diferente do mesmo tipo. Cada motor começa com diferentes níveis de desgaste pode eventualmente inicial e de algumas variações de fabrico exclusivo. Esta informação é desconhecida para o usuário. 

    - Estes dados simulados, é assumido o mecanismo para estar a funcionar normalmente no início de cada série de tempo. Ele começa a degradar-se em algum momento durante a série de ciclos de funcionamento. A degradação progride e cresce a magnitude. 

    - Quando for atingido um limiar predefinido, o mecanismo é considerado não seguro para ainda mais a operação. O último ciclo de cada série de tempo é o ponto de falha de que o motor.

- **Testar dados**: A monitorização de motores de dados operacionais, sem os eventos de falha registados. Os dados de teste (PM_test.txt) tem o mesmo esquema de dados como os dados de treinamento. A única diferença é que os dados não indicam quando a falha ocorre (o último período de tempo faz *não* representam o ponto de falha). Não se sabe quantos ciclos mais esse mecanismo pode durar antes de falhar.

- **Dados de verdade**: as informações de restantes verdadeiro ciclos para cada mecanismo de dados de teste. Os dados de verdade zero fornecem o número de ciclos de trabalho restantes para os mecanismos de dados de teste.

## <a name="scenario-structure"></a>Estrutura do cenário

O fluxo de trabalho de cenário é dividido em três passos e cada passo é executado num bloco de notas do Jupyter. Cada bloco de anotações produz os artefactos de dados que são mantidos localmente para utilização nos blocos de notas.

### <a name="task-1-data-ingestion-and-preparation"></a>Tarefa 1: Ingestão de dados e a preparação

As notas do Jupyter de ingestão de dados no Code/1_data_ingestion_and_preparation.ipnyb carrega os três conjuntos de dados de entrada para o formato de Pandas DataFrame. O bloco de notas, em seguida, prepara os dados de modelagem e faz alguma visualização de dados preliminar. Os conjuntos de dados são armazenados localmente para o contexto de cálculo para utilização no bloco de notas do Jupyter de construção do modelo.

### <a name="task-2-model-building-and-evaluation"></a>Tarefa 2: Criação de modelo e avaliação

O modelo de criação de notas do Jupyter no Code/2_model_building_and_evaluation.ipnyb lê os conjuntos de dados de formação e teste do disco e cria uma rede LSTM para o conjunto de dados de treinamento. O desempenho do modelo é medido no conjunto de dados de teste. O modelo resultante é serializado e armazenado no contexto de cálculo local para utilização na tarefa de operacionalização.

### <a name="task-3-operationalization"></a>Tarefa 3: Operacionalização

O bloco de notas do Jupyter Operacionalização no Code/3_operationalization.ipnyb utiliza o modelo de armazenado para criar as funções e o esquema para chamar o modelo de serviço web alojado no Azure. O bloco de notas testa as funções e, em seguida, compacta os ativos para o ficheiro de LSTM_o16n.zip. O arquivo é carregado para o contentor de armazenamento do Azure para a implementação.

O ficheiro de implementação de LSTM_o16n.zip contém os seguintes artefatos:

- **webservices_conda.yaml**: define os pacotes de Python que são necessários para executar o modelo LSTM no destino de implementação.  
- **service_schema**: define o esquema de dados que é esperado pelo modelo LSTM.   
- **lstmscore.PY**: define as funções que o destino de implementação está a executar a pontuação novos dados.    
- **modellstm.JSON**: define a arquitetura LSTM. As funções de lstmscore.py ler a arquitetura e os pesos para inicializar o modelo.
- **modellstm.H5**: define os pesos de modelo.
- **test_service.PY**: um script de teste que chama o ponto de final de implementação com o teste de registos de dados. 
- **PM_test_files.pkl**: O script de test_service.py lê os dados históricos de motor do arquivo PM_test_files.pkl e envia o serviço web ciclos suficientes para LSTM retornar uma probabilidade de falha do motor.

O bloco de notas testa as funções utilizando a definição de modelo antes de ele os ativos de operacionalização para a implementação de pacotes. Instruções para configurar e testar o serviço web estão incluídas no final do bloco de notas Code/3_operationalization.ipnyb.

## <a name="conclusion"></a>Conclusão

Este tutorial fornece um cenário simples que utiliza valores de sensor para fazer previsões. Cenários de manutenção preditiva, como mais avançados os [preditiva manutenção modelagem guia R bloco de notas](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) pode utilizar várias origens de dados, como registos de manutenção de histórico, registos de erros e funcionalidades de máquina. Origens de dados adicionais podem exigir diferentes tratamentos para utilizar com aprendizagem profunda.


## <a name="references"></a>Referências

As referências seguintes fornecem exemplos de outra manutenção preditiva casos de utilização para várias plataformas:

* [Modelo de solução de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Guia de modelagem de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Preditiva manutenção modelagem guia usando serviços R do SQL](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [O bloco de notas do guia Python de modelagem de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Manutenção Preditiva com PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Cenários do mundo real de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance)

## <a name="next-steps"></a>Passos Seguintes

Outros cenários de exemplo estão disponíveis no Machine Learning Workbench demonstrar funcionalidades adicionais do produto. 
