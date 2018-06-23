---
title: Sobre a conversão de reconhecimento de voz | Microsoft Docs
description: Uma descrição geral das funcionalidades de tradução de reconhecimento de voz
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 43fcde887c21794989aa43540a214ef34893a630
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355616"
---
# <a name="about-the-speech-translation-api"></a>Sobre a API de tradução de reconhecimento de voz

A API de reconhecimento de voz Microsoft permite-lhe adicionar ponto-a-ponto, em tempo real, multilingues tradução de reconhecimento de voz aos seus dispositivos, ferramentas e aplicações. A API do mesma pode ser utilizada para tradução de reconhecimento de voz-para-reconhecimento de voz e reconhecimento de voz para texto.

Com a API de reconhecimento de voz do Microsoft tradutor, aplicações de cliente áudio de reconhecimento de voz para o serviço de fluxo e recebem um fluxo de resultados de volta. Estes resultados incluem o texto reconhecido no idioma de origem e a tradução na linguagem de destino. Traduções provisórias podem ser fornecidos até um utterance estiver concluída, em que momento é fornecida uma tradução final.

Opcionalmente, uma versão de áudio sintetizada da tradução final possam ser preparada, ativar a tradução de reconhecimento de voz a voz verdadeira.

A API de tradução de reconhecimento de voz utiliza um protocolo de WebSockets para fornecer um canal de comunicação de full-duplex entre o cliente e o servidor. Mas não tem de lidar com WebSockets; o SDK de reconhecimento de voz processa que por si.

A API de tradução de reconhecimento de voz emprega as mesmas tecnologias de energia vários produtos e serviços Microsoft. Este serviço já está utilizado por milhares de empresas em todo o mundo nas respetivas aplicações e os fluxos de trabalho.

## <a name="about-the-technology"></a>Sobre a tecnologia

Subjacente do motor de tradução da Microsoft é duas abordagens diferentes: análises tradução (SMT) e neuronal tradução (NMT). A última opção, uma abordagem de artificial intelligence empregar as redes neurais, é a abordagem mais moderna a tradução automática. NMT fornece traduções de melhor — não apenas mais exata, mas também mais fluent e natural. O motivo de chave para este fluidity é NMT que utiliza o contexto completo de uma frase traduzir palavras.

Hoje em dia, Microsoft foi migrada para NMT para os idiomas mais populares, empregar SMT apenas em idiomas sem frequentemente utilizados. Todos os [idiomas disponíveis para tradução de reconhecimento de voz a voz](supported-languages.md#speech-translation) com tecnologia NMT. Tradução de reconhecimento de voz para texto utilizar SMT ou NMT consoante o par de idioma. Se o idioma de destino é suportado pelo NMT, a tradução completa é NMT-se ligados à corrente. Se o idioma de destino não é suportado pela NMT, a conversão é uma versão híbrida do NMT e SMT, utilizando o inglês como "dinâmica" entre os dois idiomas.

As diferenças entre os modelos são internas para o motor de conversão. Os utilizadores finais Repare apenas a qualidade de tradução melhorada, especialmente para chinês, japonês e árabe.

> [!NOTE]
> Estiver interessado em obter mais informações sobre a tecnologia por trás do motor de tradução da Microsoft? Consulte [tradução](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)
