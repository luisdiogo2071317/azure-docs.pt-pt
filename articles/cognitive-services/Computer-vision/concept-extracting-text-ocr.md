---
title: Extrair texto com o OCR - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a extração de texto com reconhecimento ótico de carateres (OCR) usando a API de imagem digitalizada.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: af88063a68cef4953da8357596ce64e4510e8d36
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985461"
---
# <a name="extracting-text-with-ocr"></a>Extrair texto com o OCR

Tecnologia de reconhecimento ótico de carateres (OCR) na imagem digitalizada Deteta o conteúdo de texto numa imagem e extrai o texto identificado para um fluxo de carateres legível por máquina. Pode usar o resultado de pesquisa e inúmeras outras finalidades, como registros médicos, segurança e banca. Deteta automaticamente o idioma. O OCR poupa tempo e cómodo para os utilizadores ao permitir tirar fotografias do texto em vez de transcrever o texto.

OCR suporta 25 idiomas. Essas linguagens são: Árabe, chinês simplificado, chinês tradicional, checo, dinamarquês, Holandês, inglês, finlandês, francês, alemão, grego, húngaro, italiano, japonês, coreano, norueguês, polaco, português, romeno, russo, Sérvio (Cirílico e Latim) Eslovaco, espanhol, sueco e turco.

Se for necessário, OCR corrige a rotação do texto reconhecido, em graus, à volta do eixo horizontal de imagem. OCR fornece as coordenadas de quadro de cada palavra, como mostrado na ilustração seguinte.

![Descrição geral de OCR](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>Requisitos de OCR

Imagem digitalizada pode extrair texto usando OCR a partir de imagens que cumprem os requisitos seguintes:

* A imagem tem de ser apresentada no formato JPEG, PNG, GIF ou BMP
* O tamanho da imagem de entrada deve ser entre 40 x 40 e 3200 x 3200 pixels
* A imagem não pode ser maior do que 10 megapixels

A imagem de entrada pode ser rodada qualquer múltiplo de 90 graus além de um ângulo pequeno de até 40 graus.

## <a name="improving-ocr-accuracy"></a>Melhorar a precisão de OCR

A precisão de reconhecimento de texto depende a qualidade da imagem. Uma leitura incorreta pode ser causada por seguintes situações:

* Imagens indistinta.
* Texto manuscrito ou cursivo.
* Estilos de tipo de letra artístico.
* Tamanho do texto pequeno.
* Planos de fundo complexos, sombras ou glare pela distorção de texto ou uma perspetiva.
* Demasiado grandes ou em falta letras maiúsculas nos primórdios de palavras
* Dolní Index Je, superior ou rasurado texto.

### <a name="ocr-limitations"></a>Limitações de OCR

Em fotografias em que o texto é dominante, os falsos positivos podem ser provenientes de palavras parcialmente reconhecidas. Em alguns fotografias, especialmente as fotos sem qualquer texto muito precisão pode variar consoante o tipo de imagem.

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [reconhecer texto impresso e manuscritos](concept-recognizing-text.md).