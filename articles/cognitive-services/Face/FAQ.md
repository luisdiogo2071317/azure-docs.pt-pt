---
title: Perguntas mais frequentes sobre - Face API
titlesuffix: Azure Cognitive Services
description: Seguem-se as respostas às perguntas mais populares sobre o serviço de API de rostos.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: sbowles
ms.openlocfilehash: 9b30fa0fbbd655c03800dadb19cc2568d404204d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129561"
---
# <a name="face-api-frequently-asked-questions"></a>Face API perguntas mais frequentes

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-face-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de API de rostos em [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte ajuda e suporte no [UserVoice](https://cognitive.uservoice.com/).

-----
**Pergunta**: que fatores podem reduzir a precisão da API de rostos para reconhecimento, verificação ou encontrar semelhante?

**Resposta**: geralmente é os mesmo casos em que os humanos têm dificuldades em identificar alguém incluindo;
* Obstructions um ou ambos os olhos de bloqueio
* Backlighting Harsh iluminação, por exemplo, grave
* Alterações de cabelo de cabelo estilo ou facial
* Alterações à idade
* Expressões faciais Extreme (por exemplo, alegam)

Face API, muitas vezes, é efetuada com êxito num desafio casos como esses, mas pode ser reduzida a precisão. Para tornar o reconhecimento mais robusto e enfrentar esses desafios, Treine as pessoas com fotos que incluem uma diversidade de ângulos e de iluminação.

-----
**Pergunta**: Estou passando os dados de imagem binária na mas recebo um erro de "imagem da face inválida".

**Resposta**: isso implica que o algoritmo tinha um problema ao analisar a imagem. As causas incluem:
* Os formatos de entrada de imagem suportados inclui JPEG, PNG, GIF (o primeiro quadro), BMP.
* Tamanho do ficheiro de imagem deve ser não maior do que 4MB
* O intervalo de tamanho de face detetável é 36 x 36 para 4096 x 4096 pixels. Rostos fora deste intervalo não serão detetados
* Alguns rostos poderão não ser detetados devido a desafios técnicos, por exemplo, muito grandes de face de ângulos (head-representam), oclusão grandes. Rostos frontal e quase frontal tem os melhores resultados

-----
