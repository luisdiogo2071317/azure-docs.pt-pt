---
title: "Learning avançada para cenários no mundo real de manutenção preditiva - Azure | Microsoft Docs"
description: "Saiba como replicar o tutorial num learning profunda de manutenção Preditiva com o Azure Machine Learning Workbench."
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: ireiter
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 7d4fe98b5c45767fb06391218e80789fc0c96a3b
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Ligação avançada de aprendizagem para o cenário do mundo real de manutenção preditiva.

Learning profunda é um das tendências mais populares no machine learning, com as aplicações para várias áreas, incluindo:
- carros driverless e robotics
- reconhecimento de voz e imagem
- a previsão financeiros.

Também conhecido como profunda neuronal redes (DNN), estes métodos são inspired pelos neurons individuais dentro de (biological as redes neurais) "-brain".

O impacto dos períodos de indisponibilidade do equipamento agendada pode ser detrimental para as empresas. É essencial manter a executar para maximizar o desempenho e de utilização e minimizar o período de indisponibilidade dispendioso, período de equipamento de campo. Antecipado identificação de problemas pode ajudar a alocar recursos de manutenção limitado de forma económica e melhorar a qualidade e fornecer os processos de cadeia. 

Uma estratégia de manutenção preditiva (horas) utiliza métodos do machine learning para determinar a condição do equipamento preventivamente efetuem a manutenção para evitar o desempenho da máquina adversos. PM, dados, recolhidos ao longo do tempo para monitorizar o estado da máquina, são estatísticas para localizar os padrões que podem ser utilizadas para prever falhas. [Muito pequeno termo memória (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) redes são apelativo para esta definição, desde que foram concebidos para saber de sequências de dados.

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub da Galeria

Segue-se a ligação para o repositório do GitHub público para os relatórios de problema e contribuições: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

## <a name="use-case-overview"></a>Descrição geral de cenário de utilização

Este tutorial utiliza o exemplo de eventos de executar a falha de motor de aeronave simulada para demonstrar a processo de modelação de manutenção preditiva. O cenário é descrito em [manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3)

O principal pressuposto nesta definição é o motor progressivamente degradado longo da respetiva duração. A degradação pode ser detetada em medidas de sensor do motor. PM tenta a relação entre as alterações nestes valores de sensor e as falhas de histórico de modelo. O modelo, em seguida, pode prever quando e de motor poderão falhar no futuro, com base no estado atual da medidas sensor.

Este cenário cria uma rede LSTM para prever a vida útil restantes (RUL) dos motores das aeronaves utilizando valores de histórico do sensor. Utilizar [Keras](https://keras.io/) com [Tensorflow](https://www.tensorflow.org/) profunda learning framework como um motor de computação, o cenário trains LSTM com um conjunto de motores e teste a rede de um conjunto de unseen motor.

## <a name="prerequisites"></a>Pré-requisitos
- Um [conta do Azure](https://azure.microsoft.com/free/) (gratuitas estão disponíveis).
- Azure Workbench do Machine Learning, com uma área de trabalho criada.
- Para operationalization do modelo: Operationalization de aprendizagem máquina do Azure, com um ambiente de implementação de local configurar e um [conta de gestão de modelo do Azure Machine Learning](model-management-overview.md).

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto ao utilizar este exemplo como um modelo:

1. Abra do Machine Learning Workbench.
2. No **projetos** página, selecione  **+** e, em seguida, selecione **novo projeto**.
3. No **criar novo projeto** painel, introduza as informações para o novo projeto.
4. No **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Manutenção preditiva" e selecione o **Learning avançada para o cenário de manutenção preditiva** modelo.
5. Clique em de **criar** botão

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de cálculo de servidor do bloco de notas

Para executar no seu computador local, a partir do Workbench AML `File` menu, selecione o `Open Command Prompt` ou `Open PowerShell CLI`. A interface CLI permite-lhe acedam aos serviços do Azure utilizando o `az` comandos. Primeiro, início de sessão à sua conta do Azure com o comando:

```
az login
``` 

Este comando fornece uma chave de autenticação a ser utilizado com o `https:\\aka.ms\devicelogin` URL. A CLI aguarda até que a operação de início de sessão de dispositivo devolve e fornece algumas informações de ligação. Em seguida, se tiver uma local [docker](https://www.docker.com/get-docker) instalar, preparar o ambiente de computação local com os seguintes comandos:

```
az ml experiment prepare --target docker --run-configuration docker
```

É preferível com um [Máquina Virtual de ciência de dados para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) de requisitos de memória e o disco. Depois do DSVM estiver configurado, prepare o ambiente de docker remoto com os seguintes dois comandos:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Assim que estiver ligado ao contentor do docker remoto, preparar o DSVM através de ambiente de computação do docker: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Com o docker computação ambiente preparado, abra o servidor de bloco de notas do Jupyter no separador de blocos de notas do AML Workbench ou iniciar um servidor baseado em browser com: 
```
az ml notebook start
```

As notas de exemplo são armazenadas no `Code` diretório. Os blocos de notas estão configurados para são executados sequencialmente, começando no primeiro (`Code\1_data_ingestion.ipynb`) bloco de notas. Quando abre cada bloco de notas, lhe for pedido para selecionar o kernel do computação. Escolha o `[Project_Name]_Template [Connection_Name]` kernel para executar o DSVM configurada anteriormente.

## <a name="data-description"></a>Descrição de dados

O modelo utiliza três conjuntos de dados como entradas, nos ficheiros **PM_train.txt**, **PM_test.txt**, e **PM_truth.txt**. 

-  **Preparar dados**: dados de falha de execução de motor da aeronave. Os dados de formação (PM_train.txt) é composta por vários séries de tempo multivariate, com *ciclo* como a unidade de tempo. Inclui as leituras dos sensores de 21 para cada ciclo. 

    - Cada série de tempo é gerado a partir de um motor de diferentes do mesmo tipo. Cada motor começa com diferentes de graus de atuais de desgaste inicial e algumas variação de fabrico exclusivo. Esta informação é desconhecida ao utilizador. 

    - O motor destes dados simulados, pressupõe-se a estar a funcionar normalmente no início de cada série de tempo. Começa como degradada em algum momento durante a série de ciclos de sistema operativos. A degradação avança e crescimentos de magnitude. 

    - Quando for atingido um limiar predefinido, o motor de é considerado não seguro para continuar a operação. O último ciclo de cada série de tempo é o ponto de falha desse motor.

-   **Testar dados**: A aeronave motor dados operacionais, sem eventos de falha registados. Os dados de teste (PM_test.txt) tem o mesmo esquema de dados como os dados de preparação. A única diferença é que os dados não indicam quando ocorre a falha (o período de tempo último *não* representam o ponto de falha). Não é conhecido como muitas ciclos mais este motor de pode última antes de falhar.

- **Dados truth**: as informações de restantes verdadeiro ciclos de cada motor nos dados de teste. Os dados de truth zero fornece o número de ciclos de trabalho restantes para os motores de dados de teste.

## <a name="scenario-structure"></a>Estrutura do cenário

O fluxo de trabalho de cenário é dividido em três passos, cada execução blocos de notas do Jupyter. Cada bloco de notas produz artefactos de dados que são mantidos localmente para utilização em blocos de notas seguintes: 

### <a name="task-1-data-ingestion-and-preparation"></a>Tarefa 1: Ingestão de dados e a preparação

Notas do Jupyter de ingestão de dados no `Code/1_data_ingestion_and_preparation.ipnyb` carrega os três conjuntos de dados de entrada para o formato do pacote de dados de Pandas. Em seguida, prepara os dados para a modelação e efetua algumas visualização de dados preliminar. Os conjuntos de dados são armazenados local para o contexto de computação para utilização num bloco de notas da criação do modelo.

### <a name="task-2-model-building-and-evaluation"></a>Tarefa 2: Modelo edifício e avaliação

O modelo de criação de notas do Jupyter no `Code/2_model_building_and_evaluation.ipnyb` lê os conjuntos de dados de formação e teste a partir do disco e cria uma rede LSTM o conjunto de dados de formação. O desempenho do modelo é medido no conjunto de teste. O modelo resultante é serializado e armazenado no contexto de computação local para utilização na tarefa operationalization.

### <a name="task-3-operationalization"></a>Tarefa 3: Operationalization

O operationalization bloco de notas do Jupyter no `Code/3_operationalization.ipnyb` utiliza o modelo armazenado a criação de funções e o esquema para chamar o modelo de um serviço web alojado no Azure. O bloco de notas testa as funções e, em seguida, comprime os recursos para o `LSTM_o16n.zip` ficheiro, que é carregado para o contentor de armazenamento do Azure para a implementação.

O `LSTM_o16n.zip` ficheiro de implementação contém os artefactos seguintes:

- `webservices_conda.yaml` Define os pacotes de python necessários para executar o modelo LSTM no destino da implementação.  
- `service_schema.json` Define o esquema de dados esperado pelo modelo de LSTM.     
- `lstmscore.py` Define as funções que do destino da implementação está a executar para pontuar novos dados.    
- `modellstm.json` Define a arquitetura LSTM. As funções de lstmscore.py leem da arquitetura e as ponderações para inicializar o modelo.
- `modellstm.h5` Define as ponderações de modelo.
- `test_service.py` Um script de teste que chama o ponto final de implementação com o teste de registos de dados. 
- `PM_test_files.pkl` O `test_service.py` script lê dados de histórico do motor do `PM_test_files.pkl` de ficheiros e envia o webservice suficiente ciclos de LSTM devolver uma probabilidade de falha do motor.

O bloco de notas testa as funções com a definição de modelo antes de que os ativos de operationalization para a implementação de pacotes. Instruções para configurar e testar o serviço Web estão incluídas no final de `Code/3_operationalization.ipnyb` bloco de notas.

## <a name="conclusion"></a>Conclusão

Este tutorial utiliza um cenário simples com valores do sensor para tornar as predições. Mais avançadas em cenários de manutenção preditiva, como o [preditiva manutenção modelação guia R bloco de notas](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1), pode utilizar várias origens de dados, tais como registos históricos de manutenção, os registos de erros e as funcionalidades do computador. Origens de dados adicionais podem exigir treatments diferentes para ser utilizado com learning profunda.


## <a name="references"></a>Referências

Outros exemplos de cenários de utilização de manutenção preditiva estão disponíveis numa variedade de plataformas:

* [Modelo de solução da manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Preditiva manutenção modelação Guia utilizando os serviços do SQL Server R](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Bloco de notas do Python guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Manutenção preditiva utilizando PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

* [Cenário do mundo real de manutenção preventiva](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-predictive-maintenance)

## <a name="next-steps"></a>Passos Seguintes

Existem muitos cenários de exemplo disponíveis do Workbench do Azure Machine Learning que demonstram as funcionalidades adicionais do produto. 