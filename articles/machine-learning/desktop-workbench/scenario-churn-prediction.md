---
title: Previsão de abandono de clientes com o Azure Machine Learning | Documentos da Microsoft
description: Como efetuar a análise de alterações a dados com o Azure ML Workbench.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: 7c7b50098cfd1bcac534156dd905b37affab80bd
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648219"
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Previsão de abandono de clientes com o Azure Machine Learning

Em média, a manter os clientes existentes é cinco vezes mais barato do que o custo de recrutamento de novos métodos. Como resultado, os executivos de marketing muitas vezes descobrem tentar calcular a probabilidade de abandono de clientes e determinar as ações necessárias para minimizar a taxa de abandono.

O objetivo dessa solução é demonstrar a análise de alterações a dados Preditiva com o Azure Machine Learning Workbench. Esta solução oferece um modelo fácil de usar para desenvolver pipelines de dados de previsão de abandono para varejistas. O modelo pode ser utilizado com diferentes conjuntos de dados e definições diferentes das alterações. O objetivo do exemplo prático é:

1. Compreenda as ferramentas de preparação de dados do Azure Machine Learning Workbench para limpar e ingestão de dados de relação do cliente para a análise de alterações a dados.

2. Execute a transformação de funcionalidade para processar dados heterogêneos ruidosos.

3. Integrar as bibliotecas de terceiros (por exemplo, `scikit-learn` e `azureml`) para desenvolver Bayesianos e classificadores baseados em árvore para prever o volume de alterações.

4. Implemente.

## <a name="link-of-the-gallery-github-repository"></a>Ligação do repositório do GitHub de galeria
Segue-se a ligação para o repositório público do GitHub, onde está alojado todo o código:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Descrição geral de caso de utilização
As empresas precisam de uma estratégia eficaz para gerir o abandono de clientes. Abandono de clientes inclui os clientes pararem de utilizar um serviço, mudar para um serviço da concorrência, mudar para uma experiência de escalão inferior no serviço ou reduzirem o compromisso para com o serviço.

Neste caso, vamos ver os dados da empresa de telecomunicações francesa laranja para identificar os clientes que estejam a probabilidades de abandono a curto prazo para melhorar o serviço e criar campanhas de sensibilização que ajudam a manter os clientes.

Empresas de telecomunicações enfrentam um mercado competitivo. Várias operadoras perdem receita de clientes pós-pagos devido ao abandono. Por conseguinte, a capacidade de identificar com precisão o abandono de clientes pode ser uma enorme vantagem competitiva.

Alguns dos fatores que contribuem para o abandono de clientes de telecomunicações incluem:

* Interrupções de serviço frequentes percebido
* Experiências de serviço ruim para o cliente em lojas online/revenda
* Ofertas de outras operadoras concorrentes (melhor plano para a família, plano de dados, etc.).

Nesta solução, utilizamos um exemplo concreto da criação de modelo de alterações a um cliente preditivo para empresas de telecomunicações.

## <a name="prerequisites"></a>Pré-requisitos

* Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis)

* Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [guia de início rápido de instalação](../service/quickstart-installation.md) para instalar o programa e criar uma área de trabalho

* Para operacionalização, é melhor se tiver o motor do Docker instalado e em execução localmente. Caso contrário, pode utilizar a opção de cluster, mas lembre-se de que pode ser dispendioso executar um Azure Container Service (ACS).

* Esta solução assume que está a executar Azure Machine Learning Workbench no Windows 10 com o motor de Docker instalado localmente. Se estiver a utilizar o macOS a instrução é basicamente o mesmo.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Previsão de abandono de clientes" e selecione o modelo
5.  Clique em **Criar**.

## <a name="data-description"></a>Descrição de dados

É o conjunto de dados utilizado na solução da concorrência SIDKDD 2009. Ele é chamado `CATelcoCustomerChurnTrainingSample.csv` e está localizado no [ `data` ](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data) pasta. O conjunto de dados consiste em dados ruidosos heterogéneos (variáveis numéricas/categórica) da empresa de telecomunicações francês laranja e é anónimos.

As variáveis de capturam informações demográficas do cliente, estatísticas de chamada (como a duração média das chamadas, taxa de falhas de chamada, etc.), informações, estatísticas de reclamação de contrato. Alterações a variável é binário (0 - não alterações, 1 - alterações a dados).

## <a name="scenario-structure"></a>Estrutura do cenário

A estrutura de pastas é organizada da seguinte forma:

__dados__: contém o conjunto de dados utilizado na solução  

__Docs__: contém todos os laboratórios práticos

A ordem dos laboratórios práticos para executar a solução é o seguinte:
1. Preparação de dados: É o principal arquivo relacionados com a preparação de dados na pasta de dados `CATelcoCustomerChurnTrainingSample.csv`
2. Avaliação e modelação: é o arquivo principal relacionado à modelagem e avaliação na pasta raiz `CATelcoCustomerChurnModeling.py`
3. Modelagem e de avaliação sem .dprep: é o arquivo principal para esta tarefa na pasta raiz `CATelcoCustomerChurnModelingWithoutDprep.py`
4. Operacionalização: Os ficheiros de principais de implementação são o modelo (`model.pkl`) e `churn_schema_gen.py`

| Encomenda| Nome de Ficheiro | Ficheiros de Realted |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | 'data/CATelcoCustomerChurnTrainingSample.csv' |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | 'CATelcoCustomerChurnModeling.py' |
| 3 | [`ModelingAndEvaluationWithoutDprep.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationWithoutDprep.md) | 'CATelcoCustomerChurnModelingWithoutDprep.py' |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | 'pkl'<br>'churn_schema_gen.py' |

Siga os laboratórios da forma sequencial, descrito acima.

## <a name="conclusion"></a>Conclusão
Isso entrega cenário demonstrado como realizar a previsão de abandono de utilizar o Azure Machine Learning Workbench. Efetuámos primeiro para processar dados desnecessárias e heterogéneos, seguidos de engenharia de funcionalidades com as ferramentas de preparação de dados de limpeza de dados. Podemos, em seguida, utilizar ferramentas de aprendizagem automática de código-fonte aberto para criar e avaliar um modelo de classificação, em seguida, utilizado o contentor do local docker para implementar o modelo, tornando-o pronto para produção.
