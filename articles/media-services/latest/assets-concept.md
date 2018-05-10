---
title: Ativos nos serviços de multimédia do Azure | Microsoft Docs
description: Este artigo fornece uma explicação de quais são os recursos e como são utilizados pelos serviços de suporte de dados do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 928d88d51503350abe7df847ce58ff066b987c8e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="assets"></a>Elementos

Um **Asset** contém ficheiros digitais (incluindo as vídeo, áudio, imagens, coleções de miniaturas, controla de texto e legendas ficheiros) e os metadados sobre estes ficheiros. Depois dos ficheiros digitais são carregados para um elemento, pode ser utilizados nos serviços de suporte de dados de codificação e fluxos de trabalho de transmissão em fluxo.

Um recurso está mapeado para um contentor do blob no [conta de armazenamento do Azure](storage-account-concept.md) e os ficheiros no elemento são armazenados como blobs de blocos num contentor. Pode interagir com os ficheiros de recurso dos contentores utilizando os clientes do SDK de armazenamento.

Media Services do Azure suporta camadas de Blob quando a conta utiliza para fins gerais v2 (GPv2) de armazenamento. Com GPv2, pode mover o armazenamento de frio ou ficheiros para esporádico. Armazenamento de frio é adequado para arquivar os ficheiros de mezanino quando já não é necessário (por exemplo, depois de se tem sido codificados.)

Nos Media Services v3, a entrada da tarefa pode ser criada de recursos ou a partir do URL de HTTP (s). Para criar um recurso que pode ser utilizado como entrada para a sua tarefa, consulte [criar uma entrada de tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).

Além disso, leia sobre [contas de armazenamento nos serviços de suporte de dados](storage-account-concept.md) e [transformações e tarefas](transform-concept.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Um ficheiro de fluxo](stream-files-dotnet-quickstart.md)
