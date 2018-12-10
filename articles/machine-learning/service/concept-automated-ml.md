---
title: O que é o Machine Learning automatizada
titleSuffix: Azure Machine Learning service
description: Saiba mais sobre como serviço Azure Machine Learning pode automaticamente escolher um algoritmo para e gerar um modelo com ele para poupar que tempo utilizando os parâmetros e os critérios fornecidos para selecionar o melhor algoritmo para o seu modelo.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8c1641c975ff235b581f784f0965caf203a9562f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53142535"
---
# <a name="what-is-automated-machine-learning"></a>O que é automatizado aprendizagem automática?

Aprendizagem automática é o processo de mudança dos dados de treinamento com um recurso de destino definidos e fazendo a iteração por meio de combinações de algoritmos e seleções de funcionalidade para selecionar automaticamente o melhor modelo para os seus dados com base nas pontuações de treinamento. A processo de desenvolvimento do modelo de aprendizagem tradicional é altamente com muitos recursos e requer investimento de dados de conhecimento e a hora de domínio significativo para executar e comparar os resultados de dezenas de modelos. Aprendizagem automática simplifica este processo através da geração de modelos ajustados de objetivos e restrições definidas para a experimentação, como o tempo para a experimentação para execução ou que modela à lista de bloqueio.

## <a name="how-it-works"></a>Como funciona

1. Configurar o tipo de problema de aprendizado de máquina que está a tentar resolver. Categorias de aprendizagem supervisionada são suportadas:
   + Classificação
   + Regressão
   + Previsão

   Enquanto a aprendizagem automática está disponível em geral, **a previsão funcionalidade ainda está em pré-visualização pública.**

   Consulte a [lista de modelos](how-to-configure-auto-train.md#select-your-experiment-type) pode experimentar o Azure Machine Learning ao treinar.

1. Especifique a origem e o formato dos dados de treinamento. Os dados devem ser o nome e podem ser armazenados no seu ambiente de desenvolvimento ou no armazenamento de Blobs do Azure. Se os dados são armazenados no seu ambiente de desenvolvimento, tem de estar no mesmo diretório que seus scripts de treinamento. Este diretório é copiado para o destino de computação que selecionar para treinamento.

    No seu script de treinamento, os dados podem ser lidos em matrizes de Numpy ou um Pandas dataframe.

    Pode configurar opções de divisão para selecionar dados de treinamento e validação, ou pode especificar os conjuntos de dados de treinamento e validação separados.

1. Configurar o [destino de computação](how-to-set-up-training-targets.md) que serve para preparar o modelo.

1. Configure a configuração de aprendizado de máquina automatizada. Esse item controla os parâmetros utilizados como o Azure Machine Learning é iterada em modelos diferentes, as definições de hiper-parâmetros, e que métricas para ver quando determinar o melhor modelo 

1. Submeta um treinamento ser executado.

Durante o treinamento, o serviço Azure Machine Learning cria um número de pipelines que tente parâmetros e algoritmos diferentes. Ele irá parar quando atingir o limite de iteração que é fornecer ou quando ela atinge o valor de destino para a métrica que especificar.

[ ![Aprendizagem automática](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Pode inspecionar as informações de execução com sessão iniciada, que contém a métricas recolhidas durante a execução. A execução de treinamento também produz um objeto de Python serializados (`.pkl` ficheiro) que contém o modelo e, em seguida, o pré-processamento de dados.

## <a name="model-explainability"></a>Modelo explainability

Uma armadilha comum da aprendizagem automática é uma incapacidade de ver o processo de ponto-a-ponto. O Azure Machine Learning permite-lhe ver informações detalhadas sobre os modelos para aumentar a transparência no que está sendo executado no back-end. Saída mostra importância de funcionalidade geral no modelo de otimização, classificação os resultados pelos recursos que influenciada mais o seu modelo. Além disso, para problemas de classificação pode ver a importância de recurso por classe e classificação.

## <a name="next-steps"></a>Passos Seguintes

Ver exemplos e saiba como criar modelos com o Machine Learning automatizada:

+ [Tutorial: Preparar automaticamente um modelo de classificação com o Azure automatizada Machine Learning](tutorial-auto-train-models.md)

+ [Configurar as definições para treinamento automática](how-to-configure-auto-train.md)

+ [Utilizar o treinamento automática num recurso remoto](how-to-auto-train-remote.md) 
