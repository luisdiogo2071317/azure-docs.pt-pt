---
title: Acerca
titleSuffix: Azure Machine Learning service
description: Descrição geral do serviço do Azure Machine Learning--uma solução de ciência de dados ponto a ponto integrada para cientistas de dados profissionais desenvolver, experimentar e implementar aplicações de análise avançada à escala da cloud.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: f0860982da6a06a3529539768179f9bc28a0695a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53272988"
---
# <a name="what-is-azure-machine-learning-service"></a>O que é o serviço Azure Machine Learning?

O serviço do Azure Machine Learning é um serviço cloud que pode utilizar para preparar, implementar, automatizar e gerir modelos de aprendizagem automática, tudo na escala amplo que a cloud oferece.

## <a name="what-is-machine-learning"></a>O que é o Machine Learning?

A aprendizagem automática é uma técnica da ciência de dados que permite aos computadores utilizar dados existentes para prever futuros comportamentos, resultados e tendências. Ao utilizarem a aprendizagem automática, os computadores aprendem sem serem explicitamente programados.

As previsões ou predições da aprendizagem automática podem tornar as aplicações e os dispositivos mais inteligentes. Por exemplo, quando faz compras online, o machine learning recomenda outros produtos que poderá gostar com base nos que já comprou. Quando o seu cartão de crédito é utilizado, o machine learning compara a transação com uma base de dados de transações e ajuda a detetar fraudes. Quando o robô aspirador limpa uma sala, o machine learning ajuda-o a decidir se a tarefa está concluída.

## <a name="what-is-azure-machine-learning-service"></a>O que é o serviço Azure Machine Learning?

O serviço Azure Machine Learning fornece um ambiente baseado na cloud que pode utilizar para criar, preparar, testar, implementar, gerir e controlar os modelos de machine learning.

[ ![Fluxo de trabalho do serviço Azure Machine Learning](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

O serviço Azure Machine Learning suporta totalmente tecnologias de open source, pelo que pode utilizar dezenas de milhares de pacotes Python de open source com componentes de machine learning, como o TensorFlow e o scikit-learn.
Suporte para ferramentas avançadas, como [blocos de notas do Jupyter](http://jupyter.org) ou o [do Azure Machine Learning para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai#overview) extensão, facilita a interativamente explorar dados, transformá-los e, em seguida, desenvolver e testar modelos.
O serviço Azure Machine Learning também inclui funcionalidades que [automatizam a otimização e a geração de modelos](tutorial-auto-train-models.md) para ajudá-lo a criar modelos com facilidade, eficiência e precisão.

O serviço Azure Machine Learning permite-lhe começar a preparar o seu computador local e, em seguida, aumentar horizontalmente para a cloud. Em muitas disponíveis [destinos de computação](how-to-set-up-training-targets.md) , como a computação do Azure Machine Learning e [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)e com [avançada dos serviços de otimização de hiper-parâmetros](how-to-tune-hyperparameters.md), pode criar melhores modelos com mais rapidez, usando o poder da cloud.

Quando tiver o modelo certo, pode implementá-lo facilmente num contentor, como o Docker. Isso significa que é simples de implementar no Azure Container Instances ou serviço Kubernetes do Azure ou pode utilizar o contentor em suas próprias implementações, no local ou na cloud. Para obter mais informações, consulte a [como implementar e, em que](how-to-deploy-and-where.md) documento.
Pode gerir os modelos implementados e controlar várias execuções à medida que experimenta para encontrar a melhor solução.
Depois de implementada, o seu modelo pode retornar previsões em [em tempo real](how-to-consume-web-service.md), ou [assincronamente](how-to-run-batch-predictions.md) em grandes quantidades de dados.

E com avançados [de machine learning pipelines](concept-ml-pipelines.md), pode colaborar em todos os passos de preparação de dados, preparação de modelos e avaliação e implantação.

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>O que posso fazer com o serviço Azure Machine Learning?

O serviço do Azure Machine Learning pode auto-preparar um modelo e otimização automáticas-lo para.
Por exemplo, veja [Tutorial: Preparar automaticamente um modelo de classificação com o Azure automatizada Machine Learning](tutorial-auto-train-models.md).

Utilizar o Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> para o Python, juntamente com pacotes de Python de código-fonte aberto, que pode criar e treinar a aprendizagem altamente precisos e aprendizagem aprofundada modela-se numa área de trabalho do serviço Azure Machine Learning.
Pode escolher de entre vários componentes de machine learning disponíveis em pacotes Python de open source, como os seguintes:

- <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Depois de ter um modelo, usá-lo para criar um contentor (por exemplo, o Docker) que pode ser implementado localmente para fins de teste. Assim que o teste é realizado, o modelo pode ser implementado como um serviço da web de produção no Azure Container Instances ou no serviço Kubernetes do Azure. Para obter mais informações, consulte a [como implementar e, em que](how-to-deploy-and-where.md) documento.

Em seguida, pode gerir seus modelos implementados com o [do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk) ou o [portal do Azure](https://portal.azure.com/).
Pode avaliar as métricas de modelo, reformular e voltar a implementar novas versões do modelo, ao mesmo tempo experimentações do modelo de controlo.

Para começar a utilizar o serviço Azure Machine Learning, veja os [Passos seguintes](#next-steps) que estão abaixo.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Qual é a diferença entre o serviço Azure Machine Learning e o Azure Machine Learning Studio?

O Azure Machine Learning Studio é uma área de trabalho visual de colaboração de arrastar e largar onde pode criar, testar e implementar soluções de machine learning sem a necessidade de escrever código. Utiliza algoritmos de machine learning pré-criados e pré-configurados e módulos de processamento de dados.

Utilize o Machine Learning Studio quando quiser experimentar modelos de machine learning de forma rápida e fácil, e quando os algoritmos de machine learning incorporados forem suficientes para as suas soluções.

Utilize o serviço Machine Learning se trabalhar num ambiente Python, quiser mais controlo sobre os algoritmos de machine learning ou quiser utilizar bibliotecas de machine learning de open source.

> [!NOTE]
> Os modelos criados no Azure Machine Learning Studio não podem ser implementados nem geridos pelo serviço Azure Machine Learning.

## <a name="free-trial"></a>Avaliação gratuita

Se não tiver uma subscrição do Azure, crie uma conta gratuita antes de começar. Experimente o [uma versão gratuita ou paga do serviço Azure Machine Learning](http://aka.ms/AMLFree) hoje mesmo.

Receberá créditos para gastar em serviços do Azure. Depois de serem utilizados, pode manter a conta e utilizar os [serviços gratuitos do Azure](https://azure.microsoft.com/free/). O seu cartão de crédito não será cobrado, a menos que altere explicitamente as suas definições e peça para ser cobrado. Em alternativa, pode [ativar os benefícios de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), que dá-lhe créditos todos os meses que pode utilizar para os serviços do Azure pagos.

## <a name="next-steps"></a>Passos Seguintes

- Criar uma área de trabalho da serviço de aprendizagem máquina para começar a utilizar [através do portal do Azure](quickstart-get-started.md) ou [em Python](quickstart-create-workspace-with-python.md).

- Siga o tutorial completo, [Train e implementar um modelo de classificação de imagem com o Azure Machine Learning](tutorial-train-models-with-aml.md).

- [Utilizar o Azure Machine Learning para gerar automaticamente e um modelo de autotune](tutorial-auto-train-models.md).

- Saiba mais sobre [pipelines de aprendizagem automática](/azure/machine-learning/service/concept-ml-pipelines) para criar, otimizar e gerir cenários da aprendizagem automática.

- Leia o aprofundada [arquitetura de serviço do Azure Machine Learning e conceitos](concept-azure-machine-learning-architecture.md) artigo.

- Para obter mais informações sobre outros produtos de machine learning da Microsoft, consulte [outros produtos da Microsoft de aprendizagem](./overview-more-machine-learning.md).


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
