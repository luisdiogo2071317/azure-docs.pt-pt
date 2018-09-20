---
title: Perguntas mais frequentes sobre - API de emoções
titlesuffix: Azure Cognitive Services
description: Obtenha respostas para perguntas mais frequentes sobre a API de emoções.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ms.openlocfilehash: 56b3f2899f1e77c1a2b840285e2c69bdb8987ff4
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363093"
---
# <a name="emotion-api-frequently-asked-questions"></a>API de emoções perguntas mais frequentes
 
> [!IMPORTANT]
> Pré-visualização da API de vídeo termina a 30 de Outubro de 2017. Experimente a nova [pré-visualização de API do indexador de vídeo](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente informações dos vídeos e para melhorar as experiências de deteção de conteúdos, como resultados de pesquisa, através da deteção falada, rostos, carateres e emoções. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de API de emoções em [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).  

-----

**Pergunta**: *que tipos de imagens, obtenha o melhor resulta da API de emoções?*

**Resposta**: utilizar imagens de faciais frontal unobstructed, completas para obter melhores resultados. Confiabilidade diminui com parciais rostos frontal e a API de emoções talvez não reconheça emoções em imagens onde o mostrador é igual ou superior a girada 45 graus.

-----

**Pergunta**: *emoções quantos pode identificar a API de emoções?*

**Resposta**: API de emoções reconhece oito emoções diferentes universalmente aceites: 
* Felicidade
* Tristeza
* Surpresa
* Raiva
* Medo
* Desdém
* Repulsa 
* Neutro 

-----

**Pergunta**: *existe alguma forma para passar uma transmissão de vídeo em direto para a API e obter o resultado em simultâneo?*

**Resposta**: utilizar a API de emoções baseada em imagens e chamá-lo em cada quadro ou ignorar os quadros de desempenho.  Exemplos de análise de quadro por quadro de vídeo estão disponíveis.

-----

**Pergunta**: *, estou passando os dados de imagem binária na, mas ele me oferece: "imagem da face inválido.**

**Resposta**: isso implica que o algoritmo tinha um problema ao analisar a imagem.  
* Os formatos de entrada de imagem suportados inclui JPEG, PNG, GIF (o primeiro quadro), BMP. 
* Tamanho do ficheiro de imagem deve ser não maior do que 4MB
* O intervalo de tamanho de face detetável é 36 x 36 para 4096 x 4096 pixels. Rostos fora deste intervalo não serão detetados
* Alguns rostos poderão não ser detetados devido a desafios técnicos, por exemplo, muito grandes de face de ângulos (head-representam), oclusão grandes. Rostos frontal e quase frontal tem os melhores resultados

-----
