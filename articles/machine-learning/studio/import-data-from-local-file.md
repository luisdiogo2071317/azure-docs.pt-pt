---
title: Importar dados de ficheiro - Azure Machine Learning Studio | Documentos da Microsoft
description: Saiba como carregar um ficheiro de dados de treinamento do disco rígido para o Azure Machine Learning Studio. Esta ação cria um módulo de conjunto de dados na área de trabalho.
keywords: Importar dados, formato de dados, os tipos de dados, origens de dados, dados de treinamento
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 77f08bd81b9b0cefc0bb956f38856bcb6d0ad572
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53093391"
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importar dados de treinamento de um ficheiro no disco rígido para o Machine Learning Studio

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
