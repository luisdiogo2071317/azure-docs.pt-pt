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
ms.date: 05/01/2018
ms.author: HeidiSteen
ms.openlocfilehash: 3a9ff6697357dec443691b261ae6fc0477603a9c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>API de REST do serviço de pesquisa do Azure: Versão de 2017-11-11-Preview
Este artigo descreve o `api-version=2017-11-11-Preview` versão do serviço da Azure Search API REST, que fornece as seguintes funcionalidades experimental:

+ [Pesquisa cognitivos](cognitive-search-concept-intro.md), uma nova capacidade de sem causa no indexação da pesquisa do Azure, que localiza informações latentes em origens não sejam de texto e texto undifferentiated, transformá-lo para conteúdo pesquisável de texto completo na Azure Search.

A pré-visualização API é equivalente à API geralmente disponível com a exceção seguintes duas operações:

+ [Criar Skillset (api-version = 2017-11-11-pré-visualização)](ref-create-skillset.md)
+ [Criar indexador (api-version = 2017-11-11-pré-visualização)](ref-create-indexer.md)

Lembre-se de que a versão de pré-visualização API de destino `api-version=2017-11-11-Preview` ao avaliar as funcionalidades de pré-lançamento. A sintaxe de exemplo seguinte ilustra uma chamada para a versão de pré-visualização API.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

> [!NOTE]
> Funcionalidades de pré-visualização estão disponíveis para testar e experiência com o objetivo de recolha de comentários e estão sujeitos a alterações. **Aconselhamos vivamente contra a utilização de APIs de pré-visualização em aplicações de produção.**

Serviço de pesquisa do Azure está disponível em múltiplas versões. Consulte [versões de API](search-api-versions.md) para obter mais detalhes.
