---
title: Moderator conteúdo do Azure - moderação de interrupção de imagem | Microsoft Docs
description: Utilize a moderação de interrupção de imagem para moderada imagens inadequadas
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: c7cbc343c6e9113642d0ac79f4a4d60a404e8171
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355076"
---
# <a name="image-moderation"></a>Moderação de imagens

Utilize a moderação de interrupção do Moderator de conteúdo imagem auxiliada a uma máquina e [revisão humana ferramenta](Review-Tool-User-Guide/human-in-the-loop.md) para moderada de imagens para o conteúdo para adultos e racy. Análise de imagens para o conteúdo de texto e extrair esse texto e detetar faces. Pode corresponder a imagens de listas personalizadas e qualquer ação adicional.

## <a name="evaluating-for-adult-and-racy-content"></a>Avaliar o conteúdo para adultos e racy

O **Evaluate** operação devolve uma pontuação de confiança entre 0 e 1. Também devolve dados booleano igual a true ou false. Estes valores prever se a imagem contém conteúdo para adultos ou racy potencial. Quando chamar a API com a imagem (ficheiro ou URL), a resposta devolvida inclui as seguintes informações:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]

> - `isImageAdultClassified` representa a presença potencial de imagens que pode ser considerada sexually explícita ou para adultos em determinadas situações.
> - `isImageRacyClassified` representa a presença potencial de imagens que pode ser considerada sexually suggestive ou madura em determinadas situações.
> - As pontuações encontram-se entre 0 e 1. Quanto maior for a classificação, quanto maior for o modelo é a previsão que a categoria pode ser aplicável. Esta pré-visualização depende de um modelo de análises, em vez de resultados manualmente codificados. Recomendamos que teste com o seus próprios conteúdo para determinar a forma como cada categoria alinha os seus requisitos.
> - Os valores booleanos são VERDADEIRO ou FALSO, consoante o modelo de pontuação interno limiares. Os clientes devem avaliar se pretende utilizar este valor ou opte por utilizar limiares personalizados com base nas políticas os respetivos conteúdas.
>

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Detetar texto com reconhecimento de caráter Optical (OCR)

O **Optical caráter reconhecimento (OCR)** operação prevê a presença de conteúdo de texto numa imagem e extrai-lo para a moderação de interrupção de texto, entre outras utilizações. Pode especificar o idioma. Se não especificar uma linguagem, será assumida a deteção para inglês.

A resposta inclui as seguintes informações:
- O texto original.
- Os elementos de texto detetado com os respetivos pontuações de confiança.

Extração de exemplo:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Detetar faces

Detetar faces ajuda a detetar informações de identificação pessoal (PII) como faces nas imagens do. Detetar potenciais faces e o número de potenciais faces em cada imagem.

Uma resposta inclui estas informações:

- Contagem de faces
- Lista de localizações de faces detetados

Extração de exemplo:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Criar e gerir listas personalizadas

Em muitos comunidades online, depois dos utilizadores carregar imagens ou outro tipo de conteúdo, itens ofensivas poderão obter partilhadas várias vezes durante os seguintes dias, semanas ou meses. Os custos de repetidamente analisar e filtrar a mesma imagem ou mesmo ligeiramente modificadas versões da imagem a partir de vários locais podem ser dispendiosa e sujeito a erros.

Em vez de moderating da mesma imagem várias vezes, adicione as imagens ofensivas à sua lista personalizada de conteúdo bloqueada. Dessa forma, o sistema de moderação de interrupção conteúdo compara as imagens de entrada relativamente as listas personalizadas e deixa de qualquer processamento adicional.

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de imagem de 5** com cada lista para **não pode exceder 10 000 imagens**.
>

O conteúdo Moderator fornece um concluída [API de gestão da lista de imagem](try-image-list-api.md) com operações para a gestão de listas de imagens personalizadas. Começar a utilizar o [imagem apresenta uma lista de API consola](try-image-list-api.md) e utilizar os exemplos de código da REST API. Consulte também o [início rápido de .NET de lista de imagem](image-lists-quickstart-dotnet.md) se estiver familiarizado com o Visual Studio e c#.

## <a name="matching-against-your-custom-lists"></a>Correspondência contra as listas personalizadas

A operação de correspondência permite a correspondência por semelhantes de imagens de entrada em relação a qualquer uma das suas listas personalizadas criados e geridos através de operações de lista.

Se for encontrada uma correspondência, a operação devolve o identificador e as etiquetas de moderação interrupção da imagem correspondente. A resposta inclui estas informações:

- Classificação de correspondência (entre 0 e 1)
- Imagem correspondente
- Etiquetas de imagem (atribuídas durante a moderação de interrupção anterior)
- Etiquetas de imagem

Extração de exemplo:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>Ferramenta de revisão humana

Para obter mais informações nuanced casos, utilizar Moderator o conteúdo [Rever ferramenta](Review-Tool-User-Guide/human-in-the-loop.md) e respetiva API a anexação os resultados de moderação interrupção e o conteúdo em revisão para a sua moderators humanos. Reveja as etiquetas máquina atribuída e confirme as decisões finais.

![Revisão de imagem para moderators humanos](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Passos Seguintes

Testar o [consola imagem moderação interrupção API](try-image-api.md) e utilizar os exemplos de código da REST API. Consulte também o [início rápido do .NET de moderação interrupção imagem](image-moderation-quickstart-dotnet.md) se estiver familiarizado com o Visual Studio e c#.
