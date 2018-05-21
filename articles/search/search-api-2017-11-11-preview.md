---
title: A pesquisa do Azure serviço REST versão da API de 2017-11-11-pré-visualização | Microsoft Docs
description: Pré-visualização do Azure Search serviço REST versão da API de 2017-11-11-inclui funcionalidades experimental como sinónimos e moreLikeThis pesquisas.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/17/2018
ms.author: HeidiSteen
ms.openlocfilehash: afcc8ac31c45c1a43d3d759ef6f5a1cee8166c0a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>API de REST do serviço de pesquisa do Azure: Versão de 2017-11-11-Preview
Este artigo descreve o `api-version=2017-11-11-Preview` versão do serviço da Azure Search API REST, oferta experimental funcionalidades ainda não geralmente está disponíveis.

> [!NOTE]
> Funcionalidades de pré-visualização estão disponíveis para testar e experiência com o objetivo de recolha de comentários e estão sujeitos a alterações. Aconselhamos vivamente contra a utilização de APIs de pré-visualização em aplicações de produção.


## <a name="new-in-this-preview"></a>Novo nesta pré-visualização

+ [Pesquisa cognitivos](cognitive-search-concept-intro.md), uma nova capacidade de sem causa na Azure Search localiza informações latentes em origens não sejam de texto e texto undifferentiated, transformá-lo para conteúdo pesquisável de texto completo na Azure Search.

As seguintes duas operações são introduzidas ou modificadas na pré-visualização REST API. Todas as outras APIs REST são os mesmos se chamar geralmente disponível ou (por exemplo, a versão de pré-visualização

+ [Criar Skillset (api-version = 2017-11-11-pré-visualização)](ref-create-skillset.md)

+ [Criar indexador (api-version = 2017-11-11-pré-visualização)](ref-create-indexer.md)

Todas as outras APIs REST são os mesmos se chamar geralmente disponível ou a versão de pré-visualização. Por exemplo, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` e `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (sem `Preview`) são funcionalmente equivalente.

## <a name="other-preview-features"></a>Outras funcionalidades de pré-visualização

Funcionalidades de pré-visualizações anteriores ainda estão em pré-visualização pública.

+ [Ficheiros CSV em indexação de Blobs do Azure](search-howto-index-csv-blobs.md), onde cada linha num ficheiro CSV está indexada como um documento separado.

+ [As matrizes JSON no Blob do Azure indexação](search-howto-index-json-blobs.md), onde cada elemento da matriz está indexado como um documento separado.

+ [`moreLikeThis` parâmetro de consulta](search-more-like-this.md) localizar documentos que são relevantes para um documento específico. Esta funcionalidade tem sido pré-visualizações anteriores. Se estiver a chamar esta API com uma versão de api anterior, pode continuar a utilizar essa versão.


## <a name="how-to-call-a-preview-api"></a>Como chamar uma API de pré-visualização

Pré-visualizações anteriores estão operacionais ainda mas ficam obsoletas ao longo do tempo. Se o seu código chama `api-version=2016-09-01-Preview` ou `api-version=2015-02-25-Preview`, essas chamadas ainda são válidas. No entanto, apenas a versão de pré-visualização mais recente é atualizada com as melhorias. 

A sintaxe de exemplo seguinte ilustra uma chamada para a versão de pré-visualização API.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Serviço de pesquisa do Azure está disponível em múltiplas versões. Para obter mais informações, consulte [versões de API](search-api-versions.md).
