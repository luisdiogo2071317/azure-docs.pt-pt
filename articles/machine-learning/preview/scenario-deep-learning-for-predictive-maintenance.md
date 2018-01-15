---
title: "Learning avançada para cenários no mundo real de manutenção preditiva - Azure | Microsoft Docs"
description: "Saiba como replicar o tutorial num learning profunda de manutenção Preditiva com o Azure Machine Learning Workbench."
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: a55209256c29fa62cc2da72f9653fbc7fc0e7c54
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenarios"></a>Learning avançada para cenários no mundo real de manutenção preditiva

Learning profunda é um das tendências mais populares no machine learning. Learning profunda é utilizado em vários campos e aplicações, incluindo carros driverless, reconhecimento de voz e imagem, robotics e financeiros. Learning profunda é um conjunto de algoritmos que é inspired pela forma de "-brain" (biological as redes neurais) e a aprendizagem. Normalmente, consulte cientistas cognitivos profunda learning como as redes neurais artificial (ANNs).

Manutenção preditiva também é popular. Em manutenção preditiva, muitos técnicas diferentes foram concebidas para ajudar a determinar a condição do equipamento e para prever quando deve ser efetuada a manutenção. Algumas utilizações comuns de manutenção preditiva são predição de falha, diagnóstico de falha (análise da causa raiz), a deteção de falha, classificação de tipo de falha e recomendação das ações de mitigação ou manutenção após falha.

Em cenários de manutenção preditiva, os dados são recolhidos ao longo do tempo para monitorizar o estado de equipamento. O objetivo é encontrar padrões que podem ajudar a prever e, em última análise evitar falhas. Utilizar [muito curto prazo memória (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) redes é uma avançada learning método que é especialmente apelativos para resolver em manutenção preditiva. Redes LSTM são eficiente a aprendizagem da sequências. Dados de séries de tempo podem ser utilizados para observar durante períodos de tempo para detetar padrões de falha longos novamente.

Neste tutorial, iremos criar uma rede LSTM para o conjunto de dados e o cenário que são descritas em [manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3). Podemos utilizar a rede para prever a vida útil restantes dos motores das aeronaves. O modelo utiliza valores de sensores simulados aeronave para prever quando um motor de aeronave falhará no futuro. Utilizar este predição, manutenção pode ser planeada seguinte com antecedência, para impedir a falha.

Este tutorial utiliza o [Keras](https://keras.io/) profunda learning biblioteca e o Toolkit de cognitivos [CNTK](https://docs.microsoft.com/cognitive-toolkit/Using-CNTK-with-Keras) como um back-end.

O repositório do GitHub público com os exemplos deste tutorial é [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance).

## <a name="use-case-overview"></a>Descrição geral de cenário de utilização

Este tutorial utiliza o exemplo de eventos de executar a falha de motor de aeronave simulada para demonstrar a processo de modelação de manutenção preditiva. 

Pressuposto implícito dos dados de modelação descritos aqui é que o elemento tem um padrão de degradação suspenso. O padrão é refletido em medidas do sensor do elemento. Ao examinar os valores de sensor o elemento ao longo do tempo, o algoritmo do machine learning pode obter a relação entre os valores do sensor, alterações nos valores do sensor e falhas de histórico. Esta relação é utilizada para prever falhas no futuro. 

Sugerimos que examinar o formato dos dados e todos os três passos do modelo de ser concluído antes de substituir os dados de exemplo com a seus próprios data.

## <a name="prerequisites"></a>Pré-requisitos

- Um [conta do Azure](https://azure.microsoft.com/free/) (gratuitas estão disponíveis).
- Azure Workbench do Machine Learning, com uma área de trabalho criada.
- Para operationalization do modelo: Operationalization de aprendizagem máquina do Azure, com um ambiente de implementação de local configurar e um [conta de gestão de modelo do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview).

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto ao utilizar este exemplo como um modelo:

1. Abra do Machine Learning Workbench.
2. No **projetos** página, selecione  **+** e, em seguida, selecione **novo projeto**.
3. No **criar novo projeto** painel, introduza as informações para o novo projeto.
4. No **modelos de projeto de pesquisa** caixa de pesquisa, introduza **manutenção preditiva**e, em seguida, selecione o modelo.
5. Selecione **Criar**.

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de cálculo de servidor do bloco de notas

No computador local, no Workbench do Machine Learning **ficheiro** menu, selecione **abra a linha de comandos** ou **PowerShell aberta**. Na janela da linha de comandos da opção que escolher, execute os seguintes comandos:

`az ml experiment prepare --target docker --run-configuration docker`

Sugerimos que executar o servidor de bloco de notas uma norma DS4_V2 [Máquina Virtual de ciência de dados (DSVM) para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Depois do DSVM estiver configurado, execute os comandos seguintes para preparar as imagens de Docker:

`az ml computetarget attach remotedocker --name [connection_name] --address [VM_IP_address] --username [VM_username] --password [VM_password]`

`az ml experiment prepare --target [connection_name] --run-configuration [connection_name]`

Com as imagens de Docker preparadas, abra o servidor de bloco de notas do Jupyter. Para abrir o servidor de bloco de notas do Jupyter, o vá para o Workbench do Machine Learning **blocos de notas** separador ou iniciar um servidor baseado em browser:`az ml notebook start`

Blocos de notas são armazenados no diretório do código no ambiente do Jupyter. Execute estes blocos de notas sequencialmente como numerado, começando pelo Code\1_data_ingestion_and_preparation.ipynb.

Selecione o kernel para corresponder ao seu [project_name] modelos [connection_name] e, em seguida, selecione **definir Kernel**.

## <a name="data-description"></a>Descrição de dados

O modelo utiliza três conjuntos de dados como entradas, nos ficheiros de PM_train.txt, PM_test.txt e PM_truth.txt.

-  **Preparar dados**: dados de falha de execução de motor da aeronave. Os dados de formação (PM_train.txt) é composta por vários séries de tempo multivariate, com *ciclo* como a unidade de tempo. Inclui as leituras dos sensores de 21 para cada ciclo. 

    Cada série de tempo pode ser considerado como gerado a partir de um motor de diferentes do mesmo tipo. Cada motor é assumido para começar a utilizar graus diferentes de atuais de desgaste inicial e a variação de fabrico. Esta informação é desconhecida ao utilizador. 

    O motor destes dados simulados, pressupõe-se a estar a funcionar normalmente no início de cada série de tempo. Começa como degradada em algum momento durante a série de ciclos de sistema operativos. A degradação avança e crescimentos de magnitude. 

    Quando for atingido um limiar predefinido, o motor de é considerado não seguro para continuar a operação. O último ciclo de cada série de tempo pode ser considerado o ponto de falha do motor correspondente.

-   **Testar dados**: A aeronave motor dados operacionais, sem eventos de falha registados. Os dados de teste (PM_test.txt) tem o mesmo esquema de dados como os dados de preparação. A única diferença é que os dados não indicam quando ocorre a falha (o período de tempo último *não* representam o ponto de falha). Não é conhecido como muitas ciclos mais este motor de pode última antes de falhar.

- **Dados truth**: as informações de restantes verdadeiro ciclos de cada motor nos dados de teste. Os dados de truth zero fornece o número de ciclos de trabalho restantes para os motores de dados de teste.

## <a name="scenario-structure"></a>Estrutura do cenário

O conteúdo para o cenário está disponível no [repositório do GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance). 

No repositório de um ficheiro de [Leia-me] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md) descreve os processos, desde a preparação de dados de criação e operacionalizar o modelo. Os blocos de notas do Jupyter três estão disponíveis na pasta [código] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) no repositório. 

Em seguida, vamos descrever o fluxo de trabalho do cenário passo a passo.

### <a name="task-1-data-ingestion-and-preparation"></a>Tarefa 1: Ingestão de dados e a preparação

Notas do Jupyter de ingestão de dados no Code/1_data_ingestion_and_preparation.ipnyb carrega os três conjuntos de dados de entrada num ficheiro em formato dataframe Pandas. Em seguida, prepara os dados para a modelação e algumas visualização de dados preliminar. Os dados, em seguida, são transformados PySpark formato e armazenados num contentor de armazenamento de Blobs do Azure na sua subscrição do Azure para utilização na próxima tarefa de modelação.

### <a name="task-2-model-building-and-evaluation"></a>Tarefa 2: Modelo edifício e avaliação

O modelo de edifício de notas do Jupyter no Code/2_model_building_and_evaluation.ipnyb leituras o PySpark dar formação e testar conjuntos de dados do armazenamento de Blobs. Em seguida, uma rede LSTM baseia-se com os conjuntos de dados de formação. O desempenho do modelo é medido no conjunto de teste. O modelo resultante é serializado e armazenado no contexto de computação local para utilização na tarefa operationalization.

### <a name="task-3-operationalization"></a>Tarefa 3: Operationalization

O bloco de notas do Jupyter no Code/3_operationalization.ipnyb de operationalization utiliza o modelo armazenado a criação de funções e o esquema que são necessárias para chamar o modelo de um serviço web alojado no Azure. O bloco de notas testa as funções e, em seguida, zips ficheiro (comprime) os ativos de operationalization num. zip.

O ficheiro zipped contém estes ficheiros:

- **modellstm.JSON**: O ficheiro de definição de esquema para a implementação. 
- **lstmscore.PY**: O **init()** e **run()** funções, que são necessários pelo serviço web do Azure.
- **lstm.Model**: O diretório de definição do modelo.

O bloco de notas testa as funções utilizando a definição de modelo antes de que os ativos de operationalization para a implementação de pacotes. Instruções de implementação são incluídas no fim do bloco de notas.


## <a name="conclusion"></a>Conclusão

Este tutorial utiliza um cenário simples que dados apenas uma origem (valores sensor) é utilizada para tornar as predições. Em mais avançadas em cenários de manutenção preditiva, como o [preditiva manutenção modelação guia R bloco de notas](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), várias origens de dados podem ser utilizadas. Outras origens de dados podem incluir registos históricos de manutenção, os registos de erros e funcionalidades de máquina e operador. Origens de dados adicionais poderão necessitar de diferentes tipos de treatments a ser utilizada em redes de aprendizagem profunda. Também é importante otimizar os modelos para os parâmetros corretos, como para o tamanho da janela. 

Pode editar as partes relevantes deste cenário e tente cenários de outro problema, tais como as descritas no [guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1), que envolve várias outras origens de dados.


## <a name="references"></a>Referências

- [Modelo de solução da manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
- [Bloco de notas do Python guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
- [Manutenção preditiva utilizando PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

