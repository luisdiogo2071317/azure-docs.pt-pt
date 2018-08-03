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
ms.openlocfilehash: 0cd6d984ac9329112aa388e8d8ee808d4c3e6227
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445639"
---
# <a name="what-is-the-speech-service-preview"></a>O que é o serviço de voz (pré-visualização)?

O serviço de voz com tecnologia as tecnologias usadas em outros produtos da Microsoft, incluindo o Cortana e o Microsoft Office.  Esse mesmo serviço está disponível para todos os clientes como um serviço cognitivo. 

> [!NOTE]
> O serviço de voz está atualmente em pré-visualização pública. Regresse aqui regularmente para atualizações à documentação, exemplos de código adicionais e muito mais.

Com uma subscrição, o nosso serviço de voz dá aos programadores uma forma fácil de dar a seus recursos do aplicativos poderosos habilitado para fala. As suas aplicações podem agora apresentam os comandos de voz, transcrição, ditado, síntese de fala e a tradução.

## <a name="speech-service-features"></a>Funcionalidades do serviço de voz

|Função|Descrição|
|-|-|
|[Conversão de voz em texto](speech-to-text.md)| Converte os fluxos de áudio em texto que seu aplicativo pode aceitar como entrada. Também se integra com o [serviço de compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS) para derivar a intenção do utilizador de expressões.|
|[Voz](text-to-speech.md)| Converte texto sem formatação para conversão de voz com o som natural, entregue ao seu aplicativo num arquivo de áudio. Várias vozes, variando em sexo ou com/sem acento, estão disponíveis para muitos idiomas com suporte. |
|[Tradução de voz](speech-translation.md)| Pode ser utilizado para traduzir o áudio de transmissão em fluxo em quase tempo real ou para processar a voz gravado. |
|[Dispositivos de voz SDK](speech-devices-sdk.md)| Com a introdução do serviço de voz unificado, a Microsoft e respetivos parceiros oferecem uma plataforma de hardware/software integrado otimizada para o desenvolvimento de dispositivos habilitados para conversão de voz |

## <a name="using-the-speech-service"></a>Utilizar o serviço de voz

O serviço de voz é disponibilizado em duas formas. [O SDK](speech-sdk.md) abstrai os detalhes dos protocolos de rede. O [REST API](rest-apis.md) funciona com qualquer linguagem de programação, mas não oferece todas as funções oferecidas pelo SDK.

|<br>Método|Voz<br>para texto|Texto a<br>Voz|Voz<br>Tradução|<br>Descrição|
|-|-|-|-|-|
|[SDKs](speech-sdk.md)|Sim|Não|Sim|Bibliotecas de linguagens de programação específicas que simplificam o desenvolvimento.|
|[REST](rest-apis.md)|Sim|Sim|Não|Uma simple baseada em HTTP API que torna mais fácil adicionar voz à sua aplicação.|

## <a name="next-steps"></a>Passos Seguintes

Obtenha uma chave de subscrição de avaliação gratuita para o serviço de voz.

> [!div class="nextstepaction"]
> [Experimente gratuitamente o serviço de voz](get-started.md)
