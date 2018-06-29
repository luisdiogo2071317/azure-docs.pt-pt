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
ms.openlocfilehash: 2e000628ebec393147328125a04d5d6f55afd3c6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100492"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Versão da api de REST do serviço de pesquisa do Azure de 2017-11-11-Preview
Este artigo descreve o `api-version=2017-11-11-Preview` versão do serviço da Azure Search API REST, oferta experimental funcionalidades ainda não geralmente está disponíveis.

> [!NOTE]
> Funcionalidades de pré-visualização estão disponíveis para testar e experiência com o objetivo de recolha de comentários e estão sujeitos a alterações. Aconselhamos vivamente contra a utilização de APIs de pré-visualização em aplicações de produção.


## <a name="new-in-2017-11-11-preview"></a>Novo no 2017-11-11-Preview

[**A conclusão automática** ](search-autocomplete-tutorial.md) associa existente [sugestões API](https://docs.microsoft.com/rest/api/searchservice/suggestions) adicionar experiências complementares antecipada para a barra de pesquisa. A conclusão automática devolve candidato termos de consulta, um utilizador pode escolher como a cadeia de consulta para uma pesquisa subsequente. Sugestões devolve documentos reais em resposta às entradas parciais: os resultados da pesquisa são imediatos e alterar dinamicamente à medida que aumenta a entrada do termo de pesquisa no comprimento e a especificidade.

[**Pesquisa cognitivos**](cognitive-search-concept-intro.md), uma nova capacidade de sem causa na Azure Search localiza informações latentes em origens não sejam de texto e texto undifferentiated, transformá-lo para conteúdo pesquisável de texto completo na Azure Search. Os seguintes recursos são introduzidos ou modificados na pré-visualização REST API. Todas as outras APIs REST são os mesmos se chamar geralmente disponível ou a versão de pré-visualização.

+ [Skillset operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [Criar indexador (api-version = 2017-11-11-pré-visualização)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Competências predefinidas](cognitive-search-predefined-skills.md)

Todas as outras APIs REST são os mesmos, independentemente de como definir a api-version. Por exemplo, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` e `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (sem `Preview`) são funcionalmente equivalente.

## <a name="other-preview-features"></a>Outras funcionalidades de pré-visualização

Funcionalidades anunciadas nas pré-visualizações anteriores ainda estão em pré-visualização pública. Se estiver a chamar uma API com uma api-version anterior pré-visualização, pode continuar a utilizar essa versão ou mudar para `2017-11-11-Preview` sem alterações ao comportamento esperado.

+ [Ficheiros CSV em indexação de Blobs do Azure](search-howto-index-csv-blobs.md), introduzida nos `api-version=2015-02-28-Preview`, permanece uma funcionalidade de pré-visualização. Esta funcionalidade é a parte de indexação de Blobs do Azure e é invocada através de uma definição de parâmetro. Cada linha num ficheiro CSV é indexada como um documento separado.

+ [As matrizes JSON no indexação de Blobs do Azure](search-howto-index-json-blobs.md), introduzida nos `api-version=2015-02-28-Preview`, permanece uma funcionalidade de pré-visualização. Esta funcionalidade é a parte de indexação de Blobs do Azure e é invocada através de uma definição de parâmetro. em que cada elemento da matriz está indexado como um documento separado.

+ [parâmetro de consulta moreLikeThis](search-more-like-this.md) localiza os documentos que são relevantes para um documento específico. Esta funcionalidade tem sido pré-visualizações anteriores. 


## <a name="how-to-call-a-preview-api"></a>Como chamar uma API de pré-visualização

Pré-visualizações anteriores estão operacionais ainda mas ficam obsoletas ao longo do tempo. Se o seu código chama `api-version=2016-09-01-Preview` ou `api-version=2015-02-28-Preview`, essas chamadas ainda são válidas. No entanto, apenas a versão de pré-visualização mais recente é atualizada com as melhorias. 

A sintaxe de exemplo seguinte ilustra uma chamada para a versão de pré-visualização API.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Serviço de pesquisa do Azure está disponível em múltiplas versões. Para obter mais informações, consulte [versões de API](search-api-versions.md).
