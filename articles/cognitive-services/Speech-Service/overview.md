---
title: O que é o serviço de voz (pré-visualização)?
description: 'O serviço de voz, parte dos serviços cognitivos da Microsoft, une os vários serviços de voz do Azure que estavam disponíveis anteriormente em separado: de voz do Bing (que inclui o reconhecimento de voz e de texto em fala), conversão de voz personalizada e tradução de voz.'
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: fc6ef4ccbe6e392b991bd375afcc63a54f58db02
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285464"
---
# <a name="what-is-the-speech-service-preview"></a>O que é o serviço de voz (pré-visualização)?

O serviço de voz, parte dos serviços cognitivos da Microsoft, une os vários serviços de voz do Azure que estavam disponíveis anteriormente em separado: de voz do Bing (que inclui o reconhecimento de voz e de texto em fala), conversão de voz personalizada e tradução de voz. Como seu precursors, o serviço de voz com tecnologia as tecnologias usadas em outros produtos da Microsoft, incluindo o Cortana e o Microsoft Office.

> [!NOTE]
> O serviço de voz está atualmente em pré-visualização pública. Regresse aqui regularmente para atualizações à documentação, exemplos de código adicionais e muito mais.

Com uma subscrição, o serviço de voz unificado, dá aos programadores uma forma fácil de dar a seus recursos do aplicativos poderosos habilitado para fala. As suas aplicações podem agora apresentam os comandos de voz, transcrição, ditado, síntese de fala e a tradução.

|Função|Descrição|
|-|-|
|Conversão de Voz em Texto|Converte voz humana contínua em texto que pode ser utilizado como entrada para a sua aplicação. Pode ser integrado com o [serviço de compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para derivar a intenção do utilizador de expressões.|
|Conversão de Texto em Voz|Converte texto em arquivos de áudio de fala sintetizada de som natural.|
|Voz&nbsp;tradução|Fornece as traduções de voz em outros idiomas, com a saída de texto ou de voz.|

## <a name="using-the-speech-service"></a>Utilizar o serviço de voz

O serviço de voz é disponibilizado em duas formas. [O SDK](speech-sdk.md) abstrai os detalhes dos protocolos de rede. O [REST API](rest-apis.md) funciona com qualquer linguagem de programação, mas não oferece todas as funções oferecidas pelo SDK.

|<br>Método|Voz<br>para texto|Texto a<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|Sim|Não|Sim|Bibliotecas de linguagens de programação específicas que simplificam o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma simple baseada em HTTP API que torna mais fácil adicionar voz à sua aplicação.|

## <a name="speech-to-text"></a>Conversão de Voz em Texto

O [conversão de voz em texto](speech-to-text.md) (STT), ou o reconhecimento de voz, API converte os fluxos de áudio em texto que seu aplicativo pode aceitar como entrada. Em seguida, a sua aplicação pode, por exemplo, introduzir o texto num documento ou agir sobre o mesmo como um comando.

Conversão de voz em texto foi otimizado separadamente para conversação interativa e cenários de ditado. Seguem-se os casos de utilização comuns para a conversão de voz em texto de API. 

* Reconhecer uma expressão breve, tal como um comando, sem resultados intermediárias
* Transcrição de uma expressão de longa e gravado anteriormente, por exemplo, uma mensagem de correio de voz
* Transcrição de voz de transmissão em fluxo em tempo real, com resultados parciais, de ditado
* Determinar o que os usuários desejam fazer com base numa solicitação de linguagem natural falada

A conversão de voz em texto API suporta a transcrição de voz interativa com o reconhecimento contínuo em tempo real e resultados intermediárias. Também suporta deteção de fim de voz, capitalização e pontuação automáticas opcionais, máscara de linguagem obscena e normalização de texto.

Pode personalizar a conversão de voz em texto acústico e modelos de linguagem para acomodar o vocabulário especializado, ambientes desnecessárias e formas diferentes de falar.

## <a name="text-to-speech"></a>Conversão de Texto em Voz

O [texto em voz](text-to-speech.md) (TTS), ou sintetização de voz, API converte texto sem formatação em voz com som de natural, entregue ao seu aplicativo num arquivo de áudio. Várias vozes, variando em sexo ou com/sem acento, estão disponíveis para muitos idiomas com suporte.

A API suporta [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md) etiquetas, então, pode especificar exata pronúncia fonética palavras problemáticas. O SSML também pode indicar características de voz (incluindo ênfase, ritmo, volume, género e tom) diretamente no texto.

Seguem-se os casos de utilização comuns para a API de voz em texto.

* Saída de voz como uma saída de tela alternativa para os utilizadores visualmente-com deficiências visuais
* Solicitar a aplicações no carro, como navegação de voz
* Interfaces do usuário conversacionais em conjunto com a conversão de voz em texto de API

Se necessitar de um dialeto não suportado ou apenas desejar uma voz única para a sua aplicação, o texto em voz, API suporta [modelos de voz personalizada](how-to-customize-voice-font.md).

## <a name="speech-translation"></a>Tradução de Voz

O [tradução por voz](speech-translation.md) API pode ser usada para converter áudio de transmissão em fluxo em quase tempo real ou para processar a voz gravado. Tradução de transmissão em fluxo, o serviço devolve resultados intermediárias que podem ser apresentados ao utilizador para indicar o progresso de tradução. Os resultados podem ser devolvidos como texto ou voz.

Casos de utilização para tradução de voz incluem o seguinte:

* Implementar uma aplicação móvel de tradução "conversacionais" ou o dispositivo para viajantes 
* Fornecer traduções automática para limitado de gravações de áudio e vídeos

## <a name="speech-devices-sdk"></a>SDK de Dispositivos de Voz

Com a introdução do serviço de voz unificado, a Microsoft e respetivos parceiros oferecem uma plataforma de hardware/software integrado otimizada para o desenvolvimento de dispositivos com capacidade de voz: os [SDK de dispositivos de voz](speech-devices-sdk.md). Este SDK é adequado para o desenvolvimento de dispositivos de voz inteligentes para todos os tipos de aplicações.

O SDK de dispositivos de voz permite-lhe criar seus próprios dispositivos de ambiente com uma palavra de reativação personalizadas, para que a indicação que aciona a captura de áudio é exclusiva para a sua marca. Também fornece processamento de áudio superior de origens de vários canais para um reconhecimento de voz mais exato, incluindo supressão de ruído, voz de campo distante e beamforming.

O SDK é baseado em sockets web através da porta 443.

## <a name="next-steps"></a>Passos Seguintes

Obtenha uma chave de subscrição de avaliação gratuita para o serviço de voz.

> [!div class="nextstepaction"]
> [Experimente gratuitamente o serviço de voz](get-started.md)
