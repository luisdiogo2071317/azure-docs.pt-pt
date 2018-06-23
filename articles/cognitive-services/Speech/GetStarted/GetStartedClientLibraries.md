---
title: Começar com a API de reconhecimento de voz do Microsoft através da utilização de bibliotecas de cliente | Microsoft Docs
description: Utilize as bibliotecas de cliente do serviço de reconhecimento de voz Microsoft nos serviços cognitivos da Microsoft para desenvolver aplicações que se convertem áudio ditas texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 5abe5bc48c2bd73d0facf33e41a8076df2972153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352178"
---
# <a name="get-started-with-speech-service-client-libraries"></a>Começar com bibliotecas de cliente do serviço de reconhecimento de voz

Para além de efetuar pedidos HTTP diretos através de uma API REST, o serviço de reconhecimento de voz proporciona aos programadores com bibliotecas de cliente de reconhecimento de voz em idiomas diferentes. As bibliotecas de cliente de reconhecimento de voz:

- Suporta funcionalidades mais avançadas no reconhecimento de voz, tais como resultados intermédios em tempo real, a sequência de áudio longa (até 10 minutos) e reconhecimento contínuo.
- Forneça uma API simple e idiomatic no idioma da sua preferência.
- Ocultar detalhes de comunicação de baixo nível.

Atualmente, as bibliotecas de cliente de reconhecimento de voz seguintes estão disponíveis:

- [C# ambiente de trabalho biblioteca](GetStartedCSharpDesktop.md)
- [Biblioteca de serviço do c#](GetStartedCSharpServiceLibrary.md)
- [Biblioteca JavaScript](GetStartedJSWebsockets.md)
- [Biblioteca Java para Android](GetStartedJavaAndroid.md)
- [Biblioteca de Objective-C para iOS](Get-Started-ObjectiveC-iOS.md)

## <a name="additional-resources"></a>Recursos adicionais

- O [amostras](../samples.md) página fornece amostras completas a utilizar bibliotecas de cliente de reconhecimento de voz.
- Se precisar de uma biblioteca de cliente que ainda não é suportada, pode criar o seus próprios SDK. Implementar o [protocolo WebSocket de reconhecimento de voz](../API-Reference-REST/websocketprotocol.md) a plataforma e utilizar o idioma da sua preferência.

## <a name="license"></a>Licença

Todos os cognitivos de serviços de SDKs e ferramentas são licenciadas com a licença MIT. Para obter mais informações, consulte [licença](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
