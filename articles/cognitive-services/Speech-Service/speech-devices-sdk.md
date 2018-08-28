---
title: Sobre os dispositivos de voz SDK
description: Uma introdução ao SDK de dispositivos de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 97a5d00d43ac6a9d4b28ba23ca69d9b67e1bdf6f
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049804"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Sobre os dispositivos de voz SDK (pré-visualização)

O [serviço do Microsoft Speech](overview.md) funciona com uma ampla variedade de dispositivos e fontes de áudio. Agora pode tirar seus aplicativos de fala para o próximo nível com correspondentes de hardware e software. O SDK de dispositivos de voz é uma biblioteca previamente atento emparelhada com o objetivo da matriz de microfone kits de desenvolvimento. Que lhe dá a capacidade de rapidamente testar novos cenários de voz, o SDK de dispositivos de voz torna mais fácil integrar o serviço de voz de Microsoft com base na cloud no seu dispositivo e criar uma experiência de usuário excecional para os seus clientes. 

O SDK de dispositivos de voz consome os [SDK de voz](speech-sdk.md)e utiliza o SDK de voz para enviar áudio processado pelo nosso algoritmo de processamento de áudio avançado da matriz de microfone do dispositivo para o [o serviço de voz de Microsoft](overview.md).  Ele usa o áudio de vários canal para oferecer extremidade campo mais preciso [reconhecimento de fala](speech-to-text.md) através de supressão de ruído, cancelamento de eco, beamforming e reverberation eliminação.

O SDK de dispositivos de voz também permite que crie dispositivos de ambiente com os seus próprios [personalizado do word de reativação](speech-devices-sdk-create-kws.md)— para que a indicação de que inicia uma interação do utilizador é exclusiva para a sua marca. 

O SDK facilita a uma variedade de cenários habilitados para voz, como a unidade através de sistemas de ordenação, assistentes na loja ou in-home e alto-falantes inteligentes. Poderia responder aos utilizadores com texto, falar para si numa predefinição ou [voz personalizada](how-to-customize-voice-font.md), forneça os resultados da pesquisa, [traduzir](speech-translation.md) para outros idiomas e muito mais. Estamos ansiosos por ver o que cria!



## <a name="development-kit-providers"></a>Fornecedores do kit de desenvolvimento

Estruturas de referência do sistema completa, ponto-a-ponto. Mais em breve!

|||
|-|-|
|[![Logótipo ROOBO](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO fornece soluções completas de sistema de ia para electric aparelhos domésticos, automóveis, robôs, toys e outros setores. Estruturas de referência do ROOBO reduzem significativamente o desenvolvimento time-to-market por meio da integração com o serviço Microsoft Speech. [Visite ROOBO](http://ddk.roobo.com/)|

## <a name="next-steps"></a>Passos Seguintes

Para começar, obtenha uma [conta gratuita do Azure](https://azure.microsoft.com/free/ai/) e inscreva-se para o SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Inscreva-se o SDK de dispositivos de voz](get-speech-devices-sdk.md)

