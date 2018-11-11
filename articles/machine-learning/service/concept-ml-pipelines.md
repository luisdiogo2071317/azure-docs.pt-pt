---
title: Criar pipelines do machine learning - serviço do Azure Machine Learning
description: Neste artigo, saiba mais sobre o machine learning pipelines, que pode criar com o Azure Machine Learning SDK para Python e as vantagens da utilização de pipelines. Pipelines (ML) de aprendizado de máquina são utilizados por cientistas de dados para criar, otimizar e gerir os seus fluxos de trabalho de aprendizagem.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 11/07/2018
ms.openlocfilehash: 212488b74ba726012966787c7b2de633414e96a4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245346"
---
# <a name="pipelines-and-azure-machine-learning"></a>Pipelines e do Azure Machine Learning

Neste artigo, saiba mais sobre o machine learning pipelines, que pode criar com o Azure Machine Learning SDK para Python e as vantagens da utilização de pipelines.

## <a name="what-are-machine-learning-pipelines"></a>Quais são os pipelines de aprendizagem automática?

Utilizar pipelines (ML) de aprendizado de máquina, cientistas de dados, engenheiros de dados e os profissionais de TI pode colaborar nas etapas envolvidas na:
+ Preparação de dados, como normalizations e transformações
+ Preparação de modelos
+ Avaliação do modelo
+ Implementação 

O diagrama seguinte mostra um pipeline de exemplo:

[ ![De machine learning pipelines no serviço Azure Machine Learning](./media/concept-ml-pipelines/pipelines.png) ] (. / media/concept-ml-pipelines/machine-learning-pipelines-big.png#lightbox)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Porquê criar pipelines com o Azure Machine Learning?

O [do Azure Machine Learning SDK para Python](#the-python-sdk-for-pipelines) pode ser utilizado para criar pipelines de ML, bem como submeter e monitorizar as execuções de pipeline individuais.

Com os pipelines, pode otimizar o seu fluxo de trabalho com simplicidade, velocidade, portabilidade e reutilização. Ao criar pipelines com o Azure Machine Learning, pode se concentrar em daquilo que conhece melhor &mdash; aprendizagem &mdash; e não na infraestrutura.

Usar as etapas distintas torna possível voltar a executar apenas as etapas que necessárias à medida que ajustar e testar o fluxo de trabalho. Um passo é uma unidade computacional no pipeline. Conforme mostrado no diagrama acima, a tarefa de preparação de dados pode envolver muitos passos, incluindo, mas não se limitando a, normalização, transformação, validação e featurization. Origens de dados e dados intermediários são reutilizados em pipeline, que poupa tempo e recursos de computação. 

Assim que o pipeline foi concebido, há muitas vezes mais ajustar o loop de treinamento do pipeline. Quando voltar a executar um pipeline, a saltar execução para as etapas que precisam ser executados novamente, por exemplo, um script de treinamento atualizado e ignora o que não mudou. O paradigma do mesmo se aplica a inalterado scripts utilizados para a execução do passo. 

Com o Azure Machine Learning, pode utilizar vários kits de ferramentas e estruturas, como o Microsoft Cognitive Toolkit ou TensorFlow para cada etapa no seu pipeline. As coordenadas do Azure entre os diversos [destinos de computação](concept-azure-machine-learning-architecture.md) utilizar, para que os seus dados intermediários podem ser facilmente partilhados com os destinos de computação de downstream. 

## <a name="key-advantages"></a>Principais vantagens

As principais vantagens para a criação de pipelines para de aprendizagem para fluxos de trabalho é:

|Principais vantagens|Descrição|
|:-------:|-----------|
|**Autónoma&nbsp;é executado**|Agende algumas etapas para executar em paralelo ou na sequência de forma fiável e autónoma. Desde a preparação de dados e modelagem pode pela última vez dias ou semanas, pode agora se concentrar em outras tarefas enquanto seu pipeline está em execução. |
|**Mista e diversificada de computação**|Utilize vários pipelines que são fiável coordenados entre computações heterogêneas e dimensionáveis e de armazenamento. Passos de individual pipeline podem ser executados na computação diferentes destinos, como o HDInsight, VMs de ciência de dados de GPU e Databricks, para tornar eficiente utilizam opções de computação disponíveis.|
|**Capacidade de reutilização**|Podem ser templatized pipelines para cenários específicos, como reparametrização e classificação de lote.  Eles podem ser acionados de sistemas externos por meio de chamadas REST simples.|
|**Controlo de versões e controlo**|Em vez de controlar manualmente os dados e o resultado caminhos como iterar, utilizam os SDK de pipelines explicitamente o nome e versão seus dados de origens, entradas e saídas, bem como gerir scripts e os dados separadamente para uma maior produtividade.|

## <a name="the-python-sdk-for-pipelines"></a>O SDK de Python para pipelines

Utilizar o Python para criar os seus pipelines de ML. O SDK do Azure Machine Learning oferece construções imperativas para sequenciar e paralelização os passos em seus pipelines quando nenhuma dependência de dados está presente. Pode interagir com ele em blocos de notas do Jupyter ou em outro IDE preferido. 

Através de dependências de dados declarativa, pode otimizar as suas tarefas. O SDK inclui uma estrutura de módulos criados previamente para tarefas comuns, como a transferência de dados e a publicação de modelo. O framework pode ser estendido para modelar suas próprias convenções implementando passos personalizados que são reutilizáveis em pipelines. Destinos de computação e de recursos de armazenamento também podem ser gerida diretamente a partir do SDK.

Pipelines podem ser salvas como modelos e podem ser implementados para um ponto final REST, para que pode agendar as tarefas de classificação de lote ou reparametrização.

Veja a [documentos de referência do Python SDK para pipelines](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) e o bloco de notas na secção seguinte para saber como criar suas próprias.

## <a name="example-notebooks"></a>Blocos de notas de exemplo
 
O bloco de notas seguinte demonstra pipelines com o Azure Machine Learning: [pipeline/pipeline-batch-scoring.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb).
 
Obter este bloco de notas:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
