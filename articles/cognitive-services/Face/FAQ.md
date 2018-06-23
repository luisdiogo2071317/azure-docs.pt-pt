---
title: Perguntas mais frequentes sobre o serviço de API de rostos em | Microsoft Docs
description: Seguem-se as respostas às perguntas sobre o serviço de API de rostos em mais populares.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: da2f75deef8a8beea3ba23b6a39eb6d2fe104b54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351559"
---
# <a name="face-api-frequently-asked-questions"></a>Enfrentam API perguntas mais frequentes
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas às suas perguntas neste FAQ, tente pedir a Comunidade de API de rostos em [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).

-----
**Pergunta**: fatores que podem reduzir a precisão da API de letra para reconhecimento, a verificação ou Localizar semelhante?

**Resposta**: geralmente, é os mesmos casos onde humans ter dificuldade em identificar alguém incluindo;
* Obstructions bloquear continuará ser de um ou ambos
* Backlighting Harsh iluminação da, por exemplo, grave
* Alterações à hair de estilo ou facial hair
* Alterações devido a idade
* É necessário ter muito expressões facial (por exemplo, screaming)

Enfrentam API, muitas vezes, é efetuada com êxito nestes casos um desafio, mas pode ser reduzida precisão. Para tornar o reconhecimento mais robusto e resolver estes desafios, preparar as pessoas com fotografias que incluem um diversity de ângulos e iluminação da.

-----
**Pergunta**: Estou a transmitir os dados binários imagem mas receber um erro de "imagem enfrentam inválido".

**Resposta**: Isto implica que o algoritmo tido um problema ao analisar a imagem. Causas incluem:
* Os formatos de entrada de imagem suportados inclui PNG, JPEG, BMP e GIF (moldura primeiro).
* Tamanho do ficheiro de imagem deve ser não superior a 4MB
* O intervalo de tamanho de rostos em detetável é 36 x 36 para 4096 x 4096 pixéis. Não serão detetados faces fora deste intervalo
* Alguns faces não seja detetados devido a dificuldades técnicas, por exemplo, muito grandes enfrentam os ângulos (head-implicar), occlusion grande. Faces frontal e perto frontal tem obter os melhores resultados

-----
