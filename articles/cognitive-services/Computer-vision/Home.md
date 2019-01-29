---
title: O que é a API de Imagem Digitalizada? -Imagem digitalizada
titlesuffix: Azure Cognitive Services
description: O serviço Imagem Digitalizada fornece aos programadores acesso a algoritmos avançados para processamento de imagens e devolução de informações.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 08/22/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f06269b937a645a5334c1a8015528ad00adb66e8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154495"
---
# <a name="what-is-computer-vision"></a>O que é a Imagem Digitalizada?

O serviço Imagem Digitalizada baseado na cloud fornece aos programadores acesso a algoritmos avançados para processamento de imagens e devolução de informações. A Imagem Digitalizada funciona com formatos de imagem populares, como JPEG e PNG. Para analisar uma imagem, pode carregar uma imagem ou especificar um URL de imagem. Os algoritmos da Imagem Digitalizada podem analisar o conteúdo de uma imagem de diversas formas, dependendo dos elementos visuais nos quais tem interesse. Por exemplo, a Imagem Digitalizada pode determinar se uma imagem contém conteúdo para adultos ou profano, ou encontrar todos os rostos numa imagem.

Pode utilizar a Imagem Digitalizada na sua aplicação através das nossas [bibliotecas de cliente](quickstarts-sdk/csharp-analyze-sdk.md) para invocar o serviço ou invocar a [API REST](vision-api-how-to-topics/howtocallvisionapi.md) diretamente, para:

- [Analisar imagens para obter informações](#analyzing-images-for-insight)
- [Extrair texto de imagens](#extracting-text-from-images)
- [Moderar conteúdo em imagens](#moderating-content-in-images)

## <a name="analyzing-images-for-insight"></a>Analisar imagens para obter informações

Pode analisar imagens com a Imagem Digitalizada para detetar e fornecer informações sobre as características e os elementos visuais das suas imagens. Pode carregar os conteúdos de uma imagem para analisar imagens locais ou pode especificar o URL de uma imagem para analisar imagens remotas.

A Imagem Digitalizada pode realizar as seguintes ações ao analisar uma imagem:

| Ação | Descrição |
| ------ | ----------- |
|**[Etiquetar elementos visuais](concept-tagging-images.md)**|Identificar e etiquetar recursos visuais numa imagem, a partir de um conjunto de milhares de objetos reconhecíveis, vivos, paisagens e ações. Quando as etiquetas são ambíguas ou dados de conhecimento não comuns, a resposta de API fornece "sugestões" para clarificar o significado da etiqueta no contexto de uma configuração conhecida. A identificação não se limita ao motivo principal, por exemplo, uma pessoa em primeiro plano, mas também inclui o cenário (interior ou exterior), mobiliário, ferramentas, plantas, animais, acessórios, gadgets, etc.|
|**[Detetar objetos](concept-object-detection.md)**| Deteção de objetos é semelhante à marcação, mas a API devolve as coordenadas da caixa delimitadora para cada etiqueta aplicada. Por exemplo, se uma imagem contiver um cachorro, gato e person, a operação de deteção irá listar esses objetos, juntamente com suas coordenadas na imagem. Pode utilizar esta funcionalidade para processar mais relações entre os objetos numa imagem. Ele também permite-lhe saber quando existem várias instâncias da mesma marca numa imagem.|
|**[Categorizar uma imagem](concept-categorizing-images.md)**|Identifique e categorize uma imagem inteira, com uma [taxonomia de categorias](Category-Taxonomy.md) com hierarquias hereditárias principais/subordinadas. As categorias podem ser utilizadas isoladamente ou com os nossos novos modelos de identificação.<br/>Atualmente, o inglês é o único idioma suportado para identificar e categorizar imagens.|
|**[Descrever uma imagem](concept-describing-images.md)**|Gere uma descrição de uma imagem inteira numa linguagem legível por humanos, com frases completas. Os algoritmos da Imagem Digitalizada geram várias descrições com base nos objetos identificados na imagem. Cada uma das descrições é avaliada e é gerada uma pontuação de confiança. Em seguida, é devolvida uma lista ordenada da pontuação de confiança mais alta para a mais baixa.|
|**[Detetar rostos](concept-detecting-faces.md)** |Detete rostos numa imagem e forneça informações sobre cada rosto detetado. A Imagem Digitalizada devolve as coordenadas, o retângulo, o sexo e a idade para cada rosto detetado.<br/>A Imagem Digitalizada fornece um subconjunto das funcionalidades que podem ser encontradas na [Face](/azure/cognitive-services/face/) e pode utilizar o serviço Face para uma análise mais detalhada, como identificação facial e deteção de poses.|
|**[Detetar tipos de imagem](concept-detecting-image-types.md)**|Detete características sobre uma imagem como, por exemplo, se uma imagem é um desenho de linha ou a probabilidade de uma imagem ser ClipArt.|
|**[Detetar conteúdo específico de um domínio](concept-detecting-domain-content.md)**|Utilize modelos de domínio para detetar e identificar conteúdo específico de um domínio numa imagem, como celebridades e marcos de referência. Por exemplo, se uma imagem incluir pessoas, a Imagem Digitalizada pode utilizar um modelo de domínio para celebridades incluído com o serviço para determinar se as pessoas detetadas na imagem correspondem a celebridades conhecidas.|
|**[Detetar o esquema de cores](concept-detecting-color-schemes.md)**|Analise a utilização de cor numa imagem. A Imagem Digitalizada pode determinar se uma imagem é a preto e branco ou a cores, e no caso das imagens a cores, identificar as cores dominantes e de destaque.|
|**[Gerar uma miniatura](concept-generating-thumbnails.md)**|Analise o conteúdo de uma imagem para gerar uma miniatura adequada dessa imagem. Imagem digitalizada primeiro gera uma miniatura de alta qualidade e, em seguida, analisa os objetos na imagem para determinar a *área de interesse*. Imagem digitalizada, em seguida, recorta a imagem de acordo com os requisitos da área de interesse. A miniatura gerada pode ser apresentada com uma proporção diferente da proporção da imagem original, consoante as suas necessidades.|
|**[Obter a área de interesse](concept-generating-thumbnails.md#area-of-interest)**|Analisar o conteúdo de uma imagem para retornar as coordenadas do *área de interesse*. Esta é a mesma função que é utilizada para gerar uma miniatura, mas em vez de corte a imagem, de imagem digitalizada devolve as coordenadas da caixa delimitadora da região, para que o aplicativo de chamada pode modificar a imagem original, conforme desejado.|

## <a name="extracting-text-from-images"></a>Extrair texto de imagens

Pode utilizar a Imagem Digitalizada para [extrair texto com o OCR](concept-extracting-text-ocr.md) de uma imagem para um fluxo de carateres legível por computador. Se for necessário, o OCR corrige a rotação do texto reconhecido, em graus, à volta do eixo de imagem horizontal e fornece as coordenadas de fotograma de cada palavra. O OCR suporta 25 idiomas e deteta automaticamente o idioma de texto extraído.

Também pode [reconhecer texto impresso e manuscrito](concept-recognizing-text.md) a partir de uma imagem. A Imagem Digitalizada pode detetar e extrair texto impresso e manuscrito de imagens de vários objetos com diferentes superfícies e fundos, como recibos, cartazes, cartões de visita, cartas e quadros. Atualmente, o reconhecimento de texto impresso e manuscrito está em pré-visualização e o inglês é o único idioma suportado.  

## <a name="moderating-content-in-images"></a>Moderar conteúdo em imagens

Pode utilizar a Imagem Digitalizada para [detetar conteúdo para adultos e profano](concept-detecting-adult-content.md) numa imagem, classificar a probabilidade de a imagem conter conteúdo para adultos ou profano e gerar uma pontuação de confiança para ambos. O filtro para a deteção de conteúdo para adultos ou profano pode ser definido numa escala móvel para se adaptar às suas preferências.

## <a name="using-containers"></a>Através de contentores

[Utilize contentores de imagem digitalizada](computer-vision-how-to-install-containers.md) reconhecer texto impresso e manuscritos localmente, mediante a instalação de um contentor Docker padronizado mais perto aos seus dados.

## <a name="image-requirements"></a>Requisitos de imagem

A Imagem Digitalizada pode analisar imagens que cumprem os requisitos seguintes:

- A imagem tem de ser apresentada no formato JPEG, PNG, GIF ou BMP
- O tamanho do ficheiro da imagem tem de ser inferior a 4 megabytes (MB)
- As dimensões da imagem têm de ser superiores a 50 x 50 píxeis  
  Para OCR, as dimensões da imagem têm de ser entre 50 x 50 e 4200 x 4200 píxeis

## <a name="data-privacy-and-security"></a>Segurança e privacidade dos dados

Como com todos os serviços cognitivos, os desenvolvedores que usam o serviço de visão do computador devem estar ciente das políticas da Microsoft em dados do cliente. Consulte a [página dos serviços cognitivos](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) no Microsoft Trust Center para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

Comece a utilizar a Imagem Digitalizada com um dos nossos manuais de início rápido:

- [Analisar uma imagem](quickstarts-sdk/csharp-analyze-sdk.md)
- [Extrair texto manuscrito](quickstarts-sdk/csharp-hand-text-sdk.md)
- [Gerar uma miniatura](quickstarts-sdk/csharp-thumb-sdk.md)
