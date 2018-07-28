---
title: Sobre tradução de voz
description: Uma descrição geral das capacidades de tradução por voz
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 7d653a17212c727d65820382e22196d62af086e9
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324413"
---
# <a name="about-the-speech-translation-api"></a>Sobre a API de tradução de voz

A API de voz da Microsoft permite-lhe adicionar ponto-a-ponto, em tempo real, vários idioma tradução de voz às suas aplicações, ferramentas e dispositivos. A mesma API pode ser utilizada para a tradução de voz para voz e conversão de voz em texto.

Com a API de voz do Microsoft Translator, aplicativos cliente transmitir áudio de voz para o serviço e recebem um fluxo de resultados de volta. Esses resultados incluem o texto reconhecido no idioma de origem e a tradução no idioma de destino. Traduções provisórias podem ser fornecidos até que uma expressão for concluída, nesse momento é fornecida uma tradução final.

Opcionalmente, uma versão de áudio sintetizada da tradução final poderá ser preparada, ativar a tradução de voz para voz verdadeira.

A API de tradução de voz utiliza um protocolo de WebSockets para fornecer um canal de comunicação de full duplex entre o cliente e o servidor. Mas não precisa lidar com WebSockets; o SDK de voz processa isto por si.

A API de tradução de voz emprega as mesmas tecnologias que suportam vários produtos e serviços Microsoft. Este serviço já é utilizado por milhares de empresas em todo o mundo em seus aplicativos e os fluxos de trabalho.

## <a name="about-the-technology"></a>Sobre a tecnologia

Subjacentes mecanismo de tradução da Microsoft são as duas abordagens diferentes: estatísticos de tradução automática (SMT) e a tradução automática neuronal (NMT). A última opção, uma abordagem de inteligência artificial empregar as redes neurais, é a abordagem mais moderna para tradução automática. NMT fornece melhor traduções — não apenas mais precisos, mas também mais fluente e natural. O motivo principal para este fluidez é que o NMT utiliza o contexto completo de uma frase para traduzir palavras.

Hoje em dia, Microsoft tiver migrado para NMT para as linguagens mais populares, empregando SMT apenas para linguagens utilizadas menos frequentemente. Todos os [idiomas disponíveis para a tradução de voz para voz](supported-languages.md#speech-translation) têm a tecnologia NMT. Tradução de voz em texto utilizar SMT ou NMT consoante o par de idioma. Se o idioma de destino é suportado pelo NMT, a tradução inteira é com a tecnologia de NMT. Se o idioma de destino não é suportado pelo NMT, a tradução é uma mistura de NMT e SMT, usando o inglês como uma tabela "dinâmica" entre as duas linguagens.

As diferenças entre os modelos são internas para o mecanismo de tradução. Os utilizadores finais observe apenas a qualidade das traduções de melhor, especialmente para chinês, japonês e árabe.

> [!NOTE]
> Interessado em saber mais sobre a tecnologia por trás do mecanismo de tradução da Microsoft? Ver [tradução automática](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a conversão de voz em c#](how-to-translate-speech-csharp.md)
