---
title: Ciência de dados com a profunda Learning dados ciência de Máquina Virtual no Azure | Microsoft Docs
description: Como efetuar várias tarefas de ciência de dados comuns com a VM de ciência de dados de aprendizagem profunda.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 2053ed8cc420183d493097eeb2cd2ad93c82c70c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32167250"
---
# <a name="using-the-deep-learning-virtual-machine"></a>Utilização avançada de aprendizagem Máquina Virtual

Depois de ter aprovisionado a Máquina Virtual de aprendizagem profunda (DLVM), pode iniciar a criação de modelos de rede neuronal profundidade para criar aplicações de AI em domínios, como a visão do computador e a compreensão de idiomas. 

Existem várias ferramentas fornecido na VM Learning avançada para AI. O [página de estruturas de aprendizagem profunda e AI](dsvm-deep-learning-ai-frameworks.md) contém detalhes sobre como utilizar estas ferramentas. 

## <a name="deep-learning-tutorials-and-walkthroughs"></a>Learning profundo tutoriais e instruções

Para além das amostras com base em framework, um conjunto de instruções abrangentes também é fornecido que ter sido validado no DLVM. Estas instruções ajudam a iniciar o desenvolvimento de aplicações de aprendizagem profunda em domínios, como a imagem e a compreensão de idiomas de texto /. Tutoriais de ponto a ponto mais em domínios diferentes e tecnologia irão continuar a ser adicionado.   


- [A execução de redes neurais em estruturas diferentes](https://github.com/ilkarman/DeepLearningFrameworks): instruções abrangente que mostra como migrar o código de uma estrutura para outro. É também demonstra como comparar o modelo e execute o desempenho do tempo em estruturas. 

- [Um guia de procedimentos para criar uma solução ponto a ponto para detetar produtos dentro imagens](https://github.com/Azure/cortana-intelligence-product-detection-from-images): deteção de imagem é uma técnica que possam localizar e classificar objetos dentro de imagens. Esta tecnologia tem o potencial de colocar recompensas enormes em vários domínios de negócio de vida real. Por exemplo, retalhistas podem utilizar esta técnica para determinar o shelf tem captado que produto um cliente. Estas informações ajudam, por sua vez, arquivos de gerir o inventário de produtos. 

- [Com o nome de extração de entidade de PubMed abstracts](https://docs.microsoft.com/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition) este tutorial mostra como extrair as entidades com nome, como nomes de drug ou nomes de disease, a partir de texto não estruturado. Trains uma palavra personalizada incorporar no corpus texto de milhões de 18 PubMed abstracts, utiliza esse modelo para criar um modelo de rede neuronal periódica de memória de curto prazo longo (LSTM) para extração de entidade e mostra que a palavra específicas do domínio ao incorporar o modelo pode superam o desempenho de um word genérico incorporar para extração de entidade.

- [Ligação avançada de áudio aprendizagem](https://blogs.technet.microsoft.com/machinelearning/2018/01/30/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure/) este tutorial mostra como preparar um modelo de aprendizagem avançada para deteção de evento de áudio no [sons urbano conjunto de dados](https://serv.cusp.nyu.edu/projects/urbansounddataset/urbansound8k.html) e forneça uma descrição geral de como trabalhar com dados de áudio.

- [Classificação dos documentos de texto](https://github.com/anargyri/lstm_han): esta explicação passo a passo demonstra como criar e preparar duas arquiteturas de outra rede neuronal: rede hierárquica atenção rede e de tempo curto prazo memória (LSTM). As redes neurais utilizam a API de Keras para profunda de aprendizagem para classificar documentos de texto. Keras é um front-end para três a profunda mais popular estruturas de aprendizagem: Microsoft cognitivos Toolkit, TensorFlow e Theano.

## <a name="next-steps"></a>Passos Seguintes

O [página amostras](dsvm-samples-and-walkthroughs.md) fornece indicações para os exemplos de código previamente carregados na VM para cada um das estruturas para o ajudar a começar rapidamente. 
