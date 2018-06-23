---
title: Perguntas mais frequentes de emoções API | Microsoft Docs
description: Obtenha respostas às perguntas mais frequentes sobre a API de emoções nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 8532d7c00fd8d7b01d84b5e55cb9bbc60241789c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351577"
---
# <a name="emotion-api-frequently-asked-questions"></a>Emoções API perguntas mais frequentes
 
> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas às suas perguntas neste FAQ, tente pedir a Comunidade de API de emoções no [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).  

-----

**Pergunta**: *que tipos de imagens de obter os melhores resultados a partir da API emoções?*

**Resposta**: utilizar imagens facial frontal unobstructed, completas para obter os melhores resultados. Fiabilidade diminui com parciais faces frontal e emoções API não pode reconhecer emotions nas imagens em que a letra é igual ou superior a rotated 45 graus.

-----

**Pergunta**: *emotions quantos emoções API pode identificar?*

**Resposta**: API de emoções reconhece oito emotions diferentes universalmente aceites: 
* Felicidade
* Tristeza
* Surpresa
* Raiva
* Medo
* Desdém
* Repulsa 
* Neutro 

-----

**Pergunta**: *existe qualquer forma para transmitir um fluxo de vídeo em direto para a API e obter o resultado em simultâneo?*

**Resposta**: utilizar a API de emoções baseada em imagens e chamá-lo em cada moldura ou ignore frames para um desempenho.  Exemplos de análise de moldura por moldura vídeos estão disponíveis.

-----

**Pergunta**: *estou a transmitir os dados binários de imagem, mas proporciona-me: "imagem enfrentam inválido.**

**Resposta**: Isto implica que o algoritmo tido um problema ao analisar a imagem.  
* Os formatos de entrada de imagem suportados inclui PNG, JPEG, BMP e GIF (moldura primeiro). 
* Tamanho do ficheiro de imagem deve ser não superior a 4MB
* O intervalo de tamanho de rostos em detetável é 36 x 36 para 4096 x 4096 pixéis. Não serão detetados faces fora deste intervalo
* Alguns faces não seja detetados devido a dificuldades técnicas, por exemplo, muito grandes enfrentam os ângulos (head-implicar), occlusion grande. Faces frontal e perto frontal tem obter os melhores resultados

-----
