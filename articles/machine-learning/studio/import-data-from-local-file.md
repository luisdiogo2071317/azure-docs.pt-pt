---
title: Importar dados de ficheiro
titleSuffix: Azure Machine Learning Studio
description: Saiba como carregar um ficheiro de dados de treinamento do disco rígido para o Azure Machine Learning Studio. Esta ação cria um módulo de conjunto de dados na área de trabalho.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: fe09eabe399af2d3ddc776843f39742774e6ea3c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55495892"
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-azure-machine-learning-studio"></a>Importar dados de treinamento de um ficheiro no disco rígido para o Azure Machine Learning Studio

Saiba como carregar um ficheiro de dados do disco rígido para utilizar como dados de formação no Azure Machine Learning Studio. Ao importar o ficheiro de dados, tiver um módulo de conjunto de dados pronto a utilizar na sua área de trabalho.

## <a name="steps-to-import-data-from-a-local-file"></a>Passos para importar dados de um ficheiro local
Para importar dados de uma unidade de disco rígida local, faça o seguinte:

1. Clique em **+ novo** na parte inferior da janela Machine Learning Studio.
2. Selecione **conjunto de dados** e **do ficheiro LOCAL**.
3. Na **carregar um novo conjunto de dados** caixa de diálogo, procure o ficheiro que pretende carregar
4. Introduza um nome, identificar o tipo de dados e, opcionalmente, introduza uma descrição. Recomenda-se uma descrição, ele permite que Registre quaisquer características sobre os dados que deseja lembrar ao utilizar os dados no futuro.
5. A caixa de verificação **esta é a nova versão de um conjunto de dados existente** permite-lhe atualizar um conjunto de dados existente com novos dados. Clique nesta caixa de verificação e, em seguida, introduza o nome de um conjunto de dados existente.

![Carregar um novo conjunto de dados](./media/import-data-from-local-file/upload-dataset.png)

Durante o carregamento, verá uma mensagem que o ficheiro está a ser carregado. Carregar tempo depende do tamanho dos seus dados e a velocidade da sua ligação ao serviço. Se souber que o ficheiro irá demorar muito tempo, pode fazer outras coisas dentro do Machine Learning Studio enquanto espera. No entanto, fechar o navegador faz com que o carregamento de dados efetuar a ativação.

## <a name="dataset-module-is-ready-for-use"></a>Módulo de conjunto de dados está pronto a utilizar
Depois de seus dados são carregados, ele é armazenado num módulo de conjunto de dados e está disponível para qualquer experimentação na sua área de trabalho.

Quando estiver a editar uma experimentação, pode encontrar os conjuntos de dados que criou no **conjuntos de dados de meu** lista sob a **conjuntos de dados guardado** lista na paleta do módulo. Pode arrastar e largar o conjunto de dados para a tela de experimentação, quando desejar usar o conjunto de dados para análise adicional e o machine learning.
