---
title: Sobre os recursos do Batch AI - Azure | Documentos da Microsoft
description: Descrição geral das áreas de trabalho, clusters, servidores de ficheiros, experimentações e tarefas no serviço Batch AI no Microsoft Azure.
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2018
ms.author: danlep
ms.openlocfilehash: 4a9e3529f9d68ecdc614ea69cffc6897891f4548
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057226"
---
# <a name="overview-of-resources-in-batch-ai"></a>Descrição geral dos recursos no Batch AI

Quando primeiro começa a utilizar o serviço Batch AI, vai querer compreender os recursos do Batch AI disponíveis. Como com outros serviços do Azure, criar recursos do Batch AI no Azure de um ou mais *grupos de recursos*. Criar um ou mais o Batch AI *áreas de trabalho* num grupo de recursos. Cada área de trabalho contém um misto de Batch AI *clusters*, *servidores de ficheiros*, e *experimentações*. Uma experimentação de Batch AI encapsula um grupo de *tarefas*.

A imagem seguinte mostra um exemplo de uma hierarquia de recursos para o Batch AI. 

![](./media/migrate-to-new-api/batch-ai-resource-hierarchy.png)

As secções seguintes entram em mais detalhes sobre os recursos do Batch AI.

## <a name="workspace"></a>Área de trabalho

Uma área de trabalho Batch AI é uma coleção de nível superior do restante dos recursos do Batch AI. Áreas de trabalho de ajudam a separar o trabalho que pertencem a diferentes grupos ou projetos. Por exemplo, poderá criar um desenvolvimento e uma área de trabalho de teste.

## <a name="cluster"></a>Cluster

Um cluster do Batch AI contém os recursos de computação para a execução de tarefas. Todos os nós num cluster têm o mesmo tamanho VM e a imagem do SO. O batch AI oferece muitas opções para criar clusters que são personalizadas para necessidades diferentes. Normalmente, configura um cluster diferente para cada categoria de poder necessário para concluir um projeto de processamento. Dimensionamento de que clusters de IA do Batch e reduza verticalmente com base no orçamento e a pedido. Para obter mais informações, consulte [trabalhar com clusters de IA do Batch](clusters.md).

## <a name="file-server"></a>Servidor de ficheiros

Opcionalmente, crie um servidor de ficheiros no Batch AI para armazenar dados, scripts, de treinamento e registos de saída. Um servidor de ficheiros do Batch AI é um NFS gerido de nó único, que podem ser montadas automaticamente em nós de cluster para fornecer uma localização de armazenamento centralmente acessível e fácil de tarefas. Na maioria dos casos, apenas um servidor de ficheiros é necessário numa área de trabalho e pode separar dados para os trabalhos de treinamento em diretórios diferentes. Se o NFS não é adequado para cargas de trabalho, o Batch AI suporta outras opções de armazenamento, incluindo [armazenamento do Azure](use-azure-storage.md) ou soluções personalizadas como um sistema de ficheiros Gluster ou Lustre.

## <a name="experiment"></a>Experimentação

Uma experimentação agrupa uma coleção de tarefas relacionadas que consultar e gerir em conjunto. Cada área de trabalho pode ter múltiplas experimentações, onde cada experimentação tenta resolver um problema específico.

## <a name="job"></a>Tarefa

Uma tarefa é uma única tarefa ou um script que precisa ser executado, por exemplo, para preparar um modelo de aprendizagem profunda. Cada tarefa executa um script específico num cluster na área de trabalho. (O script pode ser armazenado num servidor de ficheiros de IA do Batch ou outra solução de armazenamento.) Cada tarefa do Batch AI tem um tipo de estrutura associado à mesma: TensorFlow, Horovod, CNTK, Caffe, Caffe2, pyTorch, Chainer, MPI personalizadas ou personalizado. Para cada estrutura, o serviço Batch AI configura a infra-estrutura necessária e gere os processos de trabalho. Cada experimentação pode ter várias tarefas que são semelhantes, exceto por algumas alterações aos parâmetros diferentes.

## <a name="next-steps"></a>Passos Seguintes

* Executar a sua primeira [tarefa de preparação de IA do Batch](quickstart-tensorflow-training-cli.md).

* Veja [receitas de preparação](https://github.com/Azure/BatchAI/tree/master/recipes) exemplo para diferentes estruturas.