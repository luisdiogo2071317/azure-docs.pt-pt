---
title: O que é o serviço de reconhecimento de voz (pré-visualização)? | Microsoft Docs
description: 'O serviço de reconhecimento de voz, parte serviços cognitivos da Microsoft, unites vários serviços de voz do Azure que se encontravam anteriormente disponíveis separadamente: reconhecimento de voz do Bing (que inclui o reconhecimento de voz e o reconhecimento de voz do texto para), reconhecimento de voz personalizadas e tradução de reconhecimento de voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: e7c09eee1634c52e78a523a7cc65641ea99f23e6
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356168"
---
# <a name="what-is-the-speech-service-preview"></a>O que é o serviço de reconhecimento de voz (pré-visualização)?

O serviço de reconhecimento de voz, parte serviços cognitivos da Microsoft, unites vários serviços de voz do Azure que se encontravam anteriormente disponíveis separadamente: reconhecimento de voz do Bing (que inclui o reconhecimento de voz e o reconhecimento de voz do texto para), reconhecimento de voz personalizadas e tradução de reconhecimento de voz. Como o respetivos precursors, o serviço de reconhecimento de voz utiliza a tecnologia de tecnologias utilizadas outros produtos Microsoft, incluindo Cortana e Microsoft Office.

> [!NOTE]
> O serviço de reconhecimento de voz está atualmente em pré-visualização pública. Devolva aqui regularmente para atualizações para a documentação, exemplos de código adicionais e muito mais.

Com uma subscrição, o serviço de reconhecimento de voz unificado proporciona aos programadores uma forma fácil para fornecer as respetivas funcionalidades de ativados de reconhecimento de voz poderosas aplicações. As suas aplicações podem agora funcionalidade comandos de voz, transcription, ditado, synthesis de reconhecimento de voz e conversão.

|Função|Descrição|
|-|-|
|Conversão de Voz em Texto|Converte o reconhecimento de voz humano contínuo em texto que pode ser utilizado como entrada para a aplicação. Pode integrar com o [serviço compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para derivar intenção do utilizador utterances.|
|Conversão de Texto em Voz|Converte texto em ficheiros de áudio de reconhecimento de voz sintetizado natural sounding.|
|Voz&nbsp;tradução|Fornece traduções de reconhecimento de voz para outros idiomas, a saída de texto ou de voz.|

## <a name="using-the-speech-service"></a>Utilizar o serviço de reconhecimento de voz

O serviço de reconhecimento de voz é disponibilizado de duas formas. [O SDK](speech-sdk.md) deduz ausente os detalhes dos protocolos de rede. O [REST API](rest-apis.md) funciona com qualquer linguagem de programação, mas não fornece todas as funções oferecidas pelo SDK.

|<br>Método|Voz<br>para texto|Texto para<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|Sim|Não|Sim|Bibliotecas de linguagens de programação específicas que simplificam o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma baseado em HTTP API simples que torna mais fácil adicionar voz à sua aplicação.|

## <a name="speech-to-text"></a>Conversão de Voz em Texto

O [reconhecimento de voz para texto](speech-to-text.md) (STT), ou o reconhecimento de voz, API transcribes fluxos de áudio texto que a aplicação pode aceitar como entrada. A aplicação pode, em seguida, introduza o texto para um documento por exemplo, ou agir como um comando.

Reconhecimento de voz para texto foi otimizado em separado para conversação interativa, e cenários de ditado. Seguem-se casos de utilização comuns para reconhecimento de voz a API de texto. 

* Reconhecer breve um utterance, tal como um comando, sem resultados intermédio
* Transcribe utterance longo, previamente registada, tais como uma mensagem de correio de voz
* Transcribe voz transmissão em fluxo em tempo real, com resultados parciais, para ditado
* Determinar o que os utilizadores quiser fazê-lo com base num pedido de linguagem natural ditas

O reconhecimento de voz para texto API suporta transcription de reconhecimento de voz interativo com reconhecimento contínuo em tempo real e resultados intermédio. Também suporta a deteção de fim de voz, opcional capitalização automática e pontuação, máscara de profanity e normalização de texto.

Pode personalizar o reconhecimento de voz para texto acústica e modelos de idioma para acomodar vocabulário especializado, ambientes inúteis e formas diferentes de falando.

## <a name="text-to-speech"></a>Conversão de Texto em Voz

O [reconhecimento de voz do texto para](text-to-speech.md) (TTS), ou synthesis de reconhecimento de voz, API converte texto simples para reconhecimento de voz natural sounding, entregue à sua aplicação num ficheiro de áudio. Vários voices, vários sexo ou acentos, estão disponíveis para vários idiomas suportados.

A API suporta etiquetas de reconhecimento de voz Synthesis Markup Language (SSML), pelo que pode especificar pronunciação fonético exata para troublesome palavras. SSML também pode indicar características de reconhecimento de voz (incluindo ênfase, velocidade, volume, sexo e tipos) diretamente no texto.

Seguem-se casos de utilização comuns para a API do reconhecimento de voz para de texto.

* Saída de voz como uma saída de ecrã alternativo para os utilizadores visualmente danificada
* A pedir para aplicações no car como navegação de voz
* Interfaces de utilizador Conversational concertadamente com o reconhecimento de voz a API de texto

Se precisar de um dialecto não suportado ou deseje apenas uma voz exclusiva para a sua aplicação, o texto conversão em voz API suporta modelos de voz personalizadas.

## <a name="speech-translation"></a>Tradução de Voz

O [tradução de reconhecimento de voz](speech-translation.md) API pode ser utilizada para traduzir áudio transmissão em fluxo em quase em tempo real ou para processar o reconhecimento de voz registado. Tradução de transmissão em fluxo, o serviço devolve resultados provisórias que podem ser apresentados ao utilizador para indicar o progresso da tradução. Os resultados podem ser devolvidos como texto ou como voz.

Casos de utilização para tradução de reconhecimento de voz incluem o seguinte:

* Implementar uma aplicação móvel de tradução "conversational" ou o dispositivo para viajantes 
* Fornecer traduções automáticas para subtitling de gravações de áudio e vídeos

## <a name="speech-devices-sdk"></a>SDK de Dispositivos de Voz

Com a introdução do serviço de reconhecimento de voz unificada, a Microsoft e respetivos parceiros oferecem uma plataforma integrada hardware/software otimizada para desenvolver dispositivos preparados para reconhecimento de voz: o [SDK de dispositivos de reconhecimento de voz](speech-devices-sdk.md). Este SDK é adequado para o desenvolvimento de dispositivos de reconhecimento de voz inteligente para todos os tipos de aplicações.

O SDK de dispositivos de reconhecimento de voz permite-lhe criar os seus próprios dispositivos de ambiente com uma palavra de reativação personalizadas, para que seja exclusivo para a sua marca a rectângulos que aciona a captura de áudio. Também fornece áudio superior de processamento de canal de várias origens para reconhecimento de voz mais exata, incluindo supressão do ruído, para campo voz e beamforming.

## <a name="next-steps"></a>Passos Seguintes

Obter uma chave de subscrição de avaliação gratuita para o serviço de reconhecimento de voz.

> [!div class="nextstepaction"]
> [Experimente gratuitamente o serviço de reconhecimento de voz](get-started.md)
