---
title: Serviço Azure Batch AI - Preparação de IA | Microsoft Docs
description: Saiba como utilizar o serviço Azure Batch AI gerido para preparar a inteligência artificial (IA) e outros modelos de machine learning em clusters de GPUs e CPUs.
services: batch-ai
documentationcenter: ''
author: alexsuttonms
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 08/01/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 37f1bf5dc20d097f7f5f560e3bf1fdd25dc38045
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408056"
---
# <a name="what-is-azure-batch-ai"></a>O que é o Azure Batch AI?

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

O Azure Batch AI é um serviço gerido para ajudar os cientistas de dados e investigadores de IA a preparar e testar modelos de aprendizagem automática e IA à escala no Azure, sem que seja preciso de gerir infraestruturas complexas. Descreva os recursos de computação, as tarefas que quer executar, onde armazenar as entradas e saídas de modelos e o Batch AI trata do resto.

Pode utilizar o Batch AI de forma autónoma ou para executar a preparação de modelos como parte de um fluxo de trabalho de desenvolvimento maior:

* Utilize o Batch AI para preparar, testar e classificar em lote os modelos de machine learning e IA em clusters de [GPUs](../virtual-machines/linux/sizes-gpu.md) ou CPUs. 

* Segmente um cluster do Batch AI num fluxo de trabalho com o [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) ou outras [ferramentas da plataforma Azure AI](https://azure.microsoft.com/overview/ai-platform/). O Azure ML proporciona uma experiência avançada de preparação de dados, experimentação e histórico de tarefas. O Azure ML também pode empacotar um modelo preparado num contentor e implementar um modelo para inferência ou em dispositivos IoT.  

## <a name="train-machine-learning-and-ai-models"></a>Preparar modelos de machine learning e IA

Utilize o Batch AI para preparar modelos de machine learning, bem como redes neurais profundas (aprendizagem aprofundada) e outras abordagens de IA. O Batch AI tem suporte incorporado para bibliotecas e arquiteturas de IA de código fonte aberto populares, incluindo o [TensorFlow](https://github.com/tensorflow/tensorflow), o [PyTorch](https://github.com/pytorch/pytorch) e o [Chainer](https://github.com/chainer/chainer), e [Microsoft Cognitive Toolkit (CNTK)](https://github.com/Microsoft/CNTK).

Depois de ter identificado a sua área de problemas e preparado os dados, trabalhe interativamente com o Batch AI para testar as ideias do modelo. Quando estiver pronto para experimentar em escala, inicie tarefas em vários GPUs com MPI ou outras bibliotecas de comunicação e execute mais experimentações em paralelo.

O Batch AI ajuda-o a preparar modelos em escala de várias formas. Por exemplo: 

|  |  |
|---------|---------|
| **Distribuir a preparação**<br/>![Preparação distribuída](./media/overview/distributed-training.png)  | Aumente verticalmente a preparação para uma única tarefa em vários GPUs ligados à rede para preparar redes maiores com grandes volumes de dados.|
| **Experimentação**<br/>![Experimentação](./media/overview/experimentation.png) | Aumente horizontalmente a preparação com várias tarefas. Execute varrimentos de parâmetros para testar novas ideias ou ajustar hiperparâmetros para otimizar a precisão e o desempenho. |
| **Executar em paralelo**![Execução paralela](./media/overview/parallel-execution.png) | Prepare ou classifique inúmeros modelos em simultâneo em execução em paralelo numa frota de servidores para concluir as tarefas com mais rapidez.|

Quando tiver preparado um modelo, utilize o Batch AI para testá-lo, caso não faça parte do script de preparação, ou efetue a classificação em lote.

## <a name="how-it-works"></a>Como funciona

Utilize os SDKs do Batch AI, scripts da linha de comandos ou o portal do Azure para gerir os recursos de computação e agendar tarefas para preparação e teste de IA: 

* **Aprovisionar e dimensionar clusters de VMs** – Escolha o número de nós (VMs) e selecione um tamanho ativado para GPU ou outro tamanho de VM que satisfaça as suas necessidades de preparação. Aumente ou reduza verticalmente o número de nós automática ou manualmente para utilizar os recursos apenas quando for necessário. 

* **Gerir dependências e contentores** – Por predefinição, os clusters do Batch AI executam imagens de VM do Linux que têm dependências pré-instaladas para executar arquiteturas de preparação baseadas em contentores em GPUs ou CPUs. Para configuração adicional, utilize imagens personalizadas ou execute scripts de arranque.

* **Distribuir dados** -escolha uma ou várias opções de armazenamento para gerir dados de entrada e scripts e o resultado da tarefa: Os ficheiros do Azure, armazenamento de Blobs do Azure ou um servidor NFS gerido. O Batch AI suporta também soluções de armazenamento personalizadas, incluindo sistemas de ficheiros paralelos de elevado desempenho. Monte sistemas de ficheiros de armazenamento nos nós de cluster e contentores de tarefas com ficheiros de configuração simples.

* **Agendar tarefas** – Submeta as tarefas para uma fila de tarefas com base na prioridade para partilhar os recursos de cluster e tirar partido de VMs de baixa prioridade e instâncias reservadas.

* **Processar falhas** – Monitorize o estado das tarefas e reinicie tarefas em caso de falhas da VM durante tarefas de execução potencialmente longa.

* **Recolher resultados** – Aceda facilmente aos registos de saída, Stdout, Stderr e modelos preparados. Configure as tarefas do Batch AI para enviar a saída diretamente para o armazenamento montado.

Como serviço do Azure, o Batch AI suporta ferramentas comuns, como o controlo de acesso baseado em funções (RBAC) e redes virtuais do Azure para segurança.  

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre os [recursos do Batch AI](resource-concepts.md) para preparar um modelo de machine learning ou IA.

* Comece a [preparar um modelo de aprendizagem aprofundada de exemplo](quickstart-tensorflow-training-cli.md) com o Batch AI.

* Veja [receitas de preparação](https://github.com/Azure/BatchAI/blob/master/recipes) de exemplo para arquiteturas de IA populares.
