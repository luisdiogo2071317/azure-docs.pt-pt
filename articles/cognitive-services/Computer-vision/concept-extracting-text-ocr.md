---
title: Extrair texto com o OCR - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a extração de texto com reconhecimento ótico de carateres (OCR) usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: b1fc2e18dd5fc8e995c2d997683a4c5e5c501087
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188971"
---
# <a name="extracting-text-with-optical-character-recognition"></a>Extrair texto com reconhecimento Ótico de carateres

Tecnologia de reconhecimento ótico de carateres (OCR) na imagem digitalizada Deteta o conteúdo de texto numa imagem e extrai o texto identificado para um fluxo de carateres legível por máquina. Pode utilizar o resultado para pesquisa e inúmeras outras finalidades, como registos médicos, segurança e atividades bancárias. O OCR deteta automaticamente o idioma. O OCR poupa tempo e é mais cómodo para os utilizadores ao permitir tirar fotografias do texto em vez de o transcrever.

O OCR suporta 25 idiomas. Essas linguagens são: Árabe, chinês simplificado, chinês tradicional, checo, dinamarquês, Holandês, inglês, finlandês, francês, alemão, grego, húngaro, italiano, japonês, coreano, norueguês, polaco, português, romeno, russo, Sérvio (Cirílico e Latim), eslovaco, espanhol, Sueco e turco.

Se for necessário, o OCR corrige a rotação do texto reconhecido, em graus, à volta do eixo de imagem horizontal. OCR fornece as coordenadas de quadro de cada palavra, como mostrado na ilustração seguinte.

![Diagrama a ilustrar uma imagem a ser girada e o texto a ser lido e delimitado](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>Requisitos de OCR

Imagem digitalizada pode extrair texto usando OCR a partir de imagens que cumprem os requisitos seguintes:

* A imagem tem de ser apresentada no formato JPEG, PNG, GIF ou BMP
* O tamanho da imagem de entrada deve ser entre 50 x 50 e 4200 x 4200 pixels


A imagem de entrada pode ser rodada qualquer múltiplo de 90 graus além de um ângulo pequeno de até 40 graus.

## <a name="improving-ocr-accuracy"></a>Melhorar a precisão de OCR

A precisão de reconhecimento do texto depende da qualidade da imagem. As seguintes situações podem causar uma leitura incorreta:

* Imagens desfocadas.
* Texto manuscrito ou cursivo.
* Estilos de tipos de letra artísticos.
* Tamanho do texto pequeno.
* Planos de fundo complexos, sombras ou distorção de perspetiva ou por reflexos sobre o texto.
* Letras maiúsculas demasiado grandes ou em falta no início das palavras.
* Texto inferior à linha, superior à linha ou rasurado.

### <a name="ocr-limitations"></a>Limitações de OCR

Em fotografias em que o texto é dominante, os falsos positivos podem ser provenientes de palavras parcialmente reconhecidas. Em alguns fotografias, especialmente as fotos sem qualquer texto muito precisão pode variar consoante o tipo de imagem.

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [reconhecer texto impresso e manuscritos](concept-recognizing-text.md).
