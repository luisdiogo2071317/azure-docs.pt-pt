---
title: Predefinidas de extração de dados, a linguagem natural, a imagem, processamento de habilidades (Azure Search) | Documentos da Microsoft
description: Extração de dados, linguagem natural, capacidades cognitivas de processamento de imagens adicionar semântica e a estrutura a conteúdos não processados num pipeline do Azure procurar.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: c342eca8f27db713e139c187147abddd80eb854e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734539"
---
# <a name="predefined-skills-for-content-enrichment-azure-search"></a>Competências predefinidas para melhoria do conteúdo (Azure Search)

Neste artigo, ficará a conhecer as capacidades cognitivas fornecidas com o Azure Search. R *competências cognitivas* é uma operação que transforma o conteúdo de alguma forma. Muitas vezes, é um componente que extrai dados ou infere a estrutura e, portanto, aumenta a nossa compreensão dos dados de entrada. Quase sempre, a saída é baseado em texto. R *conjunto de capacidades* é o conjunto de habilidades que definem o pipeline de melhoria. 

> [!NOTE]
> A Pesquisa Cognitiva está em pré-visualização pública. Conjunto de capacidades execução e a extração de imagem e a normalização atualmente são oferecidos gratuitamente. Posteriormente, os preços para estas capacidades serão anunciado. 

## <a name="predefined-skills"></a>Competências predefinidas

Várias habilidades são flexíveis o que consomem ou produzir. Em geral, a maioria das habilidades são baseadas em modelos com formação prévia, o que significa que não é possível preparar o modelo de utilizar os seus próprios dados de treinamento. Para obter orientações sobre como criar uma habilidade personalizada, veja [como definir uma interface personalizada](cognitive-search-custom-skill-interface.md) e [exemplo: criar uma habilidade personalizada](cognitive-search-create-custom-skill-example.md). A tabela seguinte enumera e descreve as capacidades fornecidas pela Microsoft. 

| Habilidades | Descrição |
|-------|-------------|
| [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md) | Essa habilidade utiliza um modelo de pretrained para detetar as frases importantes com base na colocação de termo, regras lingüísticas, proximidade outros termos e é o termo como invulgar na origem de dados. |
| [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)  | Este utiliza de habilidade um modelo pretrained para detetar o idioma que é utilizada (um ID de idioma por documento). Quando vários idiomas forem utilizados dentro dos mesmo segmentos de texto, a saída é o LCID do idioma predominantemente utilizado.|
| [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md) | Consolida texto de uma coleção de campos num único campo.  |
| [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md) | Essa habilidade utiliza um modelo de pretrained para estabelecer entidades para um conjunto fixo de categorias: as pessoas, a localização, a organização. |
| [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)  | Essa habilidade utiliza um modelo de pretrained a pontuação de sentimento positivo ou negativo numa base de Registro por Registro. A pontuação é entre 0 e 1. As pontuações neutras ocorrerem caso nulo quando não é possível detetar o sentimento e para o texto que é considerado neutro.  |
| [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md) | Divide texto numa páginas para que possa melhorar ou aumentar o conteúdo de forma incremental. |
| [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md) | Essa habilidade usa um algoritmo de deteção de imagem para identificar o conteúdo de uma imagem e gerar uma descrição de texto. |
| [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md) | Reconhecimento ótico de carateres. |
| [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md) | Mapas de saída para um tipo complexo (com várias partes tipo de dados, que pode ser utilizado para um nome completo, um endereço com várias linha ou uma combinação de apelido e um identificador pessoal.) |

## <a name="see-also"></a>Consulte também

+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
+ [Definição de interface de competências personalizada](cognitive-search-custom-skill-interface.md)
+ [Tutorial: Enriquecida indexação com pesquisa cognitiva](cognitive-search-tutorial-blob.md)