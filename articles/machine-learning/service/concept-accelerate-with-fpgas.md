---
title: O que é um FPGA e Project Brainwave?
titleSuffix: Azure Machine Learning service
description: Saiba como acelerar as redes neurais profundas com FPGAs e modelos no Azure. Este artigo fornece uma introdução sobre matrizes de porta de campos programáveis (FPGA) e como o serviço do Azure Machine Learning fornece em tempo real de inteligência artificial (IA) ao implementar o seu modelo para um FPGA do Azure.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 10/24/2018
ms.custom: seodec18
ms.openlocfilehash: b55d641f9a72fc2f757a7687b1b6214a98c92507
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094003"
---
# <a name="what-is-fpga-and-project-brainwave"></a>O que é FPGA e Project Brainwave?

Este artigo fornece uma introdução sobre matrizes de porta de campos programáveis (FPGA) e como o serviço Azure Machine Learning fornece em tempo real de inteligência artificial (IA) ao implementar o seu modelo para um FPGA do Azure.

FPGAs contêm uma matriz de blocos de lógica programável e uma hierarquia de reconfiguráveis interconexões. O interconexões permitir que esses blocos de ser configurada em pós-fabricação e várias formas. FPGAs oferecem uma combinação de programação e desempenho em comparação comparada outros chips.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs vs. CPU, GPU e ASIC

![Comparação FPGA de serviço do Azure Machine Learning](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Processador||Descrição|
|---|:-------:|------|
|Circuitos do integrado específicas da aplicação|ASICs|Circuitos personalizados, como o da Google TensorFlow processador unidades (TPU), fornecem a mais alta eficiência. Não pode ser reconfigurados conforme suas necessidades mudam.|
|Matrizes de porta de campos programáveis|FPGAs|FPGAs, como as disponíveis no Azure, apresentam um desempenho quase ASICs, mas são flexíveis e reconfiguráveis ao longo do tempo para implementar a lógica de novo.|
|Unidades de processamento de gráficos|GPUs|Uma opção popular para computações de IA, oferecendo capacidades de processamento paralelo, tornando-o mais rápido na composição de imagem que CPUs.|
|Unidades de processamento central|CPUs|Processadores para fins gerais, cujo desempenho não é ideal para gráficos e processamento de vídeo.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave no Azure

[Project Brainwave](https://www.microsoft.com/research/project/project-brainwave/) é arquitetura de hardware económicas da Microsoft, com base em dispositivos FPGA da Intel, que os programadores e cientistas de dados usam para acelerar os cálculos de IA em tempo real.  Esta arquitetura habilitados em FPGA oferece **desempenho**, **flexibilidade**, e **dimensionamento** e está disponível no Azure.

**FPGAs tornam possível obter a baixa latência para pedidos de inferência em tempo real.** Solicitações assíncronas (batch) não são necessários. Processamento em lote pode causar uma latência porque precisam de mais dados a ser processada, mas ele pode melhorar o débito em alguns contextos. Project Brainwave implementações de unidades de processamento neural não exigem a criação de batches; sendo assim a latência pode ser muitas vezes mais baixo em comparação comparadas a CPU e GPU.

### <a name="reconfigurable-power"></a>Power reconfiguráveis
**FPGAs podem ser reconfigurados para diferentes tipos de modelos de aprendizagem automática.** Esta flexibilidade torna mais fácil acelerar as aplicações com a mais precisão numérica ideal e o modelo de memória que está a ser utilizado.

Novas técnicas de aprendizado de máquina estão a ser desenvolvidas em intervalos regulares e design de hardware do Project Brainwave também está a evoluir rapidamente. Uma vez que FPGAs são reconfiguráveis, é possível manter-se atualizado com os requisitos dos algoritmos de IA mudanças rapidamente.

### <a name="whats-supported-on-azure"></a>O que é suportado no Azure
**O Microsoft Azure é o investimento de maior na cloud do mundo em FPGAs.** Pode executar Project Brainwave na infraestrutura de dimensionamento do Azure.

Hoje em dia, Project Brainwave suporta:
+ Cenários de classificação e reconhecimento de imagem
+ Implementação do TensorFlow
+ DNNs: Utilizar o ResNet 50, utilizar o ResNet 152, VGG-16, SSD VGG e DenseNet 121
+ Hardware da Intel FPGA 

Com esta arquitetura de hardware habilitados em FPGA, redes neurais treinadas executam rapidamente e com uma latência mais baixa. Project Brainwave pode executar em paralelo com formação prévia redes neurais profundas (DNN) em FPGAs para aumentar horizontalmente o seu serviço. Os DNNs podem ser previamente treinados, como um featurizer profunda para a transferência de aprendizado ou otimizar com pesos atualizados.

### <a name="scenarios-and-applications"></a>Cenários e aplicações

Project Brainwave está integrado com o Azure Machine Learning. Microsoft usa FPGAs de avaliação de DNN, classificação de pesquisa do Bing e software definidas redes aceleração de (SDN) para reduzir a latência, liberando CPUs para outras tarefas.

Os seguintes cenários utilizam FPGA na arquitetura do Project Brainwave:
+ [Automatizada do sistema de inspeção óptica](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Mapeamento de capa de terra](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Implementar para FPGAs no Azure

Eis o fluxo de trabalho para criar um serviço de reconhecimento de imagem no Azure utilizando DNNs suportados como um featurizer para implementação no Azure FPGAs:

1. Utilize o [do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk) para criar uma definição de serviço, que é um ficheiro que descreve um pipeline de gráficos (entrada, featurizer e classificador) com base em TensorFlow. O comando de implementação automaticamente comprimir a definição e gráficos para um ficheiro ZIP e carregue o ZIP para o armazenamento de Blobs do Azure.  O DNN já estiver implementado no Project Brainwave para ser executado no FPGA.

1. Registe o modelo com o SDK com o arquivo ZIP no armazenamento de Blobs do Azure.

1. Implemente o serviço com o modelo registado com o SDK.

Pode começar a implementar modelos DNN ensinados para FPGAs no Azure na cloud com este artigo **"[implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)"**.


## <a name="next-steps"></a>Passos Seguintes

Veja estes vídeos e blogs:

+ [Hardware de Hiperescala: ML em escala com base no Azure + FPGA: Build 2018 (vídeo)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [Dentro do Microsoft cloud configurável baseada em FPGA (vídeo)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave para IA em tempo real: home page do projeto](https://www.microsoft.com/research/project/project-brainwave/)
