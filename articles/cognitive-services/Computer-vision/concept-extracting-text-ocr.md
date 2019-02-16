---
title: Extrair texto com reconhecimento ótico de carateres (OCR) - de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a extração de texto com reconhecimento ótico de carateres (OCR) usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb73eb9fdd6879a5fbe1fed820bf92b2d627b65
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310445"
---
# <a name="extract-text-with-optical-character-recognition"></a>Extrair texto com reconhecimento ótico de carateres

Funcionalidade de reconhecimento ótico de carateres (OCR) do imagem digitalizada Deteta o conteúdo de texto numa imagem e converte o texto identificado num fluxo de carateres legível por máquina. Pode usar o resultado para muitas finalidades, como pesquisa, registros médicos, segurança e banca. 

OCR suporta 25 idiomas: Árabe, chinês simplificado, chinês tradicional, checo, dinamarquês, Holandês, inglês, finlandês, francês, alemão, grego, húngaro, italiano, japonês, coreano, norueguês, polaco, português, romeno, russo, Sérvio (Cirílico e Latim), eslovaco, espanhol, Sueco e turco. OCR Deteta automaticamente o idioma do texto detetado.

Se necessário, OCR corrige a rotação do texto reconhecido, retornando o deslocamento de rotação em graus sobre o eixo horizontal de imagem. OCR também fornece as coordenadas de quadro de cada palavra, como mostrado na ilustração seguinte.

![Diagrama a ilustrar uma imagem a ser girada e o texto a ser lido e delimitado](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>Requisitos de imagem

Imagem digitalizada pode extrair texto usando OCR a partir de imagens que cumprem os requisitos seguintes:

* A imagem tem de ser apresentada no formato JPEG, PNG, GIF ou BMP
* O tamanho da imagem de entrada deve ser entre 50 x 50 e 4200 x 4200 pixels
* O texto na imagem pode ser girado pelo qualquer múltiplo de 90 graus, mais um pequeno ângulo de até 40 graus.

## <a name="improving-ocr-accuracy"></a>Melhorar a precisão de OCR

A precisão de reconhecimento do texto depende da qualidade da imagem. Uma leitura incorreta pode ser causada pelo seguinte:

* Imagens desfocadas.
* Texto manuscrito ou cursivo.
* Estilos de tipos de letra artísticos.
* Tamanho do texto pequeno.
* Planos de fundo complexos, sombras ou distorção de perspetiva ou por reflexos sobre o texto.
* Letras maiúsculas demasiado grandes ou em falta no início das palavras.
* Texto inferior à linha, superior à linha ou rasurado.

### <a name="ocr-limitations"></a>Limitações de OCR

Nas imagens em que o texto é dominante, os falsos positivos podem ser provenientes de palavras parcialmente reconhecidas. Em algumas imagens, especialmente as fotos sem qualquer texto, muita precisão pode variar consoante o tipo de imagem.

## <a name="next-steps"></a>Passos Seguintes

Consulte a [documentação de referência de OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) para saber mais.
