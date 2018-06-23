---
title: Ponto final de pré-visualização de URL - serviços cognitivos Microsoft do projeto | Microsoft Docs
description: Resumo de ponto final do URL de pré-visualização.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ddd53aa49db01d7a6db397eb285d0854edc59388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353996"
---
# <a name="project-url-preview-endpoint"></a>Ponto final de pré-visualização do URL do projeto

A API de pré-visualização do URL inclui um ponto final.

## <a name="endpoint"></a>Ponto Final
Para obter uma pré-visualização de URL, envie um pedido para o seguinte ponto final. Utilize os cabeçalhos e os parâmetros de URL para outras especificações.

GET:
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

````

### <a name="query-parameters"></a>Parâmetros de consulta
|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|Q|URL para pré-visualização|Cadeia |Sim|
|safeSearch|Conteúdo para adultos ilegal ou pirated conteúdo, está bloqueado com o código de erro 400 e o *isFamilyFriendly* sinalizador não é devolvido. <p>É o comportamento para o conteúdo para adultos legal, abaixo. Código de estado devolve 200 e o *isFamilyFriendly* sinalizador está definido como false.<ul><li>safeSearch = strict: título, a descrição, URL e imagem não serão devolvidos.</li><li>safeSearch = moderada; Obter o título, o URL e descrição mas não a imagem descritiva.</li><li>safeSearch = desativado; Obter todos os resposta objetos/elementos – título, o URL, descrição e imagem.</li></ul> |Cadeia|Não é necessária. </br> Por predefinição safeSearch = rigorosa.| 

## <a name="response-object"></a>Objeto de resposta

A resposta inclui os cabeçalhos de HTTP e o objeto de página Web com atributos, conforme mostrado no exemplo seguinte: `name`, `url`, `description`, `isFamilyFriendly`, e `primaryImageOfPage`.

````
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](csharp.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Início rápido de JavaScript](javascript.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)
