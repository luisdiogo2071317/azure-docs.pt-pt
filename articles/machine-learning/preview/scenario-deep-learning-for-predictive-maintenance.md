---
title: "Ligação avançada de cenário do mundo Real de manutenção preditiva - Azure aprendizagem | Microsoft Docs"
description: "Este documento descreve como replicar a aprendizagem profunda tutorial de manutenção Preditiva com o Azure Machine Learning Workbench"
services: machine-learning
author: FrancescaLazzeri
ms.author: Lazzeri
manager: ireiter
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 11/22/2017
ms.openlocfilehash: 8997e38a81ed848c9e052ab08566ffc99560ed86
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="deep-learning-for-predictive-maintenance-real-world-scenario"></a>Ligação avançada de aprendizagem para o cenário do mundo real de manutenção preditiva

Learning profunda é um das tendências mais populares na aprendizagem automática espaço nowadays e existem muitas campos e aplicações em que se representa, como carros driverless, reconhecimento de voz e imagem, robotics e financeiro. Learning profunda é um conjunto de algoritmos que é inspired pela forma da nossa "-brain" (biological as redes neurais) e machine learning e cientistas cognitivos normalmente fazem referência ao mesmo como Artificial neuronal redes (ANN).

Manutenção preditiva também é uma área muito popular em que várias técnicas diferentes foram concebidas para ajudar a determinar a condição do equipamento para prever quando deve ser efetuada a manutenção. Manutenção preditiva abrange uma variedade de tópicos, incluindo mas não se limitando: predição de falha, diagnóstico de falha (análise da causa raiz), a deteção de falha, classificação de tipo de falha e recomendação de ações de mitigação ou manutenção após Falha.

Em cenários de manutenção preditiva, os dados são recolhidos ao longo do tempo para monitorizar o estado do equipamento com o objetivo final de descobrir padrões para prever falhas. Entre avançada learning métodos, [muito curto prazo memória (LSTM)](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) redes são especialmente apelativos para resolver o domínio de manutenção preditiva que são muito eficiente a aprendizagem da sequências. Este facto presta-se para as aplicações com dados de séries de tempo, tornando a possível novamente procurar por períodos mais longos de tempo para detetar padrões de falha.

Neste tutorial, iremos criar uma rede LSTM para o conjunto de dados e o cenário descrito em [manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3) para prever a vida útil de motores das aeronaves de restantes. Em resumo, o modelo utiliza valores de sensores simulados aeronave para prever quando um motor de aeronave falhará no futuro, para que a manutenção pode ser planeada com antecedência.

Este tutorial utiliza [keras](https://keras.io/) learning profunda biblioteca com o Toolkit de cognitivos [CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/Using-CNTK-with-Keras) como back-end.

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub da Galeria 

Segue-se a ligação para o repositório do GitHub pública: [https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)

## <a name="use-case-overview"></a>Descrição geral de cenário de utilização

Este tutorial utiliza o exemplo de eventos de executar a falha de motor de aeronave simulada para demonstrar a processo de modelação de manutenção preditiva. Pressuposto implícito de dados de modelação como feito abaixo é que o elemento de interesse tem um padrão de degradação suspenso, o que é refletido na medidas de sensor o elemento. Ao examinar os valores de sensor o elemento ao longo do tempo, o algoritmo do machine learning pode obter a relação entre os valores de sensor e alterações nos valores do sensor para as falhas de histórico para prever falhas no futuro. Sugerimos examinando o formato dos dados e passar todos os três passos do modelo antes de substituir os dados com os seus próprios.

## <a name="prerequisites"></a>Pré-requisitos

- Um [conta do Azure](https://azure.microsoft.com/free/) (gratuitas estão disponíveis).
- Uma cópia instalada do Workbench do Azure Machine Learning com uma área de trabalho criada.
- Para operationalization do modelo: Operationalization de aprendizagem máquina do Azure com uma configuração de ambiente de implementação de local e um [conta de gestão de modelo](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview)

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do workbench

Crie um novo projeto com este exemplo como um modelo:

1. Abra máquina do Azure Learning Workbench
2. Na página projetos, clique o + iniciar sessão e selecione o novo projeto
3. No painel de criar um novo projeto, preencha as informações para o novo projeto
4. Na caixa de pesquisa de modelos de projeto de pesquisa, escreva "Manutenção preditiva" e selecione o modelo
5. Clique em Criar

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de cálculo de servidor do bloco de notas

Para executar no seu computador local, a partir do Workbench AML `File` menu, selecione o `Open Command Prompt` ou `Open PowerShell` CLI. Dentro da CLI do windows, execute os seguintes comandos:

`az ml experiment prepare --target docker --run-configuration docker`

 Sugerimos a executar um padrão de DS4_V2 [Máquina Virtual de ciência de dados para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu). Depois de ter configurado o DSVM, terá de executar os seguintes dois comandos:

`az ml computetarget attach remotedocker --name [Desired_Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]`

`az ml experiment prepare --target [Desired_Connection_Name] --run-configuration [Desired_Connection_Name]`

Com as imagens de docker _preparado_, abra o servidor de bloco de notas do jupyter dentro de *AML Workbench* blocos de notas do separador ou para iniciar um servidor baseado em browser, execute:`az ml notebook start`

- Blocos de notas são armazenados no `Code` localizar diretório no ambiente do Jupyter. Iremos executar estes blocos de notas sequencialmente como numerado, começando no (`Code\1_data_ingestionand_and_preparation.ipynb`).

- Selecione o kernel para corresponder ao seu [Project_Name] modelos [Desired_Connection_Name] e clique em definir Kernel

## <a name="data-description"></a>Descrição de dados

O modelo utiliza três conjuntos de dados como entradas: "PM_train.txt", "PM_test.txt" e "PM_truth.txt"
1. Dados de formação: é os dados de execução-falha do motor de aeronave. Os dados de formação ("PM_train.txt") é composta por várias séries de tempo multivariate com "ciclo" como a unidade de tempo, em conjunto com as leituras dos sensores de 21 para cada ciclo. Cada série de tempo pode assumir-se como a ser gerados a partir de um motor de diferentes do mesmo tipo. Pressupõe-se que cada motor para começar a utilizar graus diferentes de atuais de desgaste inicial e a variação de fabrico e esta informação é desconhecida ao utilizador. O motor destes dados simulados, pressupõe-se a estar a funcionar normalmente no início de cada série de tempo. Começa como degradada em algum momento durante a série de ciclos de sistema operativos. A degradação avança e crescimentos de magnitude. Quando for atingido um limiar predefinido, o motor é considerado não seguro para continuar a operação. Por outras palavras, o último ciclo de cada série de tempo pode ser considerado como o ponto de falha do motor correspondente.

2. Dados de teste: é o motor de aeronave operativo dados sem falhas eventos registados. Os dados de teste ("PM_test.txt") tem o mesmo esquema de dados como os dados de preparação. A única diferença é que os dados não indicam quando ocorre a falha (por outras palavras, o último período de tempo não representam o ponto de falha). Não é conhecido como muitas ciclos mais este motor de pode última antes de falhar.

3. Dados truth: contém as informações de verdadeiros ciclos restantes para cada motor nos dados de teste. Os dados de truth zero fornece o número de ciclos de trabalho restantes para os motores de dados de teste.

## <a name="scenario-structure"></a>Estrutura do cenário

O conteúdo para o cenário está disponível no [repositório GitHub] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) 

No repositório, é um ficheiro de [Leia-me] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/blob/master/README.md), que descreve os processos de preparar os dados até que a criação e operacionalizar o modelo. Os blocos de notas do Jupyter três estão disponíveis na pasta [código] (https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance/tree/master/Code) dentro do repositório. 

Em seguida Vamos descrever o fluxo de trabalho do cenário passo a passo:

### <a name="task-1-data-ingestion--preparation"></a>Tarefa 1: Ingestão de dados e a preparação

Notas do Jupyter de ingestão de dados no `Code/1_data_ingestion_and_preparation.ipnyb` cargas de entrada de três conjuntos de dados para `Pandas` dataframe formato, prepara os dados para a parte de modelação e efetua algumas visualização de dados preliminar. Os dados, em seguida, são transformados `PySpark` formatar e armazenados num contentor de armazenamento de Blobs do Azure na sua subscrição para utilização na próxima tarefa de modelação.

### <a name="task-2-model-building--evaluation"></a>Tarefa 2: Criar modelo & avaliação

O modelo de criação de notas do Jupyter no `Code/2_model_building_and_evaluation.ipnyb` que lê `PySpark` dar formação e testar conjuntos de dados do armazenamento de Blobs. Em seguida, uma rede LSTM baseia-se com os conjuntos de dados de formação. O desempenho do modelo é medido no conjunto de teste. O modelo resultante é serializado e armazenado no contexto de computação local para utilização na tarefa operationalization.

### <a name="task-3-operationalization"></a>Tarefa 3: Operationalization

O operationalization bloco de notas do Jupyter no `Code/3_operationalization.ipnyb` que é necessário o modelo armazenado e baseiam-se as funções e o esquema para chamar o modelo num Azure alojados serviço web. O bloco de notas testa as funções e zips os ativos de operationalization para um ficheiro zip que também é armazenado no contentor de armazenamento de Blobs do Azure.
O ficheiro zipped contém:

- `service_schema.json`O ficheiro de definição de esquema para a implementação. 
- `lstmscore.py`As funções de init() e run() requeridas pelo serviço web do Azure
- `lstmfull.model`O diretório de definição do modelo.

O bloco de notas testa as funções com a definição de modelo antes de empacotar os ativos de operationalization para implementação. Instruções de implementação são incluídas no fim do bloco de notas.


## <a name="conclusion"></a>Conclusão

Este tutorial funciona como um guia para principiantes, procura para aplicar learning profunda no domínio de manutenção preditiva dentro do ambiente de bloco de notas do Jupyter no *Azure Machine Learning Workbench*. Este tutorial utiliza um cenário simples em que apenas uma origem de dados (valores sensor) é utilizada para tornar as predições. Em mais avançadas cenários de manutenção preditiva como em [guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-R-Notebook-1), existem muitas outras origens de dados (registos históricos ou seja, de manutenção, os registos de erros, as funcionalidades de máquina e operador etc.) que Pode necessitar de diferentes tipos de treatments a ser utilizado na avançada redes de aprendizagem. Uma vez que a manutenção preditiva não é um domínio típico de aprendizagem profunda, a aplicação é uma área aberta de investigação.

## <a name="references"></a>Referências

- [Modelo de solução da manutenção preditiva](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance)
- [Guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1)
- [Bloco de notas do Python guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-modeling-Guide-Python-Notebook-1)
- [Manutenção preditiva utilizando PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="future-directions-and-improvements"></a>As direções futuras e melhorias

Este tutorial abrange as noções básicas da utilização de aprendizagem profunda em manutenção preditiva; Muitos problemas de manutenção preditiva normalmente envolvem uma variedade de origens de dados tem de ser levados em consideração ao aplicar learning profunda neste domínio. Além disso, é importante otimizar os modelos para os parâmetros corretos, tais como o tamanho da janela. O leitor pode editar as partes relevantes deste cenário e tente um cenário de problema diferentes, tais como as descritas no [guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-modeling-Guide-1) onde estiverem envolvidas vários outras origens de dados.
