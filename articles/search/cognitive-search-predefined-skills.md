---
title: Predefinidas de extração de dados, a linguagem natural, a imagem processamento competências (Azure Search) | Microsoft Docs
description: Extração de dados, linguagem natural, imagem processamento competências cognitivos adicionar semântica e estrutura para o conteúdo não processado um pipeline do Azure Seach.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 870cf9629c7af8faee0ce5709199b64910b27ffb
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Competências predefinidas para conteúdo sem causa (pesquisa do Azure)

Neste artigo, pode saber mais sobre as competências cognitivos fornecidas com a pesquisa cognitivos. A *skill cognitivos* é uma operação que transforma os conteúdos de alguma forma. Muitas vezes, é um componente que extrai dados ou infere estrutura e, por conseguinte, amplia a nossa compreensão dos dados de entrada. Quase sempre, o resultado é baseado em texto. A *skillset* é coleção de competências que definem o pipeline sem causa. 

## <a name="predefined-skills"></a>Competências predefinidas

Vários competências são flexíveis que consumir ou produzir. Em geral, a maioria das competências baseiam-se em modelos de formação previamente, o que significa que não é possível preparar o modelo utilizando os seus próprios dados de formação. Para obter orientações sobre a criação de um skill personalizada, consulte [como definir uma interface personalizada](cognitive-search-custom-skill-interface.md) e [exemplo: criar um skill personalizado](cognitive-search-create-custom-skill-example.md). A tabela seguinte enumera e descreve as competências fornecidas pela Microsoft. 

| Skill | Descrição |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Este skill utiliza um modelo pretrained para detetar expressões importantes com base na colocação termo, linguístico regras, proximidade aos outros termos, e como invulgar o termo pertence a origem de dados. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Esta utiliza skill um modelo pretrained para detetar o idioma que é utilizada (um ID de idioma por documento). Quando são utilizados vários idiomas dentro dos mesmo segmentos de texto, o resultado é o LCID do idioma predominantemente utilizado.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Consolida texto de uma coleção de campos para um único campo.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Este skill utiliza um modelo de pretrained estabelecer entidades para um conjunto fixo de categorias: pessoas, a localização, a organização. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Este skill utiliza um modelo pretrained para pontuar sentimento positivo ou negativo numa base de registo ao registo. A classificação é entre 0 e 1. Pontuações independentes ocorrerem para caso nulo quando não é possível detetar o sentimento e para o texto que é considerado independente.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Divide texto numa páginas para que possa enriqueça ou aumentar progressivamente o conteúdo. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Este skill utiliza um algoritmo de deteção de imagem para identificar o conteúdo de uma imagem e gerar uma descrição de texto. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Optical caráter reconhecimento. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Mapas de saída para um tipo complexo (um com várias partes tipo de dados, que pode ser utilizado para um nome completo, um endereço com várias linha ou uma combinação de apelido e um identificador pessoal.) |

## <a name="see-also"></a>Consulte também

+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Definição de interface de competências personalizada](cognitive-search-custom-skill-interface.md)
+ [Tutorial: Enriquecidos indexação com a pesquisa cognitivos](cognitive-search-tutorial-blob.md)