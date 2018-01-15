---
title: "Cenário do mundo Real de manutenção preditiva | Microsoft Docs"
description: "Preditiva manutenção Real cenário do mundo utilizando PySpark"
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
ms.openlocfilehash: 0299e73aecca3b3e5714b37c8b0b776ec8561e29
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="predictive-maintenance-real-world-scenario"></a>Cenário do mundo real de manutenção preditiva.

O impacto dos períodos de indisponibilidade do equipamento agendada pode ser detrimental para as empresas. É fundamental para, por conseguinte, o equipamento de campo manter em execução para maximizar o desempenho e de utilização e pela minimizar o período de indisponibilidade dispendioso, agendado. Antecipado identificação de problemas pode ajudar a alocar recursos de manutenção limitado de forma económica e melhorar a qualidade e fornecer os processos de cadeia. 

Este cenário explicar um relativamente [conjunto de dados em grande escala simulado](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) a guiá-lo através de um projeto de ciência de dados de manutenção preditiva da ingestão de dados, funcionalidade operationalization de modelo, de criação de modelo e de engenharia e implementação. O código para todo o processo é escrito em blocos de notas do Jupyter com PySpark Workbench do Azure ML. O modelo final é implementado utilizando a gestão de modelo do Azure Machine Learning para fazer predições de falha de equipamento em tempo real.   

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub da Galeria

Segue-se a ligação para o repositório do GitHub pública: [https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)


## <a name="use-case-overview"></a>Descrição geral de cenário de utilização

Um problema principais deparam por empresas na asset pesado indústrias é os custos significativos que estão associados a atrasos mechanical problemas. A maioria das empresas estão interessadas nas prever quando estes problemas surgem para proativamente impedi-los antes de ocorrem. O objetivo consiste em reduzir os custos ao reduzir o período de indisponibilidade e, possivelmente, aumentar a segurança. 

Este cenário demora ideias do [manual de comunicação social de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) para demonstrar a criação de um modelo preditivo para um conjunto de dados simulado. Os dados de exemplo são derivados de elementos comuns observados em muitos casos de utilização de manutenção preditiva.

É o problema de negócio para estes dados simulados prever problemas causado por falhas do componente. A pergunta de negócio, por conseguinte, é "*o que é a probabilidade de uma máquina fica inativo devido uma falha de um componente*?" Este problema é formatado como um problema de classificação de várias classes (vários componentes por máquina) e um algoritmo do machine learning é utilizado para criar o modelo preditivo. O modelo está preparado no histórico dados recolhidos a partir de computadores. Neste cenário, o utilizador passa através de vários passos para implementar a modelo de ambiente do Workbench do Azure Machine Learning.

## <a name="prerequisites"></a>Pré-requisitos

* Um [conta do Azure](https://azure.microsoft.com/en-us/free/) (gratuitas estão disponíveis).
* Uma cópia instalada do [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) seguintes o [guia de instalação de início rápido](./quickstart-installation.md) para instalar o programa e criar uma área de trabalho.
* Azure Operationalization de aprendizagem máquina requer um ambiente de implementação de local e um [conta de gestão de modelo](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)

Neste exemplo, pode ser executado em qualquer contexto de computação AML Workbench. No entanto, recomenda-se a executá-lo com, pelo menos, de 16 GB de memória. Este cenário foi criado e testado num computador Windows 10 com um padrão de DS4_V2 remoto [Máquina Virtual de ciência de dados para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Modelo operationalization foi feito utilizando 0.1.0a22 de versão da CLI do Azure ML.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abra máquina do Azure Learning Workbench
2.  No **projetos** página, clique em de  **+**  iniciar sessão e selecionar **novo projeto**
3.  No **criar novo projeto** painel, preencha as informações para o novo projeto
4.  No **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Manutenção preditiva" e selecione o modelo
5.  Clique em **Criar**.

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

O [simulated dados](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) é composta por cinco ficheiros de valores separados por vírgulas (. csv). Siga as ligações para obter mais uma descrição mais detalhada dos conjuntos de dados.

* [Máquinas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): funcionalidades differentiating cada máquina. Por exemplo, a idade e o modelo.
* [Erro](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): O registo de erros contém erros de sem quebra acionados enquanto a máquina ainda está operacional. Estes erros não são considerados como falhas, mas poderá estar preditivos de um evento de falha de futuras. A data / hora do erro são arredondados a hora mais próxima, uma vez que os dados de telemetria são recolhidos a uma taxa por hora.
* [Manutenção](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): O registo de manutenção contém ambos os registos de agendados e manutenção. Manutenção agendada corresponde com regular inspeção dos componentes, manutenção agendada pode surgir na falha mechanical ou outra degradação do desempenho. A data / hora de manutenção são arredondados a hora mais próxima, uma vez que os dados de telemetria são recolhidos a uma taxa por hora.
* [Telemetria](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): mede a telemetria consiste em dados de série de tempo de sensores vários dentro de cada máquina. Os dados são registados por valores do sensor uma média ao longo de cada intervalo de uma hora.
* [Falhas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): falhas correspondem a substituições de componente no registo de manutenção. Cada registo contém o ID de máquina, a tipo de componente e a data de substituição e a hora. Estes registos são utilizados para criar a aprendizagem automática etiquetas que o modelo está a tentar prever.

Consulte o [ingestão de dados](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) cenário de notas do Jupyter na secção de código para transferir os conjuntos de dados não processados a partir do repositório do GitHub e criar os conjuntos de dados para esta análise PySpark.

## <a name="scenario-structure"></a>Estrutura do cenário
O conteúdo para o cenário está disponível na [repositório do GitHub](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

O [Leia-me](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) ficheiro descreve o fluxo de trabalho de preparar os dados, criar um modelo e, em seguida, implementar uma solução para produção. Cada passo do fluxo de trabalho é encapsulado num bloco de notas do Jupyter no [código](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) pasta dentro do repositório.   

[`Code\1_data_ingestion.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): Este bloco de notas transfere os cinco ficheiros de entrada. csv, efetua algumas preliminar limpeza e visualização. O bloco de notas converte cada conjunto de dados no formato de PySpark e armazena-a para um contentor de Blobs do Azure para utilização num bloco de notas engenharia da funcionalidade.

[`Code\2_feature_engineering.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): O conjunto de dados não processado a partir do blob do Azure, as funcionalidades são construídas a utilizar a abordagem de séries de tempo de padrão para os dados de telemetria, erros e manutenção de modelo a utilizar. As substituições relacionadas com a falha de componente são utilizadas para construir as etiquetas de modelo que descrevem o componente de sincronização falhou. Os funcionalidade identificados os dados são guardados num blob do Azure para o modelo de criação de bloco de notas.

[`Code\3_model_building.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): O conjunto de dados de funcionalidade com nome, a utilizar o bloco de notas modelação divide os dados em conjuntos de dados de formação e dev juntamente o carimbo de data / hora. O bloco de notas é a experiência de conjunto de configuração com `pyspark.ml.classification` modelos. Os dados de preparação é vectorized e o utilizador pode experimentar o um `DecisionTreeClassifier` ou um `RandomForestClassifier`, manipulação de sintonização para determinar o melhor desempenho modelo. O desempenho é determinado através da avaliação de estatísticas de medida no conjunto de dados do programador. Estas estatísticas são registadas no ecrã de tempo Workbench AML executar para o controlo. Em cada execução, o bloco de notas guarda o modelo resultante para o disco local com o kernel do bloco de notas Jupyter. 

[`Code\4_operationalization.ipynb`](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): Utilizando o modelo de último guardado para disco (kernel de bloco de notas do Jupyter) local, este bloco de notas baseia-se os componentes para implementar o modelo para um serviço web do Azure. Os ativos completa operacionais são compacted para o `o16n.zip` ficheiros armazenados no outro contentor de blob do Azure. O ficheiro zipped contém:

* `service_schema.json`O ficheiro de definição de esquema para a implementação. 
* `pdmscore.py`As funções de init() e run() requeridas pelo serviço web do Azure
* `pdmrfull.model`O diretório de definição do modelo.
    
 O bloco de notas testa as funções com a definição de modelo antes de empacotar os ativos de operationalization para implementação. Instruções de implementação são incluídas no fim do bloco de notas.

## <a name="conclusion"></a>Conclusão

Este cenário fornece o leitor de uma descrição geral de como construir uma solução de manutenção preditiva ponto a ponto utilizando PySpark dentro do ambiente de bloco de notas do Jupyter no Azure ML Workbench. Neste cenário de exemplo também detalhes de implementação do modelo através do ambiente de gestão de modelo do Azure Machine Learning para fazer predições de falha de equipamento de em tempo real.

## <a name="references"></a>Referências

Neste caso, utilize foi anteriormente desenvolveu em várias plataformas:

* [Modelo de solução da manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Preditiva manutenção modelação Guia utilizando os serviços do SQL Server R](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [Bloco de notas do Python guia de modelação de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Manutenção preditiva utilizando PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)

## <a name="next-steps"></a>Passos Seguintes

Existem muitos cenários de exemplo disponíveis do Workbench do Azure Machine Learning que demonstram as funcionalidades adicionais do produto. 