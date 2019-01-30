---
title: Perguntas mais frequentes sobre - API de emoções
titlesuffix: Azure Cognitive Services
description: Obtenha respostas para perguntas mais frequentes sobre a API de emoções.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: ded91c6de498b130cc26109a70e89955dd70d862
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208997"
---
# <a name="emotion-api-frequently-asked-questions"></a>API de emoções perguntas mais frequentes

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/).

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-emotion-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de API de emoções em [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).  

-----

**Pergunta**: *Que tipos de imagens de obtém os melhores resultados de API de emoções?*

**resposta**: Utilize imagens de faciais frontal unobstructed, completas para obter melhores resultados. Confiabilidade diminui com parciais rostos frontal e a API de emoções talvez não reconheça emoções em imagens onde o mostrador é girado mais de 45 graus.

-----

**Pergunta**: *O número de emoções pode identificar a API de emoções*

**resposta**: API de emoções reconhece oito emoções universalmente aceites diferentes:
* Felicidade
* Tristeza
* Surpresa
* Raiva
* Medo
* Desdém
* Repulsa
* Neutro

-----

**Pergunta**: *Existe alguma forma para passar uma transmissão de vídeo em direto para a API e obter o resultado em simultâneo?*

**resposta**: Utilize a imagem com base em API de emoções e chamá-lo em cada quadro ou ignorar os quadros de desempenho.  Exemplos de análise de quadro por quadro de vídeo estão disponíveis.

-----

**Pergunta**: *Estou passando os dados de imagem binária na, mas ele me oferece: "Imagem da face inválido.**

**resposta**: Esta mensagem indica que o algoritmo tinha um problema ao analisar a imagem.  
* Os formatos de entrada de imagem suportados incluem JPEG, PNG, GIF (o primeiro quadro), BMP
* Tamanho do ficheiro de imagem deve ser não maior do que 4 MB
* O intervalo de tamanho de face detetável é 36 x 36 para 4096 x 4096 pixels. Não seja detectados rostos fora deste intervalo
* Alguns rostos poderão não ser detetados por causa de desafios técnicos, por exemplo, grandes de face de ângulos (head-representam), oclusão grandes. Rostos frontal e quase frontal tem os melhores resultados

-----
