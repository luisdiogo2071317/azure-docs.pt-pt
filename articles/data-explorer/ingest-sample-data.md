---
title: Ingerir dados de exemplo no Explorador de dados do Azure
description: Saiba mais sobre como ingerir dados de exemplo relacionados com Meteorologia (carga) no Explorador de dados do Azure.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 7eb0e48a5b66775ac97ed0cab751db0ef367f667
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964620"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Ingerir dados de exemplo no Explorador de dados do Azure

Este artigo mostra-lhe como ingerir dados de exemplo (carga) para uma base de dados do Explorador de dados do Azure. Existem [várias formas de ingerir dados](ingest-data-overview.md); este artigo se concentra numa abordagem básica, que é adequada para fins de teste.

> [!NOTE]
> Já tem esses dados se concluiu [início rápido: ingestão de dados usando a biblioteca de Python de Explorador de dados do Azure](python-ingest-data.md).

## <a name="prerequisites"></a>Pré-requisitos

[Um cluster de teste e a base de dados](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Ingerir dados

O **StormEvents** conjunto de dados de exemplo contém dados relacionados com Meteorologia a partir do [National centros de informações ambientais](https://www.ncdc.noaa.gov/stormevents/).

1. Inicie sessão no [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. No canto superior esquerdo da aplicação, selecione **cluster de adicionar**.

1. Na **adicionar cluster** caixa de diálogo, introduza o URL do seu cluster no formulário `https://<ClusterName>.<Region>.kusto.windows.net/`, em seguida, selecione **Add**.

1. Cole o seguinte comando e selecione **executar**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Depois de concluída a ingestão, cole a seguinte consulta, selecione a consulta na janela e selecione **executar**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    A consulta devolve os seguintes resultados dos dados de exemplo ingeridos.

    ![Resultados da consulta](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Passos Seguintes

[Escrever consultas](write-queries.md)

[Ingestão de dados do Azure Data Explorer](ingest-data-overview.md)