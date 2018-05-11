---
title: Processar e extraia o texto a partir de imagens na Azure Search | Microsoft Docs
description: Texto de processo e extrair e outras informações a partir de imagens na cognitivos pesquisa pipelines na Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: c58e731f6b8c86a0b7d6f2500d81077904b2f5ef
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
#  <a name="how-to-process-and-extract-information-from-images-in-cognitive-search-scenarios"></a>Como processar e extrair informações a partir de imagens em cenários de pesquisa cognitivos

Pesquisa cognitivos tem várias capacidades para trabalhar com imagens e os ficheiros de imagem. Durante a cracking do documento, pode utilizar o *imageAction* parâmetro, a extrair texto de fotografias ou imagens que contém texto alfanumérico, tais como a palavra "Parar de" num sinal de paragem. Outros cenários incluem gerar uma representação de texto de uma imagem, tais como "dandelion" para uma fotografia de um dandelion ou a cor "amarela". Também pode extrair metadados sobre a imagem, tais como o respetivo tamanho.

Este artigo abrange a imagem de processamento mais detalhadamente e fornece orientações para trabalhar com imagens num pipeline pesquisa cognitivos.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Obter imagens normalizadas

Como parte do cracking do documento, existem um novo conjunto de parâmetros de configuração de indexador para processamento de ficheiros de imagem ou nas imagens incorporadas ficheiros. Estes parâmetros são utilizados para normalizar imagens para obter mais processamento a jusante. Normalizing imagens, torna-os mais uniforme. Imagens grandes são redimensionadas para uma altura máxima e largura para torná-las consumíveis. Para fornecer os metadados no orientação de imagens, rotação de imagem é ajustada para carregamento vertical. Ajustes de metadados são capturados num tipo complexo criado para cada imagem. 

Não é possível desativar a normalização de imagem. Competências iterar imagens esperam imagens normalizadas.

| Parâmetro de configuração | Descrição |
|--------------------|-------------|
| imageAction   | Definido como "none" se deve ser efetuada nenhuma ação quando forem encontrados imagens incorporadas ou ficheiros de imagem. <br/>Definido como "generateNormalizedImages" para gerar uma matriz de imagens normalizadas como parte do documento cracking. Estas imagens ficará expostas no *normalized_images* campo. <br/>A predefinição é "none". Esta configuração apenas é relevante para as origens de dados do blob quando "dataToExtract" está definido como "contentAndMetadata." |
|  normalizedImageMaxWidth | A largura máxima (em pixéis) para imagens normalizadas gerado. A predefinição é 2000.|
|  normalizedImageMaxHeight | A altura máxima (em pixéis) para imagens normalizadas gerado. A predefinição é 2000.|

> [!NOTE]
> Se definir o *imageAction* propriedade para qualquer coisa que não seja "none", não será possível definir o *parsingMode* propriedade para qualquer coisa que não seja "default".  Só pode definir uma destas duas propriedades para um valor não predefinido na sua configuração de indexador.

A predefinição de 2000 pixels para a altura e largura máxima de imagens normalizado baseia-se nos tamanhos máximos suportados pelo [OCR skill](cognitive-search-skill-ocr.md) e [imagem analysis skill](cognitive-search-skill-image-analysis.md). Se aumentar os limites máximos, processamento poderá falhar nas imagens de maior.


Especificar o imageAction no seu [definição indexador](ref-create-indexer.md) da seguinte forma:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

Quando o *imageAction* está definido como "generateNormalizedImages", a nova *normalized_images* campo irá conter uma matriz de imagens. Cada imagem é um tipo complexo que tem os seguintes membros:

| Membro de imagem       | Descrição                             |
|--------------------|-----------------------------------------|
| dados               | Cadeia da imagem no formato JPEG normalizada de codificada em Base64.   |
| Largura              | Largura da imagem normalizada em pixéis. |
| Altura             | Altura da imagem normalizada em pixéis. |
| originalWidth      | A largura original da imagem antes de normalização. |
| originalHeight      | A altura original da imagem antes de normalização. |
| rotationFromOriginal |  Rotação Counter-no sentido horário em graus que ocorreram ao criar a imagem normalizada. Um valor entre 0 graus e 360 graus. Este passo lê os metadados da imagem que é gerada por uma câmara ou scanner. Normalmente, um múltiplo de 90 graus. |
| contentOffset |O desvio de carácter dentro do campo conteúdo extraída onde a imagem da. Este campo só é aplicável para os ficheiros com imagens incorporadas. |

 Valor de exemplo *normalized_images*:
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500  
  }
]
```

## <a name="image-related-skills"></a>Competências relacionados com a imagem

Existem dois competências cognitivos incorporadas que tomar imagens como entrada: [OCR](cognitive-search-skill-ocr.md) e [imagem Analysis](cognitive-search-skill-image-analysis.md). 

Atualmente, estes competências apenas a trabalhar com imagens geradas a partir do passo cracking do documento. Como tal, é a entrada suportada apenas `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Imagem skill de análise

O [skill de análise de imagem](cognitive-search-skill-image-analysis.md) extrai um conjunto avançado de funcionalidades visual com base no conteúdo de imagem. Por exemplo, pode gerar uma legenda a partir de uma imagem, gerar etiquetas ou identificar celebridades para e landmarks.

### <a name="ocr-skill"></a>OCR skill

O [OCR skill](cognitive-search-skill-ocr.md) extrai o texto de ficheiros de imagem como JPGs, PNGs e mapas de bits. Pode extrair texto, bem como informações de esquema. As informações de esquema fornecem caixas delimitador para cada uma das cadeias identificadas.

O skill OCR permite-lhe selecionar o algoritmo a utilizar para detetar o texto nas suas imagens. Atualmente suporta dois algoritmos, uma para texto impressos e outro para texto handwritten.

## <a name="embedded-image-scenario"></a>Cenário de imagem incorporada

Um cenário comum implica a criação de uma cadeia único que contém todos os conteúdos do ficheiro, o texto e o texto de origem da imagem, efetuando os seguintes passos:  

1. [Extrair normalized_images](#get-normalized-images)
1. Execute o através de skill OCR `"/document/normalized_images"` como entrada
1. Intercale a representação de texto dessas imagens com o texto não processado extraído a partir do ficheiro. Pode utilizar o [texto intercalar](cognitive-search-skill-textmerger.md) skill para consolidar ambos os segmentos de texto numa cadeia de grandes dimensões única.

Skillset de exemplo seguinte cria um *merged_text* campo que contém o conteúdo textual do seu documento. Também inclui o texto de OCRed de cada uma das imagens incorporadas. 

#### <a name="request-body-syntax"></a>Sintaxe de corpo do pedido
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```

Agora que tem um campo merged_text, foi possível mapeá-lo como um campo pesquisável a definição do indexador. Todo o conteúdo dos ficheiros, incluindo o texto de imagens, será pesquisáveis.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Visualizar delimitadora caixas de texto extraído

Outro cenário comum é visualizar informações de esquema de resultados de pesquisa. Por exemplo, pode querer realce onde foi encontrado um fragmento de texto numa imagem como parte dos resultados da pesquisa.

Uma vez que o passo de OCR é efetuado nas imagens normalizadas, são as coordenadas de esquema no espaço de imagem normalizado. Quando se apresenta a imagem normalizada, a presença de coordenadas, geralmente, não é um problema, mas em algumas situações poderá apresentar a imagem original. Converta cada um dos pontos coordenados no esquema neste caso, o sistema de coordenadas de imagem original. 

Como um programa auxiliar, se precisar de transformação normalizadas coordenadas para o espaço de coordenada original, pode utilizar o seguinte algoritmo:

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="see-also"></a>Consulte também
+ [Criar indexador (REST)](ref-create-indexer.md)
+ [Analisar skill de imagem](cognitive-search-skill-image-analysis.md)
+ [OCR skill](cognitive-search-skill-ocr.md)
+ [Skill de intercalação de texto](cognitive-search-skill-textmerger.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Como mapear os campos avançados](cognitive-search-output-field-mapping.md)
