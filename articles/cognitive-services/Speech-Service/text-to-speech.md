---
title: Sobre o reconhecimento de voz do texto para | Microsoft Docs
description: Uma descrição geral das funcionalidades do texto para voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 84baf03c83bb63883b80982056cdf6e1e25b3fb7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355633"
---
# <a name="about-the-text-to-speech-api"></a>Sobre a API do reconhecimento de voz para de texto

O **reconhecimento de voz do texto para** (TTS) API do serviço de reconhecimento de voz converte texto de entrada do reconhecimento de voz natural sounding (também denominado *synthesis de reconhecimento de voz*).

Para gerar o reconhecimento de voz, a aplicação envia pedidos de HTTP POST para o serviço de reconhecimento de voz. Não existe, o texto é synthesized para reconhecimento de voz sounding humanos e devolvido como um ficheiro de áudio. Uma variedade de voices e idiomas são suportados.

Os cenários em que voz synthesis está a ser adotado uma larga maioria incluem:

* *Melhorar a acessibilidade:* **reconhecimento de voz do texto para** tecnologia permite que os proprietários de conteúdo e publicadores para responder às pessoas de diferentes formas interagem com o respetivo conteúdo. As pessoas com visual impairment ou leitura dificuldades Agradecemos a capacidade de consumir conteúdo aurally. Voz saída também torna mais fácil para as pessoas textual conteúdos, tais como newspapers ou blogues, nos dispositivos móveis ao commuting ou exercising.

* *A responder em cenários de multitasking:* **reconhecimento de voz do texto para** permite que as pessoas absorver informações importantes rapidamente e comfortably ao ocasionar ou caso contrário, uma prática fora ao ler o ambiente. Navegação é uma aplicação comum nesta área. 

* *Otimização da learning com vários modos:* diferentes pessoas saiba melhor formas diferentes. A formação online especialistas têm mostrado que fornecer de voz e texto em conjunto pode ajudar a facilitar informações saber mais e a manter.

* *Entrega bots intuitiva ou assistentes:* a capacidade para falar pode ser uma parte integral de um bot de chat inteligente ou um assistente virtual. As empresas mais estiver a desenvolver bots chat para fornecer ao cliente interagir experiências de serviço para os seus clientes. Voz adiciona outra dimensão ao permitir que as respostas o bot a ser recebido aurally (por exemplo, por telefone).

## <a name="voice-support"></a>Suporte de voz

A Microsoft **Text-to-Speech** service oferece mais do que 75 voices em mais de 45 idiomas e regiões. Para utilizar estes padrão "tipos de letra voz", só tem de especificar o nome de voz com alguns outros parâmetros quando chamar a API de REST do serviço. Para obter os detalhes de voices suportados, consulte [idiomas suportados](supported-languages.md). 

Se pretender uma voz exclusiva para a sua aplicação, pode criar [tipos de letra de voz personalizadas](how-to-customize-voice-font.md) do seus próprio amostras de reconhecimento de voz.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)
