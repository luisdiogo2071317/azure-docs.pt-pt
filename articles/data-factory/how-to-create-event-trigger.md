---
title: Criar acionadores baseada em eventos no Azure Data Factory | Microsoft Docs
description: Saiba como criar um acionador no Azure Data Factory que executa um pipeline em resposta a um evento.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: douglasl
ms.openlocfilehash: a9c15b239ee0bd0dde0b1f11691565b2676e3d07
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062126"
---
# <a name="create-a-trigger-that-runs-a-pipeline-in-response-to-an-event"></a>Criar um acionador que executa um pipeline em resposta a um evento

Este artigo descreve os acionadores baseada em eventos que podem criar seus pipelines de fábrica de dados.

Arquitetura condicionada por eventos (EDA) é um padrão de integração de dados comuns que envolve a produção, deteção e consumo e reação eventos. Cenários de integração de dados requerem, muitas vezes, os clientes do Data Factory acionar pipelines com base em eventos. Fábrica de dados está agora integrada [grelha de eventos do Azure](https://azure.microsoft.com/services/event-grid/), pipelines que lhe permite acionar um evento.

## <a name="data-factory-ui"></a>IU do Data Factory

### <a name="create-a-new-event-trigger"></a>Criar um novo acionador de eventos

Um evento de típico é a chegada de um ficheiro ou da eliminação de um ficheiro, na sua conta do Storage do Azure. Pode criar um acionador que responde a este evento no seu pipeline do Data Factory.

> [!NOTE]
> Esta integração suporta apenas a versão 2 contas do Storage (objetivo geral).

![Criar o novo acionador de eventos](media/how-to-create-event-trigger/event-based-trigger-image1.png)

### <a name="select-the-event-trigger-type"></a>Selecione o tipo de Acionador de evento

Assim que o ficheiro chega na sua localização de armazenamento e o blob correspondente é criado, este evento é acionado e executa o pipeline do Data Factory. Pode criar um acionador que responde a um evento de criação de blob, um evento de eliminação de BLOBs ou ambos os eventos, no seus pipelines de fábrica de dados.

![Tipo de accionador Selecione como eventos](media/how-to-create-event-trigger/event-based-trigger-image2.png)

### <a name="configure-the-event-trigger"></a>Configurar o acionador de eventos

Com o **caminho do Blob começa com** e **caminho do Blob termina com** propriedades, pode especificar os contentores, pastas e os nomes de BLOBs para os quais pretende receber eventos. Pode utilizar diversas padrões para ambos **caminho do Blob começa com** e **caminho do Blob termina com** propriedades, conforme ilustrado nos exemplos neste artigo. É necessária, pelo menos, uma destas propriedades.

![Configurar o acionador de eventos](media/how-to-create-event-trigger/event-based-trigger-image3.png)

## <a name="json-schema"></a>JSON schema

A tabela seguinte fornece uma descrição geral dos elementos de esquema que estão relacionados com acionadores baseada em eventos:

| **Elemento JSON** | **Descrição** | **Tipo** | **Valores permitidos** | **Necessário** |
| ---------------- | --------------- | -------- | ------------------ | ------------ |
| **scope** | O ID de recurso do Azure Resource Manager da conta de armazenamento. | Cadeia | ID do Gestor de recursos do Azure | Sim |
| **Eventos** | O tipo de eventos que fazer com que este acionador a acionar. | Array    | Microsoft.Storage.BlobCreated, Microsoft.Storage.BlobDeleted | Sim, qualquer combinação. |
| **blobPathBeginsWith** | O caminho do blob tem de começar com o padrão fornecido para o acionador a acionar. Por exemplo, '/ registos/blobs/Dezembro /' apenas serão acionados o acionador para os blobs na pasta Dezembro sob o contentor de registos. | Cadeia   | | Tem de ser fornecida, pelo menos, uma destas propriedades: blobPathBeginsWith, blobPathEndsWith. |
| **blobPathEndsWith** | O caminho do blob tem de terminar com o padrão fornecido para o acionador a acionar. Por exemplo, 'december/boxes.csv' apenas serão acionados o acionador para blobs com o nome caixas numa pasta Dezembro. | Cadeia   | | Tem de ser fornecida, pelo menos, uma destas propriedades: blobPathBeginsWith, blobPathEndsWith. |

## <a name="examples-of-event-based-triggers"></a>Exemplos de acionadores baseada em eventos

Esta secção fornece exemplos das definições de Acionador baseada em eventos.

-   **Caminho do blob começa com**('containername /') – recebe eventos para os BLOBs no contentor.
-   **Caminho do blob começa com**('/ blobs/containername/foldername') – recebe eventos para os blobs no contentor de containername e foldername pasta.
-   **Caminho do blob começa com**('/ containername/blobs/foldername/file.txt') – recebe eventos para um blob com o nome file.txt na pasta foldername no recipiente do containername.
-   **Caminho do blob termina com**('file.txt') – Receives eventos para um blob com o nome file.txt em qualquer caminho.
-   **Caminho do blob termina com**('/ containername/blobs/file.txt') – recebe eventos para um blob com o nome file.txt em containername do contentor.
-   **Caminho do blob termina com**('foldername/file.txt') – Receives eventos para um blob com o nome file.txt na pasta de foldername em qualquer contentor.

> [!NOTE]
> Tem de incluir o `/blobs/` segmento do caminho sempre especificar o contentor e pasta, contentor e pasta de ficheiros ou de contentores e, de ficheiros.

## <a name="using-blob-events-trigger-properties"></a>Utilizar as propriedades de Acionador de eventos de Blob

Quando um acionador de eventos do blob é acionado, disponibiliza duas variáveis para o pipeline: *folderPath* e *fileName*. Para aceder a estas variáveis, utilize o `@triggerBody().fileName` ou `@triggerBody().folderPath` expressões.

Por exemplo, considere um acionador configurado para accionar quando é criado um blob com `.csv` como o valor do `blobPathEndsWith`. Quando um ficheiro. csv é ignorado para a conta de armazenamento, o *folderPath* e *fileName* descrevem a localização do ficheiro. csv. Por exemplo, *folderPath* tem o valor `/containername/foldername/nestedfoldername` e *fileName* tem o valor `filename.csv`.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações detalhadas sobre acionadores, consulte [acionadores e da execução de Pipeline](concepts-pipeline-execution-triggers.md#triggers).
