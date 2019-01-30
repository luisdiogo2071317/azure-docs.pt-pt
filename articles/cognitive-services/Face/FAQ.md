---
title: Perguntas mais frequentes sobre - Face API
titlesuffix: Azure Cognitive Services
description: Seguem-se as respostas às perguntas mais populares sobre o serviço de API de rostos.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: b4b2c09ef608da7c52d415d5f1f2215ddc31c41a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223328"
---
# <a name="face-api-frequently-asked-questions"></a>Face API perguntas mais frequentes

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de API de rostos em [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).

-----
**Pergunta**: Que fatores podem reduzir a precisão da API de rostos para reconhecimento, verificação ou encontrar semelhante?

**resposta**: Geralmente é os mesmo casos em que os humanos têm dificuldades em identificar alguém incluindo;
* Obstructions um ou ambos os olhos de bloqueio
* Backlighting Harsh iluminação, por exemplo, grave
* Alterações de cabelo de cabelo estilo ou facial
* Alterações à idade
* Expressões faciais Extreme (por exemplo, alegam)

Face API, muitas vezes, é efetuada com êxito num desafio casos como esses, mas pode ser reduzida a precisão. Para tornar o reconhecimento mais robusto e enfrentar esses desafios, Treine as pessoas com fotos que incluem uma diversidade de ângulos e de iluminação.

-----
**Pergunta**:  Estou passando os dados de imagem binária na mas recebo um erro de "imagem da face inválido".

**resposta**:  Isso implica que o algoritmo tinha um problema ao analisar a imagem. As causas incluem:
* Os formatos de entrada de imagem suportados inclui JPEG, PNG, GIF (o primeiro quadro), BMP.
* Tamanho do ficheiro de imagem deve ser não maior do que 4MB
* O intervalo de tamanho de face detetável é 36 x 36 para 4096 x 4096 pixels. Rostos fora deste intervalo não serão detetados
* Alguns rostos poderão não ser detetados devido a desafios técnicos, por exemplo, muito grandes de face de ângulos (head-representam), oclusão grandes. Rostos frontal e quase frontal tem os melhores resultados

-----
