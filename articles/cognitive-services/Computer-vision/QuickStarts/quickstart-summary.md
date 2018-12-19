---
title: Resumo do Guia de Início Rápido da Imagem Digitalizada
titleSuffix: Azure Cognitive Services
description: Nestes guias de início rápido, irá analisar uma imagem, criar uma miniatura e extrair texto manuscrito e impresso através da API de Imagem Digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 645c57c9bfeaefcfd2fc50b2dd431713fffaa42b
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53578789"
---
# <a name="quickstart-summary"></a>Início rápido: Resumo

Estes inícios rápidos apresentam informações e exemplos de código para o ajudar a começar a utilizar rapidamente o serviço de Imagem Digitalizada.

Os exemplos fazem chamadas diretas de HTTP para a API de Imagem Digitalizada. Veja a secção *Inícios Rápidos do SDK* para obter exemplos que utilizem as bibliotecas de cliente da Imagem Digitalizada, que oferecem métodos convenientes que encapsulam as chamadas HTTP num wrapper.

Para experimentar rapidamente as APIs de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

Pode utilizar o serviço de Imagem Digitalizada para realizar as seguintes tarefas:

* Analisar uma imagem remota
* Analisar uma imagem local
* Detetar celebridades e marcos (modelos de domínio)
* Gerir uma miniatura de forma inteligente
* Detetar e extrair texto impresso (OCR) a partir de uma imagem
* Detetar e extrair texto manuscrito a partir de uma imagem

O código em cada exemplo é semelhante. No entanto, eles realçam diferentes funcionalidades de Imagem Digitalizada, juntamente com técnicas diferentes para trocar dados com o serviço, como:

* _Gerir uma miniatura_ devolve uma imagem como uma matriz de bytes no corpo da resposta.
* _Analisar uma imagem local_ exige que uma imagem seja incluída no pedido como uma matriz de bytes.
* _Extrair texto manuscrito_ exige duas chamadas para obter o texto.

## <a name="summary"></a>Resumo

| Início Rápido               | Parâmetros do Pedido                          | Resposta          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Analisar uma imagem remota   | visualFeatures=Categories,Description,Color | Cadeia de carateres JSON       |
| Analisar uma imagem local    | data=image_data (matriz de bytes)                | Cadeia de carateres JSON       |
| Detetar celebridades       | model=celebrities                           | Cadeia de carateres JSON       |
| Gerar uma miniatura     | width=200&height=150&smartCropping=true     | matriz de bytes        |
| Extrair texto impresso     | language=unk&detectOrientation=true         | Cadeia de carateres JSON       |
| Extrair texto manuscrito | handwriting=true                            | URL, cadeia de carateres JSON  |

## <a name="next-steps"></a>Passos Seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito.

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
