---
title: Comece com a API de reconhecimento de voz de Microsoft ao utilizar bibliotecas de cliente de voz do Bing | Documentos da Microsoft
description: Utilize as bibliotecas de cliente do serviço de voz de Microsoft nos serviços cognitivos da Microsoft para desenvolver aplicações que converta áudio falado em texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 6fb490def6807204943a1ce3ba3c53186055102b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048462"
---
# <a name="get-started-with-bing-speech-service-client-libraries"></a>Comece com bibliotecas de cliente do serviço de voz do Bing

Além de verificar os pedidos HTTP diretos através de uma API REST, o serviço de voz do Bing fornece aos desenvolvedores com bibliotecas de cliente de voz em diferentes idiomas. As bibliotecas de cliente de voz:

- Suporte capacidades mais avançadas no reconhecimento de fala, por exemplo, os resultados intermediários em tempo real, a transmissão de áudio há muito tempo (até 10 minutos) e reconhecimento contínuo.
- Fornecem uma API simples e idiomático no idioma de sua preferência.
- Oculta detalhes de comunicação de nível baixo.

Atualmente, as bibliotecas de cliente de voz do Bing seguintes estão disponíveis:

- [C# a área de trabalho biblioteca](GetStartedCSharpDesktop.md)
- [Biblioteca de serviço c#](GetStartedCSharpServiceLibrary.md)
- [Biblioteca JavaScript](GetStartedJSWebsockets.md)
- [Biblioteca de Java para Android](GetStartedJavaAndroid.md)
- [Biblioteca de Objective-C para iOS](Get-Started-ObjectiveC-iOS.md)

> [!NOTE] 
Em Maio de 2018, também lançámos o novo [serviço de voz](../../speech-service/index.yml) em pré-visualização pública. É recomendável que [experimentar gratuitamente](../../speech-service/get-started.md). 

## <a name="additional-resources"></a>Recursos adicionais

- O [amostras](../samples.md) página fornece exemplos de completos para utilizar bibliotecas de cliente de voz.
- Se precisar de uma biblioteca de cliente que ainda não é suportada, pode criar seu próprio SDK. Implementar o [protocolo WebSocket de voz](../API-Reference-REST/websocketprotocol.md) na plataforma e utilize a linguagem da sua preferência.

## <a name="license"></a>Licença

Todos os exemplos e SDKs de serviços cognitivos são licenciados com a licença do MIT. Para obter mais informações, consulte [licença](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
