---
title: Manutenção preditiva para cenários reais | Documentos da Microsoft
description: Manutenção preditiva para cenários do mundo real usando PySpark
services: machine-learning
author: ehrlinger
ms.author: jehrling
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.custom: mvc
ms.date: 10/05/2017
ms.openlocfilehash: a5531ae256a263f1c34496819ac435ce67156b49
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649336"
---
# <a name="predictive-maintenance-for-real-world-scenarios"></a>Manutenção preditiva para cenários do mundo real

O impacto do período de indisponibilidade não agendado do equipamento pode ser prejudicial para qualquer negócio. É essencial para manter os equipamentos de campo em execução para maximizar o desempenho e a utilização e para minimizar o período de indisponibilidade dispendioso, não agendado. A identificação precoce de problemas pode ajudar a alocar recursos limitados de manutenção de uma forma rentável, melhorar a qualidade e fornecer os processos de cadeia. 

Este cenário explora uma relativamente [conjunto de dados em grande escala simulado](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) para percorrer um projeto de ciência de dados de manutenção preditiva de ingestão de dados, apresentam engenharia, a criação de modelo e a operacionalização de modelo e implementação. O código para todo o processo é escrito no bloco de notas do Jupyter com PySpark no Azure Machine Learning Workbench. O modelo final é implementado utilizando a gestão de modelos do Azure Machine Learning para fazer predições de falhas de equipamento em tempo real.   

### <a name="cortana-intelligence-gallery-github-repository"></a>Repositório do GitHub de galeria do Cortana Intelligence

Galeria da Cortana Intelligence para o tutorial de PM é um repositório do GitHub público ([https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance)) onde pode comunicar problemas e fazer contribuições.


## <a name="use-case-overview"></a>Descrição geral de caso de utilização

Um grande problema é enfrentado por empresas em setores de recurso pesado é aos custos significativos que estão associados a atrasos devido a problemas de mecânicos. A maioria das empresas estão interessadas em prever quando esses problemas podem surgir para impedi-los forma proativa antes que ocorram. O objetivo é reduzir os custos ao reduzir o tempo de inatividade e, possivelmente, aumentar a segurança. 

Este cenário assume ideias a partir do [playbook de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance) para demonstrar como criar um modelo preditivo para um conjunto de dados simulado. Os dados de exemplo são derivados de elementos comuns que são observados em muitos casos de utilização de manutenção preditiva.

O problema de negócio para estes dados simulados é prever problemas causados por falhas de componentes. A pergunta de negócios é "*o que é a probabilidade de que uma máquina fica inativa devido uma falha de um componente?*" Esse problema é formatado como um problema de classificação de Roc (vários componentes por máquina). Um algoritmo de machine learning é utilizado para criar o modelo de previsão. É preparado o modelo nos dados históricos, que são recolhidos a partir de máquinas. Neste cenário, o usuário executa vários passos para implementar o modelo no ambiente de Machine Learning Workbench.

## <a name="prerequisites"></a>Pré-requisitos

* Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
* Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Siga os [guia de início rápido de instalação](../service/quickstart-installation.md) para instalar o programa e criar uma área de trabalho.
* Operacionalização de Aprendizado de máquina do Azure requer um ambiente de implantação local e um [conta de gestão de modelos do Azure Machine Learning](model-management-overview.md).

Neste exemplo é executado em qualquer contexto de computação do Machine Learning Workbench. No entanto, é recomendado para executar o exemplo de com, pelo menos, 16 GB de memória. Este cenário foi criado e testado numa máquina de Windows 10 com um padrão de DS4_V2 remoto [Máquina Virtual de ciência de dados (DSVM) para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu).

Operacionalização de modelo foi efetuada com a versão 0.1.0a22 da CLI do Azure Machine Learning.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abra o Machine Learning Workbench.
2.  Sobre o **projetos** página, selecione **+** e, em seguida, selecione **novo projeto**.
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto.
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Manutenção preditiva" e selecione o **manutenção preditiva** modelo.
5.  Selecione **Criar**.

## <a name="prepare-the-notebook-server-computation-target"></a>Preparar o destino de computação de servidor do bloco de notas

Para executar no seu computador local, a partir do Machine Learning Workbench **arquivo** menu, selecione **linha de comandos aberta** ou **CLI do PowerShell aberta**. A interface da CLI permite-lhe aceder aos seus serviços do Azure através do `az` comandos. Primeiro, inicie sessão sua conta do Azure com o comando:

```
az login
``` 

Este comando fornece uma chave de autenticação para utilizar com o https:\\aka.ms\devicelogin URL. A CLI tem de aguardar até que a operação de início de sessão de dispositivo retorna e fornece algumas informações de ligação. Em seguida, se tiver um local [Docker](https://www.docker.com/get-docker) instalação, preparar o ambiente de computação local com o comando:

```
az ml experiment prepare --target docker --run-configuration docker
```

É preferível executar numa [DSVM para Linux (Ubuntu)](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu) requisitos de memória e disco. Depois da DSVM estiver configurada, prepare o ambiente do Docker remoto com os dois comandos seguintes:

```
az ml computetarget attach remotedocker --name [Connection_Name] --address [VM_IP_Address] --username [VM_Username] --password [VM_UserPassword]
```

Depois que estiver ligado ao contentor do Docker remoto, prepare o ambiente de computação DSVM Docker com o comando: 

```
az ml experiment prepare --target [Connection_Name] --run-configuration [Connection_Name]
```

Com o ambiente de computação de Docker preparado, abra o servidor de bloco de notas do Jupyter do Azure Machine Learning Workbench **blocos de notas** separador ou um servidor baseado no browser com o comando: 

```
az ml notebook start
```

Os blocos de notas de exemplo são armazenados no diretório do código. Os blocos de notas são configurados de forma são executados sequencialmente, a partir do primeiro bloco de notas (Code\1_data_ingestion.ipynb). Quando abre cada bloco de anotações, lhe for pedido para selecionar o kernel de computação. Escolha o kernel de modelo [nome_da_conexão] [Project_Name] para executar em DSVM configurado anteriormente.

## <a name="data-description"></a>Descrição de dados

O [simulated dados](https://github.com/Microsoft/SQL-Server-R-Services-Samples/tree/master/PredictiveMaintanenceModelingGuide/Data) consiste em cinco ficheiros de valores separados por vírgulas (. csv). Utilize as seguintes ligações para obter descrições detalhadas sobre os conjuntos de dados.

* [Máquinas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/machines.csv): recursos que distinguem cada máquina, como idade e modelo.
* [Erro](https://pdmmodelingguide.blob.core.windows.net/pdmdata/errors.csv): O registo de erro contém erros de sem interrupções que forem lançados enquanto a máquina está ainda está operacional. Estes erros não são considerados falhas, ainda que possam estar a previsão de um evento de falha de futuras. Os valores de data e hora para os erros são arredondados para a hora mais próxima, uma vez que os dados de telemetria são recolhidos por hora.
* [Manutenção](https://pdmmodelingguide.blob.core.windows.net/pdmdata/maint.csv): O registo de manutenção contém ambos os registros de manutenção agendada e não programado. Corresponde a manutenção agendada com a inspeção regular de componentes. Manutenção agendada pode surgir contra falhas mecânicas ou outra degradação do desempenho. Os valores de data e hora de manutenção são arredondados para a hora mais próxima, uma vez que os dados de telemetria são recolhidos por hora.
* [Telemetria](https://pdmmodelingguide.blob.core.windows.net/pdmdata/telemetry.csv): os dados de telemetria é composta por medidas de série de tempo de vários sensores dentro de cada máquina. Os dados são registados por uma média de valores de sensor ao longo de cada intervalo de uma hora.
* [Falhas](https://pdmmodelingguide.blob.core.windows.net/pdmdata/failures.csv): falhas correspondem a substituições de componente no registo de manutenção. Cada registro contém a ID da máquina, o tipo de componente e a data de substituição e a hora. Estes registos são utilizados para criar o machine learning etiquetas que o modelo está a tentar prever.

Para transferir os conjuntos de dados não processados a partir do repositório do GitHub e criar os conjuntos de dados para esta análise PySpark, consulte a [ingestão de dados](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb) cenário de bloco de notas do Jupyter na pasta do código.

## <a name="scenario-structure"></a>Estrutura do cenário
O conteúdo para o cenário está disponível na [repositório do GitHub](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance). 

O [Leiame](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/README.md) ficheiro descreve o fluxo de trabalho de preparação dos dados, criar um modelo e, em seguida, implantando uma solução para produção. Cada passo do fluxo de trabalho é encapsulado num bloco de notas do Jupyter no [código](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/tree/master/Code) pasta dentro do repositório.   

[Code\1_data_ingestion.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/1_data_ingestion.ipynb): este bloco de notas transfere os ficheiros. csv de entrada cinco e faz alguma limpeza de dados preliminar e visualização. O bloco de notas converte cada conjunto de dados no formato de PySpark e armazena-os num contentor de Blobs do Azure para utilização no bloco de notas funcionalidade de engenharia.

[Code\2_feature_engineering.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/2_feature_engineering.ipynb): os recursos de modelo são construídos a partir do conjunto de dados não processado do armazenamento de Blobs do Azure com uma abordagem de série de fuso horário para os dados de telemetria, erros e manutenção. As substituições relacionadas com a falha de componente são usadas para construir as etiquetas de modelo que descrevem o componente de sincronização falhou. Os dados de recurso com nome são salvos num blob do Azure para o bloco de notas de construção do modelo.

[Code\3_model_building.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/3_model_building.ipynb): O bloco de notas de criação de modelo utiliza o conjunto de dados de recursos identificadas e divide os dados em conjuntos de dados de treinamento e desenvolvimento juntamente com o carimbo de data / hora. O bloco de notas é configurado como uma experimentação com modelos de pyspark.ml.classification. Os dados de treinamento são vetorizados. O utilizador pode experimentar com um um **DecisionTreeClassifier** ou **RandomForestClassifier** para manipular hiperparâmetros para encontrar o melhor desempenho de modelo. Desempenho é determinado através da avaliação de estatísticas de medida no conjunto de dados de desenvolvimento. Estas estatísticas são registadas no controlo no ecrã de tempo de execução de Machine Learning Workbench. Em cada execução, o bloco de notas guarda do modelo resultante para o disco local que está a executar o kernel de bloco de notas do Jupyter. 

[Code\4_operationalization.ipynb](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance/blob/master/Code/4_operationalization.ipynb): este bloco de notas utiliza o último modelo que é guardado no disco local (kernel de bloco de notas do Jupyter) para criar os componentes de implementação do modelo num serviço web do Azure. Os ativos completa operacionais são compactados no arquivo o16n.zip que esteja armazenado em outro contentor de Blobs do Azure. O arquivo zipado contém:

* **service_schema**: O ficheiro de definição de esquema para a implementação. 
* **pdmscore.PY**: A **Init ()** e **run()** serviço web de funções que são necessários para o Azure.
* **pdmrfull.Model**: O diretório de definição de modelo.
    
O bloco de notas testa as funções com a definição de modelo antes de empacotar os ativos de operacionalização para a implementação. Instruções para a implementação estão incluídas no final do bloco de notas.

## <a name="conclusion"></a>Conclusão

Este cenário dá uma visão geral da criação de uma solução de manutenção preditiva ponto-a-ponto utilizando PySpark dentro do ambiente de bloco de notas do Jupyter no Machine Learning Workbench. Neste cenário de exemplo também fornece detalhes sobre a implementação do modelo através do ambiente de gestão de modelos do Machine Learning para fazer predições de falhas de equipamento em tempo real.

## <a name="references"></a>Referências

As referências seguintes fornecem exemplos de outra manutenção preditiva casos de utilização para várias plataformas:

* [Modelo de solução de manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/cortana-analytics-playbook-predictive-maintenance)
* [Guia de modelagem de manutenção preditiva](https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Modelling-Guide-1)
* [Preditiva manutenção modelagem guia usando serviços R do SQL](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1)
* [O bloco de notas do guia Python de modelagem de manutenção preditiva](https://gallery.cortanaintelligence.com/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1)
* [Manutenção Preditiva com PySpark](https://gallery.cortanaintelligence.com/Tutorial/Predictive-Maintenance-using-PySpark)
* [Aprendizagem para manutenção preditiva](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance)

## <a name="next-steps"></a>Passos Seguintes

Outros cenários de exemplo estão disponíveis no Machine Learning Workbench demonstrar funcionalidades adicionais do produto. 
