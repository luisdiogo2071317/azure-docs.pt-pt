---
title: Recursos de documentação de pesquisa cognitivos (Azure Search) | Microsoft Docs
description: Uma lista dos artigos, tutoriais, amostras e blogue anotada mensagens cargas de trabalho relacionados para cognitivos pesquisa na Azure Search.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: a96b66f61b19d218c5708a0ce967e0033ba26627
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>Recursos de documentação para cargas de trabalho de pesquisa cognitivos

Cognitivos pesquisa, agora em pré-visualização pública, é uma nova camada de sem causa no indexação da pesquisa do Azure, que localiza informações latentes em origens não sejam de texto e texto undifferentiated, transformá-lo para conteúdo pesquisável de texto completo na Azure Search.

Os artigos seguintes estão a documentação completa para pesquisa cognitivos.

## <a name="getting-started"></a>Introdução
+ [O que é pesquisa cognitivos?](cognitive-search-concept-intro.md)
+ [Início rápido: Tente cognitivos pesquisa no portal do](cognitive-search-quickstart-blob.md)
+ [Tutorial: Saber a APIs de pesquisa cognitivos](cognitive-search-tutorial-blob.md)
+ [Exemplo: criar um skill personalizado](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Orientações de procedimentos
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Como fazer referência anotações de um skillset](cognitive-search-concept-annotations-syntax.md)
+ [Como mapear os campos para um índice](cognitive-search-output-field-mapping.md)
+ [Como processar e extrair informações a partir de imagens](cognitive-search-concept-image-scenarios.md)
+ [Como reconstruir um índice da Azure Search](search-howto-reindex.md)
+ [Como definir uma interface de competências personalizado](cognitive-search-custom-skill-interface.md)
+ [Sugestões de resolução de problemas](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referência

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [API de REST de pré-visualização](search-api-2017-11-11-preview.md)
  + [Criar Skillset (api-version = 2017-11-11-pré-visualização)](ref-create-skillset.md)
  + [Criar indexador (api-version = 2017-11-11-pré-visualização)](ref-create-indexer.md)

## <a name="see-also"></a>Consulte também

+ [API REST da Azure Search](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexadores na pesquisa do Azure](search-indexer-overview.md)
+ [O que é o Azure Search?](search-what-is-azure-search.md)