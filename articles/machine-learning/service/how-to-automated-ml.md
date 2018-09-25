---
title: O que é automatizada Machine Learning - Azure Machine Learning
description: Neste artigo, pode aprender sobre aprendizagem automática. O serviço Azure Machine Learning automaticamente pode escolher um algoritmo para e gerar um modelo a partir do mesmo. Automatizado aprendizagem automática ajuda a poupar tempo utilizando os parâmetros e os critérios fornecidos para selecionar o melhor algoritmo para o seu modelo.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: krishnan
author: krishnaanumalasetty
ms.date: 9/24/2018
ms.openlocfilehash: 2a9c05b68d05102fab80b2aa8fb1c1dad8a367ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960044"
---
# <a name="what-is-automated-machine-learning"></a>O que é automatizado aprendizagem automática?

Neste artigo, pode aprender sobre aprendizagem automática. O serviço Azure Machine Learning automaticamente pode escolher um algoritmo para e gerar um modelo a partir do mesmo. Automatizado aprendizagem automática ajuda a poupar tempo utilizando os parâmetros e os critérios fornecidos para selecionar o melhor algoritmo para o seu modelo.

## <a name="how-it-works"></a>Como funciona

1. Configurar o tipo de problema de aprendizado de máquina que está a tentar resolver. Duas categorias de aprendizagem supervisionada são suportadas:
   + Classificação
   + Regressão

   Consulte a [lista de modelos](how-to-configure-auto-train.md#select-your-experiment-type) pode experimentar o Azure Machine Learning ao treinar.

1. Especifique a origem e o formato dos dados de treinamento. Os dados devem ser o nome e podem ser armazenados no seu ambiente de desenvolvimento ou no armazenamento de Blobs do Azure. Se os dados são armazenados no seu ambiente de desenvolvimento, tem de estar no mesmo diretório que seus scripts de treinamento. Este diretório é copiado para o destino de computação que selecionar para treinamento.

    No seu script de treinamento, os dados podem ser lidos em matrizes de Numpy ou um Pandas dataframe.

    Pode configurar opções de divisão para selecionar dados de treinamento e validação, ou pode especificar os conjuntos de dados de treinamento e validação separados.

1. Configurar o [destino de computação](how-to-set-up-training-targets.md) que serve para preparar o modelo.

1. Configure a configuração de aprendizado de máquina automatizada. Esse item controla os parâmetros utilizados como o Azure Machine Learning é iterada em modelos diferentes, as definições de hiper-parâmetros, e que métricas para ver quando determinar o melhor modelo. 

1. Submeta um treinamento ser executado.

Durante o treinamento, o serviço Azure Machine Learning cria um número de pipelines que tente parâmetros e algoritmos diferentes. Ele irá parar quando atingir o limite de iteração que é fornecer ou quando ela atinge o valor de destino para a métrica que especificar.

[ ![Aprendizagem automática](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Pode inspecionar as informações de execução com sessão iniciada, que contém a métricas recolhidas durante a execução. A execução de treinamento também produz um objeto de Python serializados (ficheiro .pkl) que contém o modelo e, em seguida, o pré-processamento de dados.

## <a name="next-steps"></a>Passos Seguintes

Ver exemplos e saiba como criar modelos com o Machine Learning automatizada:

+ [Tutorial: Preparar automaticamente um modelo de classificação com o Azure automatizada Machine Learning](tutorial-auto-train-models.md)

+ [Como configurar as definições para treinamento automática](how-to-configure-auto-train.md)

+ [Como usar o treinamento automática num recurso remoto](how-to-auto-train-remote.md) 
