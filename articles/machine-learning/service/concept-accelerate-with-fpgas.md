---
title: O que é um FPGA? – Project Brainwave – do Azure Machine Learning
description: Saiba como acelerar as redes neurais profundas com FPGAs e modelos.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/31/2018
ms.openlocfilehash: 5c8efcbb5f2e9014c9edabfc2aee37c8c29ea0b4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646773"
---
# <a name="what-is-fpga-and-project-brainwave"></a>O que é FPGA e Project Brainwave?

Este artigo fornece uma introdução sobre matrizes de porta de campos programáveis (FPGA) e como FPGA se integra com o Azure machine learning para fornecer a IA em tempo real.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs vs. CPU, GPU e ASIC

FPGAs contêm uma matriz de blocos de lógica programável e uma hierarquia de reconfiguráveis interconexões que permitem que os blocos de ser configurados de maneiras diferentes depois de fabrico.

FPGAs fornecem uma combinação de programação e comparar o desempenho para outros chips:

![Comparação de Machine Learning FPGA do Azure](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

- **Unidades de processamento central (CPU)** são processadores para fins gerais. Desempenho de CPU não é ideal para gráficos e processamento de vídeo.
- **Gráficos (GPU) de unidades de processamento** oferecem o processamento paralelo e são uma opção popular para computações de IA. O processamento paralelo com o resultado de GPUs numa composição de imagem mais rápida do que CPUs.
- **Específico da aplicação integrada circuitos (ASIC)**, como unidades de processador da Google TensorFlow, são personalizados de circuitos. Embora estes chips de fornecem a mais alta eficiência, ASICs são inflexíveis.
- **FPGAs**, como as disponíveis no Azure, fornecer o desempenho próximo ASIC, mas oferece a flexibilidade de ser reconfiguradas mais tarde.

FPGAs estão agora em cada novo servidor do Azure. Microsoft já está a utilizar FPGAs para avaliação (DNN) de rede neural profunda classificação de pesquisa do Bing e aceleração de rede (SDN) definidas por software. Essas implementações de FPGA reduzem a latência, liberando CPUs para outras tarefas.

## <a name="project-brainwave-on-azure"></a>Project Brainwave no Azure

Project Brainwave é uma arquitetura de hardware que foi concebida com base em dispositivos FPGA da Intel e usados para acelerar os cálculos de IA em tempo real. Com essa arquitetura habilitados em FPGA económica, uma rede neural treinada pode ser executada mais rápido possível e com menor latência. Project Brainwave pode executar em paralelo com formação prévia DNNs em FPGAs para aumentar horizontalmente o seu serviço.

- Desempenho

    FPGAs tornam possível obter a baixa latência para pedidos de inferência em tempo real. Processamento em lote, significa dividir um pedido em partes mais pequenas e manutenção-los para um processador para melhorar a utilização de hardware. Criação de batches não é eficiente e pode causar uma latência. Brainwave não requer a criação de batches; Por conseguinte, a latência é 10 vezes mais baixo em comparação comparadas a CPU e GPU.

- Flexibilidade

    FPGAs podem ser reconfigurados para diferentes tipos de modelos de aprendizagem automática. Esta flexibilidade torna mais fácil acelerar as aplicações com a mais precisão numérica ideal e o modelo de memória que está a ser utilizado.

    Novas técnicas de aprendizado de máquina estão a ser desenvolvidas em intervalos regulares e design de hardware do Project Brainwave também está a evoluir rapidamente. Uma vez que FPGAs são reconfiguráveis, é possível manter-se atualizado com os requisitos dos algoritmos de IA mudanças rapidamente.

- Escala

    O Microsoft Azure é o investimento de maior na cloud do mundo em FPGAs. Pode executar Brainwave na infraestrutura de dimensionamento do Azure.

Uma pré-visualização do Project Brainwave integrado com o Azure Machine Learning está atualmente disponível. E uma pré-visualização limitada também está disponível para trazer Project Brainwave no Edge, para que pode aproveitar esse velocidade da computação em seus próprios recursos e as empresas.

Em pré-visualização atual, Brainwave é limitado para a implementação do TensorFlow e redes neurais ResNet50 com base no hardware Intel FPGA para classificação de imagens e reconhecimento. Existem planos para oferecer suporte a mais de Galeria de modelos e outras estruturas.

Os seguintes cenários utilizam FPGA na arquitetura do Project Brainwave:

- Automatizada do sistema de inspeção óptica. Ver [IA em tempo real: a Microsoft anuncia a pré-visualização do Project Brainwave](https://blogs.microsoft.com/ai/build-2018-project-brainwave/).
- Mapeamento de capa da terra. Ver [como utilizar FPGAs para inferência de tipos de aprendizagem profunda para realizar a capa de ' s Land mapeamento em Terabytes de imagens aéreas](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/).

## <a name="how-to-deploy-a-web-service-to-an-fpga"></a>Como implementar um serviço web num FPGA?

O fluxo de alto nível para criar um serviço de reconhecimento de imagem no Azure utilizando ResNet50 como um featurizer é o seguinte:

1. ResNet50 já estiver implementado no Brainwave. Pode criar outros gráficos (entrada de data, classificação e assim por diante) com o TensorFlow e defina um pipeline (entrada -> caracterização -> classificar) através de um ficheiro de json da definição de serviço. Comprimir a definição e gráficos para um ficheiro zip e carregue o ficheiro zip para o armazenamento de Blobs do Azure.
2. Registre-se do modelo a utilizar a API de gestão de modelo do Azure ML com o arquivo zip no armazenamento de Blobs.
3. Implemente o serviço com o modelo registado com a API de gestão de modelo do Azure ML.

Saiba mais sobre este processo no artigo [implementar um modelo como um serviço web num FPGA com o Azure Machine Learning](how-to-deploy-fpga-web-service.md).


## <a name="start-using-fpga"></a>Começar a utilizar FPGA

Azure Hardware aceleradas modelos de Machine Learning permitem-lhe implementar modelos DNN qualificados para FPGAs na cloud do Azure. Para começar a utilizar, veja:

- [Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)
- [Microsoft Azure Machine Learning Hardware aceleradas modelos com tecnologia do Project Brainwave](https://github.com/azure/aml-real-time-ai).

## <a name="next-steps"></a>Passos Seguintes

[Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)

[Saiba como instalar o SDK de FPGA](reference-fpga-package-overview.md)

[Hardware de Hiperescala: ML em escala com base no Azure + FPGA: Build 2018 (vídeo)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

[Dentro do Microsoft cloud configurável baseada em FPGA (vídeo)](https://channel9.msdn.com/Events/Build/2017/B8063)

[Microsoft divulgou Project Brainwave para IA em tempo real](https://www.microsoft.com/research/blog/microsoft-unveils-project-brainwave/)