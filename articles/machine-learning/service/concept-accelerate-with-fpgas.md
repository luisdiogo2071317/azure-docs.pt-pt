---
title: O que é um FPGA? – Projeto Brainwave – Azure Machine Learning
description: Saiba como acelerar as redes neurais profundo com FPGAs e modelos.
services: machine-learning
ms.service: machine-learning
ms.component: service
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5ed94c3b750c927ec48959c12388bd22de3d3df4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261955"
---
# <a name="what-is-fpga-and-project-brainwave"></a>O que é FPGA e projeto Brainwave?

Este artigo fornece uma introdução às matrizes de porta campo programável (FPGA) e como FPGA está integrado com o Azure machine learning para fornecer AI em tempo real.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs vs. CPU, GPU e ASIC

FPGAs conter uma matriz de blocos de lógica programável e uma hierarquia de reconfigurable interconnects que permitem que os blocos de formas diferentes de ser configurados após fabrico.

FPGAs fornecem uma combinação de programação para e de desempenho a comparação com outras chips:

![Comparação do Azure Machine Learning FPGA](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Unidades de processamento central (CPU)** são processadores para fins gerais. Desempenho da CPU não é ideal para o processamento de vídeo e gráficos.
- **Gráficos (GPU) de unidades de processamento** oferecer processamento paralelo e são uma opção para computações AI popular. O paralelo processamento com GPUs resultado numa composição de imagem mais rápida de CPUs.
- **Específicas da aplicação integrado circuitos (ASIC)**, como unidades de processador de TensorFlow da Google, são circuitos personalizados. Embora estas chips fornecem a eficiência máxima, ASICs são inflexible.
- **FPGAs**, tais como disponíveis no Azure, fornecer o desempenho próximo ASIC mas oferecem flexibilidade para ser reconfiguradas mais tarde.

FPGAs agora estão a ser cada novo servidor do Azure. Microsoft já está a utilizar FPGAs para avaliação de rede (DNN) da função de classificação de profundidade neuronal de pesquisa do Bing e aceleração de (SDN) redes definidas por software. Estas implementações FPGA reduzem a latência ao libertar CPUs para outras tarefas.

## <a name="project-brainwave-on-azure"></a>Brainwave projeto no Azure

Projeto Brainwave é uma arquitetura de hardware que foi concebida com base em FPGA dispositivos da Intel e utilizado para acelerar os cálculos de AI em tempo real. Com esta arquitetura preparados FPGA económica, uma rede neuronal treinada pode ser executada mais rapidamente possível e com uma latência inferior. Projeto Brainwave pode parallelize DNNs previamente treinados em FPGAs para aumentar horizontalmente o seu serviço.

- Desempenho

    FPGAs possibilitam a alcançar baixa latência para pedidos de inferencing em tempo real. Criação de batches significa dividi um pedido em partes mais pequenas e feeding-los para um processador para melhorar a utilização de hardware. Criação de batches não é eficiente e pode fazer com que a latência. Brainwave não requer a criação de batches; Por conseguinte, a latência é 10 vezes inferior em comparação comparadas a CPU e a GPU.

- Flexibilidade

    FPGAs podem ser reconfigurados para diferentes tipos de modelos de machine learning. Esta flexibilidade facilita acelerar as aplicações com base no mais precisão numérica ideal e modelo dentro da memória que está a ser utilizado.

    Novas técnicas de aprendizagem máquina estão a ser desenvolvidas regularmente e design de hardware do projeto Brainwave também evolução das rapidamente. Uma vez que FPGAs reconfigurable, é possível mantenha-se atualizado com os requisitos dos algoritmos de AI rapidamente a alteração.

- Escala

    Microsoft Azure é investimento de maior nuvem o mundo no FPGAs. Pode executar Brainwave na infraestrutura de escala do Azure.

Uma pré-visualização dos Brainwave projeto integrado com o Azure Machine Learning está atualmente disponível. E também está disponível para colocar projeto Brainwave limite, para que pode tirar partido desse velocidade de informática na sua própria empresas e instalações uma pré-visualização limitada.

Pré-visualização atual, Brainwave é limitado para a implementação de TensorFlow e redes neurais ResNet50 baseada em hardware Intel FPGA para classificação de imagem e reconhecimento. Não existem planos para suportar mais de Galeria de modelos e outras estruturas.

Os cenários seguintes utilizam FPGA na arquitetura de projeto Brainwave:

- A atribuição de sistema de inspeção optical. Consulte [AI em tempo real: Microsoft announces pré-visualização do projeto Brainwave](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Mapeamento de abrange do telefone. Consulte [como utilizar FPGAs para Learning profunda inferência para efetuar abrange telefone mapeamento de Terabytes de imagens com vista aérea](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Como implementar um serviço web para um FPGA?

O fluxo de alto nível para criar um serviço de reconhecimento de imagem no Azure utilizando ResNet50 como um featurizer é o seguinte:

1. ResNet50 já estiver implementado no Brainwave. Pode criar outros gráficos (entrada de data, classificação e assim sucessivamente) com TensorFlow e definir um pipeline (entrada -> featurize -> classificar) utilizando um ficheiro de json da definição de serviço. Comprimir a definição e gráficos para um ficheiro zip e carregar o ficheiro zip para o Blob storage do Azure.
2. Registe o modelo utilizando a API de gestão de modelo do Azure ML com o ficheiro zip no armazenamento de Blobs.
3. Implemente o serviço com o modelo registado utilizando a API de gestão de modelo do Azure ML.

Saiba mais sobre este processo no artigo, [implementar um modelo como um serviço web num FPGA com o Azure Machine Learning](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>Começar a utilizar FPGA

Azure Machine Learning Hardware acelerados modelos permitem-lhe implementar modelos de formação DNN FPGAs na nuvem do Azure. Para começar a utilizar, consulte:

- [Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning Hardware acelerados modelos com tecnologia do projeto Brainwave](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Passos Seguintes

[Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)

[Saiba como instalar o SDK FPGA](reference-fpga-package-overview.md)

[Hardware Hiperescala: ML à escala na parte superior do Azure + FPGA: 2018 de compilação (vídeo)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[Dentro do Microsoft cloud configurável baseado em FPGA (vídeo)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft unveils Brainwave de projeto para AI em tempo real](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)