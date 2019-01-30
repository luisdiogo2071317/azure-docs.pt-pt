---
title: Sobre voz-serviço de voz
titleSuffix: Azure Cognitive Services
description: A API de voz oferece mais de 75 vozes em mais de 45 idiomas e localidades. Para utilizar tipos de voz padrão, só tem de especificar o nome de voz com alguns outros parâmetros ao chamar o serviço de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 7439a2c5466784a881e71649fa86a8e51d757957
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226422"
---
# <a name="about-the-text-to-speech-api"></a>Sobre a API de texto para discurso

O **voz** (TTS) API converte a entrada de texto em voz com som natural (também denominado *síntese de fala*).

Para gerar voz, a sua aplicação envia pedidos de HTTP POST para a API de texto para voz. Lá, o texto é sintetizado em voz com som humanos e retornado como um arquivo de áudio. Uma variedade de vozes e idiomas são suportados.

Os cenários em que fala síntese está a ser adotada incluem:

* *Melhorar a acessibilidade:* **voz** tecnologia permite que os proprietários de conteúdo e os editores a responder para as pessoas de diferentes formas interagirem com os seus conteúdos. As pessoas com dificuldades de leitura ou o visual impairment apreciam a capacidade de consumir conteúdos aurally. Voz de saída também torna mais fácil para as pessoas usufruir conteúdo textual, como jornais ou posta em blog, em dispositivos móveis ao exercitar transporte ou viagens.

* *A responder em cenários de multitarefa:* **voz** permite que as pessoas que visam absorver informações importantes rapidamente e confortavelmente ao dirigir ou caso contrário, um conveniente fora ler ambiente. A navegação é um aplicativo comum nessa área.

* *Aprimoramento de aprendizagem com vários modos de:* Pessoas diferentes saiba a melhor de formas diferentes. Especialistas de aprendizagem online têm mostrado que fornecer o texto e voz em conjunto pode ajudar a facilitar as informações saber mais e manter.

* *Fornecimento de bots intuitivo ou assistentes:* A capacidade de comunicar com pode ser uma parte integral de um chatbot inteligente ou um assistente virtual. Cada vez mais empresas estão desenvolvendo bots de bate-papo para fornecer experiências de serviço de cliente atraente para seus clientes. Voz adiciona outra dimensão, permitindo que as respostas do bot para que sejam recebidas aurally (por exemplo, por telefone).

## <a name="voice-support"></a>Suporte de voz

A Microsoft **voz** serviço oferece mais de 75 vozes em mais de 45 idiomas e localidades. Para utilizar estes padrão "tipos de voz", só precisa de especificar o nome de voz com alguns outros parâmetros quando chama a API de REST do serviço. Para obter mais informações sobre vozes, localidades e idiomas suportados, consulte [idiomas suportados](language-support.md#text-to-speech).

### <a name="neural-voices"></a>Vozes neurais

Voz neural pode ser utilizado para tornar as interações com chatbots e assistentes virtual mais natural e envolventes converter textos digital, como o e-livros em audiobooks e melhorar a sistemas de navegação no carro. Com o prosody natural de semelhante à humana e articulation clara de palavras, o Neural TTS reduziu significativamente fadiga escuta quando interage com os sistemas de IA. Para obter mais informações sobre as vozes neurais, consulte [idiomas suportados](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Vozes personalizadas

Personalização de texto para discurso de voz permite-lhe criar uma voz reconhecível, um do-única para sua marca: um *tipo de voz.* Para criar o seu tipo de voz, fazer uma gravação de studio e carregar os scripts associados como os dados de treinamento. O serviço, em seguida, cria um modelo de voz única ajustado para a gravação. Pode utilizar o seu tipo de voz para sintetizar voz. Para obter mais informações, consulte [tipos de voz personalizada](how-to-customize-voice-font.md).

## <a name="api-capabilities"></a>Capacidades de API

Muitas das funcionalidades dos **voz** API, sobretudo em torno da personalização, estão disponíveis através de REST. A tabela seguinte resume as capacidades de cada método de acessar a API. Para obter uma lista completa de capacidades e detalhes de API, consulte [Swagger referência](https://westus.cris.ai/swagger/ui/index).

| Caso de utilização | REST | SDKs |
|-----|-----|-----|----|
| Carregar conjuntos de dados para adaptação de voz | Sim | Não |
| Criar e gerir modelos de tipo de letra de voz | Sim | Não |
| Criar e gerir implementações de tipo de letra de voz | Sim | Não |
| Criar e gerir os testes de tipo de letra de voz| Sim | Não |
| Gerir Subscrições | Sim | Não |

> [!NOTE]
> A API implementa limitação que limita os pedidos de API para 25 por 5 segundos. Cabeçalhos de mensagens informará os limites.

## <a name="next-steps"></a>Passos Seguintes

* [Obter uma subscrição de serviços de voz gratuito](https://azure.microsoft.com/try/cognitive-services/)
* [Quickstart: Converter a voz, o Python](quickstart-python-text-to-speech.md)
* [Quickstart: Converter a voz, .NET Core](quickstart-dotnet-text-to-speech.md)
* [Referência da API REST](rest-apis.md)
