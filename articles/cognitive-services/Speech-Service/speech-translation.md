---
title: Sobre tradução de voz - serviços de voz
titlesuffix: Azure Cognitive Services
description: A API de serviço de voz permite-lhe adicionar ponto-a-ponto, em tempo real, vários idioma tradução de voz às suas aplicações, ferramentas e dispositivos. A mesma API pode ser utilizada para a tradução de voz para voz e conversão de voz em texto.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9d94105377597ca8e79cc43ed0903371e185d510
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086713"
---
# <a name="about-the-speech-translation-api"></a>Sobre a API de tradução de voz

A API de serviço de voz permite-lhe adicionar ponto-a-ponto, em tempo real, vários idioma tradução de voz às suas aplicações, ferramentas e dispositivos. A mesma API pode ser utilizada para a tradução de voz para voz e conversão de voz em texto.

Com a API de voz do Translator, aplicativos cliente transmitir áudio de voz para o serviço e recebem um fluxo de resultados de volta. Esses resultados incluem o texto reconhecido no idioma de origem e a tradução no idioma de destino. Traduções provisórias podem ser fornecidos até que uma expressão for concluída, nesse momento é fornecida uma tradução final.

Opcionalmente, uma versão de áudio sintetizada da tradução final poderá ser preparada, ativar a tradução de voz para voz verdadeira.

A API de tradução de voz utiliza um protocolo de WebSockets para fornecer um canal de comunicação de full duplex entre o cliente e o servidor. Mas não precisa lidar com WebSockets; o SDK de voz processa isto por si.

A API de tradução de voz emprega as mesmas tecnologias que suportam vários produtos e serviços Microsoft. Este serviço já é utilizado por milhares de empresas em todo o mundo em seus aplicativos e os fluxos de trabalho.

## <a name="about-the-technology"></a>Sobre a tecnologia

Subjacentes mecanismo de tradução da Microsoft são as duas abordagens diferentes: estatísticos de tradução automática (SMT) e a tradução automática neuronal (NMT). A última opção, uma abordagem de inteligência artificial empregar as redes neurais, é a abordagem mais moderna para tradução automática. NMT fornece melhor traduções — não apenas mais precisos, mas também mais fluente e natural. O motivo principal desta fluidez deve-se à NMT utilizar o contexto completo de uma frase para traduzir palavras.

Hoje em dia, Microsoft tiver migrado para NMT para as linguagens mais populares, empregando SMT apenas para linguagens utilizadas menos frequentemente. Todos os [idiomas disponíveis para a tradução de voz para voz](language-support.md#speech-translation) têm a tecnologia NMT. Tradução de voz em texto utilizar SMT ou NMT consoante o par de idioma. Se o idioma de destino for suportado por NMT, a tradução inteira terá a tecnologia NMT. Se o idioma de destino não é suportado pelo NMT, a tradução é uma mistura de NMT e SMT, usando o inglês como uma tabela "dinâmica" entre as duas linguagens.

As diferenças entre os modelos são internas para o mecanismo de tradução. Os utilizadores finais observe apenas a qualidade das traduções de melhor, especialmente para chinês, japonês e árabe.

> [!NOTE]
> Interessado em saber mais sobre a tecnologia por trás do mecanismo de tradução da Microsoft? Ver [tradução automática](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a conversão de voz em c#](how-to-translate-speech-csharp.md)
* [Veja como a conversão de voz em C++](how-to-translate-speech-cpp.md)
* [Veja como a conversão de voz em Java](how-to-translate-speech-java.md)
