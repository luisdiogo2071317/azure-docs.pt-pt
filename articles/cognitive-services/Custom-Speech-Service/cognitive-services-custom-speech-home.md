---
title: Descrição geral do serviço de reconhecimento de voz personalizada no Azure | Microsoft Docs
description: O serviço de reconhecimento de voz personalizadas é um serviço baseado na nuvem que permite aos utilizadores personalizar os modelos de reconhecimento de voz para reconhecimento de voz para texto transcription.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ms.openlocfilehash: a6139283a555f8f97371c02f9d1f3d53dc6f15d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352903"
---
# <a name="what-is-custom-speech-service"></a>O que é o serviço de reconhecimento de voz personalizadas?

Serviço de reconhecimento de voz personalizadas é um serviço baseado na nuvem que fornece aos utilizadores a capacidade para personalizar os modelos de reconhecimento de voz para transcription de reconhecimento de voz para texto.
Para utilizar o serviço de reconhecimento de voz personalizadas, consulte o [Portal de serviço de reconhecimento de voz personalizadas](https://cris.ai).

O serviço de reconhecimento de voz personalizada permite-lhe criar modelos de idioma personalizado e acústica modelos e adaptados às suas aplicações e os seus utilizadores. Ao carregar o reconhecimento de voz específico e/ou os dados de texto para o serviço de reconhecimento de voz personalizadas, pode criar modelos personalizados que podem ser utilizados em conjunto com os modelos de estado da última reconhecimento de voz existentes da Microsoft.

Por exemplo, se estiver a adicionar a interação de voz para telemóvel, tablet ou PC aplicação, pode criar um modelo de idioma personalizado que pode ser combinado com o modelo de acústica da Microsoft para criar um ponto final de reconhecimento de voz para texto concebido especialmente para a sua aplicação. Se a aplicação foi concebida para utilização num determinado ambiente ou por uma população de utilizador em particular, também pode criar e implementar um modelo personalizado acústica com este serviço.


## <a name="how-do-speech-recognition-systems-work"></a>Como funcionam os sistemas de reconhecimento de voz?
Sistemas de reconhecimento de voz são constituídos por vários componentes que funcionam em conjunto. Dois dos componentes mais importantes são acústica modelo e o modelo de linguagem.

O modelo acústica for um classificador que etiquetas curtos fragmentos de áudio numa de um número de fonemas ou unidades de som, por um determinado idioma. Por exemplo, o palavra "reconhecimento de voz" é composto por quatro fonemas "s p iy categoria". Estas classificações são feitas na ordem de 100 vezes por segundo.

O modelo de idioma é uma distribuição de probabilidade sobre sequências de palavras. O modelo de idioma ajuda o sistema a decidir entre sequências de palavras que soam de forma semelhante, com base na probabilidade das próprias sequências de palavras. Por exemplo, “recognize speech” e “wreck a nice beach” soam de forma parecida, mas a primeira hipótese é muito mais provável de ocorrer e, portanto, será atribuída uma melhor classificação pelo modelo do idioma.

Modelos de acústica e de idioma são modelos análises aprendidos em dados de preparação. Como resultado, desempenhadas melhor quando o reconhecimento de voz que se deparam quando utilizado em aplicações é semelhante aos dados observados durante formação. Os modelos acústica e de idioma no motor de reconhecimento de voz para texto da Microsoft foi preparados uma coleção grande de reconhecimento de voz e o texto e fornecem desempenho de-de-última geração para os cenários de utilização mais comuns, tais como interagir com Cortana no seu smart telemóvel, tablet ou PC, pesquise web por voz ou dictating as mensagens de texto para um friend.

## <a name="why-use-the-custom-speech-service"></a>Porquê utilizar o serviço de reconhecimento de voz personalizadas?
Enquanto o motor de reconhecimento de voz para texto da Microsoft é trabalho, é direcionado para os cenários descritos acima. No entanto, se prevê que as consultas de voz para a aplicação que contém os itens de vocabulário específica, tais como nomes de produtos ou jargon que ocorrem raramente no reconhecimento de voz típico, é provável que pode obter um melhor desempenho ao personalizar o modelo de linguagem.

Por exemplo, se estava a criar uma aplicação para pesquisar o MSDN por voz, é provável que termos como “orientado por objetos” ou “espaço de nomes” ou “ponto net” apareçam com mais frequência do que em aplicações de discurso habitual. A personalização do modelo de idioma irá permitir que o sistema aprenda isto.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar o serviço de reconhecimento de voz personalizadas, consulte [personalizada voz Portal do serviço] (https://cris.ai).

* [Introdução](cognitive-services-custom-speech-get-started.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
