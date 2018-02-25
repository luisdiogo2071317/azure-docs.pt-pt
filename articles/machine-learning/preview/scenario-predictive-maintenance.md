---
title: "Para cenários no mundo real de manutenção preditiva | Microsoft Docs"
description: "Manutenção preditiva para cenários no mundo real, utilizando PySpark"
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: 81e227194ff64d7b7af842a208349ccc63528ab8
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Manutenção preditiva para cenários no mundo real

O impacto dos períodos de indisponibilidade do equipamento agendada pode ser detrimental para as empresas. É essencial manter em execução para maximizar a utilização e desempenho e para minimizar o período de indisponibilidade dispendioso, período de equipamento de campo. Antecipado identificação de problemas pode ajudar a alocar recursos de manutenção limitado de forma económica e melhorar a qualidade e fornecer os processos de cadeia. 

Este cenário explicar um relativamente [conjunto de dados em grande escala simulado](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) a guiá-lo através de um projeto de ciência de dados de manutenção preditiva da ingestão de dados, funcionalidade operationalization de modelo, de criação de modelo e de engenharia e implementação. O código para todo o processo é escrito no bloco de notas do Jupyter utilizando PySpark no Azure Machine Learning Workbench. O modelo final é implementado utilizando a gestão de modelo do Azure Machine Learning para fazer predições de falha de equipamento em tempo real.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Repositório do GitHub de galeria do Cortana Intelligence

Galeria da Cortana Intelligence para o tutorial PM é um repositório de GitHub público ([https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance)) onde pode comunicar problemas e certifique- contribuições.


## <a name="use-case-overview"></a>Descrição geral de cenário de utilização

Um problema principais que enfrenta de empresas no recurso pesado indústrias é os custos significativos que estão associados a atrasos devido a problemas de mechanical. A maioria das empresas estão interessadas nas prever quando estes problemas que possam surgir para proativamente impedi-los antes de ocorrem. O objetivo consiste em reduzir os custos ao reduzir o período de indisponibilidade e, possivelmente, aumentar a segurança. 

Este cenário demora ideias do [manual de comunicação social de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) para demonstrar como criar um modelo preditivo para um conjunto de dados simulado. Os dados de exemplo são derivados de elementos comuns que são observados em muitos casos de utilização de manutenção preditiva.

É o problema de negócio para estes dados simulados prever problemas que são causados por falhas do componente. A pergunta de negócio é "*o que é a probabilidade de uma máquina fica inativo devido uma falha de um componente?*" Este problema é formatado como um problema de classificação de classe multi (vários componentes por máquina). Um algoritmo do machine learning é utilizado para criar o modelo preditivo. O modelo está preparado nos dados históricos que são recolhidos a partir de computadores. Neste cenário, o utilizador passa através de vários passos para implementar o modelo no ambiente do Workbench do Machine Learning.

## <a name="prerequisites"></a>Pré-requisitos

* Um [conta do Azure](https://azure.microsoft.com/free/) (gratuitas estão disponíveis).
* Uma cópia instalada do [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md). Siga o [guia de instalação de início rápido](./quickstart-installation.md) para instalar o programa e criar uma área de trabalho.
* Azure Operationalization de aprendizagem máquina requer um ambiente de implementação de local e um [conta de gestão de modelo do Azure Machine Learning](model-management-overview.md).

Neste exemplo é executado em qualquer contexto de computação do Workbench do Machine Learning. No entanto, é recomendado que execute o exemplo com, pelo menos, 16 GB de memória. Este cenário foi criado e testado num computador Windows 10 com um padrão de DS4_V2 remoto [Máquina Virtual de ciência de dados (DSVM) para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Modelo operationalization foi feito ao utilizar a versão 0.1.0a22 da CLI do Azure Machine Learning.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto ao utilizar este exemplo como um modelo:
1.  Abra do Machine Learning Workbench.
2.  No **projetos** página, selecione  **+** e, em seguida, selecione **novo projeto**.
3.  No **criar novo projeto** painel, preencha as informações para o novo projeto.
4.  No **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Manutenção preditiva" e selecione o **manutenção preditiva** modelo.
5.  Selecione **Criar**.

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de cálculo de servidor do bloco de notas

Para executar no seu computador local, do Workbench do Machine Learning **ficheiro** menu, selecione **abra a linha de comandos** ou **abra CLI do PowerShell**. A interface CLI permite-lhe acedam aos serviços do Azure utilizando o `az` comandos. Primeiro, inicie sessão sua conta do Azure com o comando:

```
az login
``` 

Este comando fornece uma chave de autenticação a utilizar com a https:\\aka.ms\devicelogin URL. A CLI aguarda até que a operação de início de sessão de dispositivo devolve e fornece algumas informações de ligação. Em seguida, se tiver uma local [Docker](https://www.docker.com/get-docker) instalação, preparar o ambiente de computação local com o comando:

```
az ml experiment prepare --target docker --run-configuration docker
```

É preferível com um [DSVM para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) de requisitos de memória e o disco. Depois do DSVM estiver configurado, prepare o ambiente de Docker remoto com os seguintes dois comandos:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Depois de se estiver ligado ao contentor do Docker remoto, prepare o ambiente de computação de DSVM Docker com o comando: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Com o ambiente de computação de Docker preparado, abra o servidor de bloco de notas do Jupyter do Workbench do Azure Machine Learning **blocos de notas** separador ou um servidor baseado em browser com o comando: 

```
az ml notebook start
```

As notas de exemplo são armazenadas no diretório do código. Os blocos de notas são configurados para são executados sequencialmente, a partir do bloco de notas (Code\1_data_ingestion.ipynb) primeiro. Quando abre cada bloco de notas, é-lhe pedido que selecione kernel computação. Escolha o kernel de modelos [Connection_Name] de [Project_Name] para executar o DSVM configurada anteriormente.

## <a name="data-description"></a>Descrição de dados

O [simulated dados](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) é composta por cinco ficheiros de valores separados por vírgulas (. csv). Utilize as hiperligações seguintes para obter descrições detalhadas sobre os conjuntos de dados.

* [Máquinas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): funcionalidades diferenciarem cada máquina, como a idade e o modelo.
* [Erro](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): O registo de erros contém erros de sem quebra que são acionados enquanto a máquina ainda está operacional. Estes erros não são considerados falhas, embora possam ser preditivos de um evento de falha de futuras. Os valores de data / hora para os erros são arredondados a hora mais próxima, uma vez que os dados de telemetria são recolhidos a uma taxa por hora.
* [Manutenção](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): O registo de manutenção contém ambos os registos de agendados e manutenção. Manutenção agendada corresponde com a inspeção regular dos componentes. Manutenção agendada pode surgir da falha mechanical ou outra degradação do desempenho. Os valores de data / hora de manutenção são arredondados a hora mais próxima, uma vez que os dados de telemetria são recolhidos a uma taxa por hora.
* [Telemetria](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): os dados de telemetria é composta por uma medidas de séries de tempo de sensores vários dentro de cada máquina. Os dados são registados por valores do sensor uma média ao longo de cada intervalo de uma hora.
* [Falhas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): falhas correspondem a substituições de componente no registo de manutenção. Cada registo contém o ID de máquina, a tipo de componente e a data de substituição e a hora. Estes registos são utilizados para criar a aprendizagem automática etiquetas que o modelo está a tentar prever.

Para transferir os conjuntos de dados não processados a partir do repositório do GitHub e criar os conjuntos de dados para esta análise PySpark, consulte o [ingestão de dados](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) cenário de notas do Jupyter na pasta de código.

## <a name="scenario-structure"></a>Estrutura do cenário
O conteúdo para o cenário está disponível na [repositório do GitHub](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

O [Leia-me](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) ficheiro descreve o fluxo de trabalho de preparar os dados, criar um modelo e, em seguida, implementar uma solução para produção. Cada passo do fluxo de trabalho é encapsulado num bloco de notas do Jupyter no [código](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) pasta dentro do repositório.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): este bloco de notas transfere automaticamente os cinco ficheiros. csv de entrada e efetua algumas preliminar limpeza e visualização. O bloco de notas converte cada conjunto de dados no formato de PySpark e armazena-os num contentor de Blobs do Azure para utilização num bloco de notas engenharia da funcionalidade.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): as funcionalidades de modelo são construídas a partir do conjunto de dados não processado do Blob storage do Azure, utilizando uma abordagem de séries de tempo de padrão para dados de telemetria, erros e manutenção. As substituições relacionadas com a falha de componente são utilizadas para construir as etiquetas de modelo que descrevem o componente de sincronização falhou. Os funcionalidade identificados os dados são guardados num blob do Azure para o bloco de notas de criação de modelo.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): O bloco de notas de criação de modelo utiliza o conjunto de dados de funcionalidade identificados e divide os dados em conjuntos de dados de formação e dev juntamente o carimbo de data / hora. O bloco de notas é configurado como uma experiência com modelos de pyspark.ml.classification. Os dados de preparação é vectorized. O utilizador pode experimentar o um **DecisionTreeClassifier** ou **RandomForestClassifier** manipular para determinar o melhor desempenho modelo de sintonização. O desempenho é determinado através da avaliação de estatísticas de medida no conjunto de dados do programador. Estas estatísticas são registadas no controlo no ecrã de tempo de execução de Machine Learning Workbench. Em cada execução, o bloco de notas guarda o modelo resultante para o disco local que está a executar o kernel do bloco de notas Jupyter. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): este bloco de notas utiliza o modelo de último que é guardado no disco local (kernel de bloco de notas do Jupyter) para criar os componentes para implementar o modelo para um serviço web do Azure. Os ativos completa operacionais são compacted no ficheiro o16n.zip que é armazenado no outro contentor de blob do Azure. O ficheiro zipped contém:

* **service_schema.JSON**: O ficheiro de definição de esquema para a implementação. 
* **pdmscore.PY**: O **init()** e **run()** serviço web de funções que são necessários para o Azure.
* **pdmrfull.Model**: O diretório de definição do modelo.
    
O bloco de notas testa as funções com a definição de modelo antes de empacotar os ativos de operationalization para implementação. Instruções de implementação são incluídas no fim do bloco de notas.

## <a name="conclusion"></a>Conclusão

Este cenário proporcione uma descrição geral da criação de uma solução de manutenção preditiva ponto-a-ponto utilizando PySpark no ambiente de notas do Jupyter no Machine Learning Workbench. Neste cenário de exemplo também detalhes de implementação do modelo através do ambiente de gestão de modelo do Machine Learning para fazer predições de falha de equipamento em tempo real.

## <a name="references"></a>Referências

As referências seguintes fornecem exemplos de outra manutenção preditiva casos de utilização para várias plataformas:

* [Modelo de solução da manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Preditiva manutenção modelação Guia utilizando os serviços do SQL Server R](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Bloco de notas do Python guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Manutenção preditiva utilizando PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Ligação avançada de aprendizagem para manutenção preventiva](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Passos Seguintes

Outros cenários de exemplo estão disponíveis no Machine Learning Workbench que demonstram as funcionalidades adicionais do produto. 
