---
title: Análise de sentimentos com aprendizagem profunda com o Azure Machine Learning | Documentos da Microsoft
description: Como realizar uma análise de sentimentos com aprendizagem profunda com o Azure ML Workbench.
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
ms.date: 01/20/2018
ms.author: miprasad
ROBOTS: NOINDEX
ms.openlocfilehash: e2fbb0b7b0dede198be0e57ffcd2b58a7da7fce7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947776"
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Análise de sentimentos com aprendizagem profunda com o Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Análise de sentimentos é uma tarefa bem conhecida no território de processamento de linguagem natural. Devido um conjunto de textos, o objetivo é determinar o sentimento desse texto. O objetivo dessa solução é usar a aprendizagem profunda para prever o sentimento de análises de filmes.

A solução está localizada em https://github.com/Azure/MachineLearningSamples-SentimentAnalysis

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub de galeria

Siga esta ligação para o repositório do GitHub público:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Descrição geral de caso de utilização

A proliferação de dispositivos móveis e a explosão de dados criou muitas oportunidades para os clientes expressar suas sentimentos e atitudes sobre tudo o que em qualquer altura. Este opinião ou "sentimento", muitas vezes, é gerado por meio de canais de redes sociais na forma de revisões, conversa, partilhas, gosta, tweets, etc. O sentimento pode ser inestimável para as empresas que buscam melhorar os produtos e serviços, tomar decisões mais informadas e promover melhor marcas.

Para obter o valor da análise de sentimentos, as empresas tem de ter a capacidade de extrair grandes arquivos de dados não estruturados de redes sociais para obter informações acionáveis. Neste exemplo, vamos desenvolver modelos de aprendizagem profunda para a execução de análise de sentimentos de análises de filmes com AMLWorkbench

## <a name="prerequisites"></a>Pré-requisitos

* Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).

* Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [guia de início rápido de instalação](quickstart-installation.md) para instalar o programa e criar uma área de trabalho.

* Para operacionalização, é melhor se tiver o motor do Docker instalado e em execução localmente. Caso contrário, pode utilizar a opção de cluster. No entanto, a execução de um Azure Container Service (ACS) pode ser caro.

* Esta solução assume que está a executar Azure Machine Learning Workbench no Windows 10 com o motor de Docker instalado localmente. Num macOS, a instrução é basicamente o mesmo.

## <a name="data-description"></a>Descrição de dados

O conjunto de dados utilizado para este exemplo é um pequeno conjunto de dados criados manualmente e está localizado no [pasta dados](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data).

A primeira coluna contém análises de filmes e a segunda coluna contém o sentimento (0 - negativos e 1 - positivo). O conjunto de dados é utilizado apenas para fins de demonstração, mas, normalmente, para obter as classificações de sentimentos robusto, é necessário um conjunto de dados grandes. Por exemplo, o [IMDB filme analisa o problema de classificação de sentimento](https://keras.io/datasets/#datasets ) do Keras consiste num conjunto de dados das revisões de 25.000 filmes de IMDB, etiquetados pelo sentimento (positivo ou negativo). A intenção deste laboratório é mostrar a como realizar uma análise de sentimentos com aprendizagem profunda com AMLWorkbench.

## <a name="scenario-structure"></a>Estrutura do cenário

A estrutura de pastas é organizada da seguinte forma:

1. Todo o código relacionadas com a análise de sentimentos com AMLWorkbench está na pasta raiz
2. dados: contém o conjunto de dados utilizado na solução
3. Docs: contém todos os laboratórios práticos

A ordem dos laboratórios práticos para executar a solução é o seguinte:

| Encomenda| Nome de Ficheiro | Ficheiros relacionados |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 3 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Conclusão

Concluindo, essa solução apresenta a utilizar a aprendizagem profunda para efetuar a análise de sentimentos com o Azure Machine Learning Workbench. Também podemos operacionalizar através de modelos de HDF5.
