---
title: Consultar e dados do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Consulta de dados de Time Series Insights (pré-visualização) do Azure
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 00ef6eed23d1645320c28123d6670230cdd725c9
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964922"
---
# <a name="data-querying"></a>Consulta de dados

O Azure Time Series Insights (TSI) permite a consultas de dados em eventos e metadados armazenados no ambiente através de APIs de superfície públicas. Essas APIs também são usadas na [Explorador TSI](./time-series-insights-update-explorer.md).

Existem três principais categorias de API que estão disponíveis no Azure TSI:

* APIs de ambiente permitem consultar o ambiente do TSI em si, como a lista de ambientes chamador tem acesso a metadados de ambiente, etc.

* Série modelo consulta APIs (TSM-Q) para ativar criar, ler, atualizar e eliminar operações em metadados armazenados na parte do ambiente do modelo de série de tempo. Como instâncias, tipos, hierarquias, etc.

* Consulta de série de tempo (TSQ) APIs permitem a obtenção de dados de eventos à medida que ele é registrado no fornecedor de origem, ou pode efetuar operações de transformar, combinar e efetuar cálculos nos dados de séries de tempo.

O [linguagem de expressão de série de tempo](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) (TSX) é uma poderosa quarta, categoria. Utiliza modelos de série de tempo (TSM), para permitir a composição de computação avançada.

## <a name="azure-time-series-insights-core-apis"></a>APIs principais do Time Series Insights do Azure

Seguem-se as principais APIs suportamos.

![tsq][1]

### <a name="the-environment-apis"></a>As APIs de ambiente

Seguem-se as APIs de ambiente disponíveis:

* [Obter a API de ambiente](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): devolve a lista de ambientes de que o chamador está autorizado a aceder.
* [Obter a API de disponibilidade do ambiente](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): devolve a distribuição de contagem de eventos durante o período de tempo do evento `$ts`. Esta API ajuda a determinar se existem todos os eventos o carimbo de hora ao devolver a contagem de eventos, se existem.
* [Obter a API de esquema de eventos](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): devolve os metadados de esquema de eventos para um intervalo de pesquisa especificada. Esta API ajuda a obter todos os metadados/as propriedades disponíveis no esquema do intervalo de pesquisa especificada.

### <a name="time-series-model-query-tsm-q-apis"></a>APIs de modelo-Query (TSM-Q) de série de tempo

Seguem-se as APIs de consulta do modelo de série de tempo disponíveis:

* [Definições de API de modelo](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): permite a obter e aplicar um patch no tipo de padrão e o nome do modelo do ambiente.
* [Tipos de API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): permite CRUD em tipos de série de tempo e as respetivas variáveis associadas.
* [API de hierarquias](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): permite CRUD em hierarquias de série de tempo e seus caminhos de campo associados.
* [API de instâncias](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): permite CRUD em instâncias de série de tempo e dos respetivos campos de instância associada.

### <a name="the-time-series-query-tsq-apis"></a>As APIs de consulta (TSQ) da série de tempo

Seguem-se as APIs de consulta da série de tempo disponíveis:

* [Obter a API de eventos](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): A API de eventos de obter permite a consulta e obtenção de dados TSI dos eventos como eles são registados no Azure TSI do fornecedor de origem.

* [Obter a API de série](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): e obtenção de dados de Azure TSI dos eventos capturados ao tirar partido de dados registados na conexão com as variáveis de consulta ativa definem no modelo ou fornecido inline.

    >[!NOTE]
    > A cláusula de agregação é ignorada, mesmo que especificado num modelo ou fornecida inline.

  A API de série obter devolve um TSV (valor de série do tempo, um formato TSI utiliza para a saída JSON da consulta) para cada variável para cada intervalo, com base em fornecido **ID de série de tempo** e o conjunto de variáveis fornecidos.

* [API de série de agregar](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): consulta ativa e a obtenção de dados TSI dos eventos capturados por amostragem e agregar dados de registadas.

  A API de série agregado devolve um TSV para cada variável, para cada intervalo, com base em fornecido **ID de série de tempo** e o conjunto de variáveis fornecidos. A API de série agregado alcança redução ao tirar partido das variáveis armazenados no TSM ou fornecida inline para dados de agregação ou de exemplo.

  Tipos de agregação suportados: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Passos Seguintes

Leitura a [armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md).

Leia sobre [modelação de dados](./time-series-insights-update-tsm.md).

Leia sobre [de melhores práticas ao escolher um ID de série de tempo](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/tsq.png
