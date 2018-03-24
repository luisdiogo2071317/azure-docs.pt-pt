---
title: Importar dados a partir de um ficheiro para o Azure Machine Learning Studio | Microsoft Docs
description: Saiba como carregar um ficheiro de dados de formação do disco rígido para o Azure Machine Learning Studio. Esta ação cria um módulo de conjunto de dados na área de trabalho.
keywords: Importar dados, o formato de dados, os tipos de dados, as origens de dados, dados de formação
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: f7096ee9e887f13e5bff1f79b7e81702061b8dea
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importar dados de formação de um ficheiro no disco rígido no Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Saiba como carregar um ficheiro de dados do disco rígido para utilizar como dados de formação no Azure Machine Learning Studio. Ao importar o ficheiro de dados, tiver um módulo de conjunto de dados pronto a utilizar na sua área de trabalho.

## <a name="steps-to-import-data-from-a-local-file"></a>Passos para importar dados a partir de um ficheiro local
Para importar dados a partir de uma unidade de disco rígida local, faça o seguinte:

1. Clique em **+ novo** na parte inferior da janela Machine Learning Studio.
2. Selecione **DATASET** e **partir do ficheiro LOCAL**.
3. No **carregue um novo conjunto de dados** caixa de diálogo, procure o ficheiro que pretende carregar
4. Introduza um nome, identifique o tipo de dados e, opcionalmente, introduza uma descrição. Recomenda-se uma descrição - permite-lhe registar as caraterísticas sobre os dados que pretende não se esqueça ao utilizar os dados no futuro.
5. A caixa de verificação **esta é a nova versão de um conjunto de dados existente** permite-lhe atualizar um conjunto de dados existente com novos dados. Clique nesta caixa de verificação e, em seguida, introduza o nome de um conjunto de dados existente.

![Carregue um novo conjunto de dados](./media/import-data-from-local-file/upload-dataset.png)

Durante o carregamento, verá uma mensagem que o ficheiro está a ser carregado. Carregar tempo depende do tamanho dos seus dados e a velocidade da sua ligação ao serviço. Se souber que o ficheiro irá demorar muito tempo, pode efetuar outras ações no interior do Machine Learning Studio enquanto aguarde. No entanto, se fechar o browser faz com que o carregamento de dados falhar.

## <a name="dataset-module-is-ready-for-use"></a>Módulo de conjunto de dados está pronto a utilizar
Depois dos dados são carregados, é armazenado num módulo conjunto de dados e está disponível para qualquer experimentação na sua área de trabalho.

Quando estiver a editar uma experimentação, pode encontrar os conjuntos de dados que criou no **conjuntos de dados os meus** lista sob o **conjuntos de dados guardadas** lista na paleta do módulo. Pode arrastar e largar o conjunto de dados para a tela de experimentação quando pretender utilizar o conjunto de dados para análise adicional e a aprendizagem.
