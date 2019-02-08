---
title: Descrição geral do serviço de voz personalizada no Azure | Documentos da Microsoft
description: O Serviço de Voz Personalizada é um serviço com base na cloud que permite aos utilizadores personalizar modelos de voz para conversão de voz em texto.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: nitinme
ms.openlocfilehash: cdd89d68db0b4b8cb92aa685a0fe823979cb936d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877250"
---
# <a name="what-is-custom-speech-service"></a>O que é o Serviço de Voz Personalizada?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

O Serviço de Voz Personalizada é um serviço com base na cloud que oferece aos utilizadores a capacidade de personalizar modelos de voz para conversão de Voz em Texto.
Para utilizar o Serviço de Voz Personalizada, consulte o [Portal do Serviço de Voz Personalizada](https://cris.ai).

O Serviço de Voz Personalizada permite criar modelos de idioma personalizados e modelos acústicos adaptados às suas aplicações e aos seus utilizadores. Ao carregar os dados específicos de voz e/ou texto para o Serviço de Voz Personalizada, pode criar modelos personalizados que podem ser utilizados em conjunto com os modelos de voz de última geração da Microsoft.

Por exemplo, se estiver a adicionar interação de voz a um telemóvel, tablet ou aplicação de PC, pode criar um modelo de idioma personalizado que pode ser combinado com um modelo acústico da Microsoft para criar um ponto final de conversão de voz em texto concebido especialmente para a sua aplicação. Se a aplicação tiver sido concebida para utilização num ambiente específico ou por uma população de utilizadores em particular, também pode criar e implementar um modelo acústico personalizado com este serviço.


## <a name="how-do-speech-recognition-systems-work"></a>Como funcionam os sistemas de reconhecimento de voz?
Os sistemas de reconhecimento de voz são compostos por vários componentes que funcionam em conjunto. Dois dos componentes mais importantes são o modelo acústico e o modelo de idioma.

O modelo acústico é um classificador que rotula fragmentos curtos de áudio num de vários fonemas, ou unidades de som, num dado idioma. Por exemplo, a palavra “speech” é composta por quatro fonemas “s p iy ch”. Estas classificações são feitas na ordem de 100 vezes por segundo.

O modelo de idioma é uma distribuição de probabilidade sobre sequências de palavras. O modelo de idioma ajuda o sistema a decidir entre sequências de palavras que soam de forma semelhante, com base na probabilidade das próprias sequências de palavras. Por exemplo, “recognize speech” e “wreck a nice beach” soam de forma parecida, mas a primeira hipótese é muito mais provável de ocorrer e, portanto, será atribuída uma melhor classificação pelo modelo do idioma.

Os modelos acústicos e de idioma são modelos estatísticos aprendidos através dos dados de preparação. Como resultado, têm um melhor desempenho quando a voz com que se deparam quando são utilizados em aplicações é semelhante aos dados observados durante a preparação. Os modelos acústicos e de idioma no motor de conversão de Voz para Texto da Microsoft foram preparados numa coleção enorme de voz e texto, e proporcionam um desempenho de última geração para os cenários de utilização mais comuns, como a interação com o Cortana no seu smartphone, tablet ou PC, ao pesquisar na Web através de voz ou ao ditar mensagens de texto a um amigo.

## <a name="why-use-the-custom-speech-service"></a>Porquê utilizar o Serviço de Voz Personalizada?
Embora o motor de conversão de Voz para Texto da Microsoft seja de classe mundial, visa os cenários descritos acima. No entanto, se espera que as consultas de voz na sua aplicação contenham itens de vocabulário particulares, como nomes de produtos ou gíria, que ocorrem raramente no discurso habitual, é provável que consiga obter um melhor desempenho ao personalizar o modelo de idioma.

Por exemplo, se estava a criar uma aplicação para pesquisar o MSDN por voz, é provável que termos como “orientado por objetos” ou “espaço de nomes” ou “ponto net” apareçam com mais frequência do que em aplicações de discurso habitual. A personalização do modelo de idioma irá permitir que o sistema aprenda isto.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar o serviço de voz personalizada, consulte a [Portal de serviço de voz personalizada](https://cris.ai).

* [Introdução](cognitive-services-custom-speech-get-started.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
