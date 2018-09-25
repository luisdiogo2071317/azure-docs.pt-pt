---
title: Descrição geral do serviço de voz personalizada no Azure | Documentos da Microsoft
description: O serviço de voz personalizada é um serviço baseado na nuvem que permite aos utilizadores personalizar modelos de voz para transcrição de voz em texto.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/07/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: da88989753069f7ba8ca2c2e2806a648f3df4e3c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948371"
---
# <a name="what-is-custom-speech-service"></a>O que é o Serviço de Voz Personalizada?

Serviço de voz personalizada é um serviço baseado na nuvem que fornece aos utilizadores a capacidade de personalizar modelos de voz para transcrição de voz em texto.
Para utilizar o serviço de voz personalizada, veja a [Portal de serviço de voz personalizada](https://cris.ai).

O serviço de voz personalizada permite-lhe criar modelos de linguagem personalizados e modelos acústicos adaptados às suas aplicações e os seus utilizadores. Ao carregar a sua voz específico e/ou dados de texto para o serviço de voz personalizada, pode criar modelos personalizados que podem ser utilizados em conjunto com modelos de voz topo de gama existentes da Microsoft.

Por exemplo, se estiver a adicionar interação de voz para um telemóvel, tablet ou uma aplicação de PC, pode criar um modelo de idioma personalizado que pode ser combinado com um modelo acústico da Microsoft para criar um ponto final de voz em texto projetado especialmente para a sua aplicação. Se seu aplicativo for desenvolvido para utilização num determinado ambiente ou por uma população do utilizador em particular, também pode criar e implementar um modelo acústico personalizado com este serviço.


## <a name="how-do-speech-recognition-systems-work"></a>Como funcionam os sistemas de reconhecimento de fala?
Sistemas de reconhecimento de fala são compostos por vários componentes que funcionam em conjunto. Dois dos componentes mais importantes são o modelo acústico e o modelo de idioma.

O modelo acústico é um classificador que rotula fragmentos curtos de áudio num de vários fonemas, ou unidades de som, num determinado idioma. Por exemplo, a palavra "speech" é composta por quatro fonemas "s p iy ch". Estas classificações são feitas na ordem de 100 vezes por segundo.

O modelo de idioma é uma distribuição de probabilidade sobre sequências de palavras. O modelo de idioma ajuda o sistema a decidir entre sequências de palavras que soam de forma semelhante, com base na probabilidade das próprias sequências de palavras. Por exemplo, “recognize speech” e “wreck a nice beach” soam de forma parecida, mas a primeira hipótese é muito mais provável de ocorrer e, portanto, será atribuída uma melhor classificação pelo modelo do idioma.

Modelos acústicos e de idioma são modelos estatísticos aprendidos em dados de treinamento. Como resultado, eles têm um melhor desempenho quando a conversão de voz que se deparam quando usado em aplicativos é semelhante aos dados observados durante o treinamento. Os modelos de acústica e idioma no mecanismo de conversão de voz para texto da Microsoft foram treinados numa coleção enorme de voz e texto e fornecem desempenho de topo de gama para os cenários de utilização mais comuns, como interagir com a Cortana no seu inteligente telefone, tablet ou PC, pesquisa na web por meio da voz ou ditar mensagens de texto para um amigo.

## <a name="why-use-the-custom-speech-service"></a>Porquê utilizar o serviço de voz personalizada?
Enquanto o mecanismo de conversão de voz para texto da Microsoft é a nível mundial, ele se destina a cenários descritos acima. No entanto, se necessitar de consultas de voz na sua aplicação contenham itens de vocabulário particulares, como nomes de produtos ou gíria, que ocorrem raramente no discurso habitual, é provável que pode obter um melhor desempenho ao personalizar o modelo de idioma.

Por exemplo, se estava a criar uma aplicação para pesquisar o MSDN por voz, é provável que termos como “orientado por objetos” ou “espaço de nomes” ou “ponto net” apareçam com mais frequência do que em aplicações de discurso habitual. A personalização do modelo de idioma irá permitir que o sistema aprenda isto.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar o serviço de voz personalizada, veja [Custom Speech serviço Portal] (https://cris.ai).

* [Introdução](cognitive-services-custom-speech-get-started.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
