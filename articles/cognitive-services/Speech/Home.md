---
title: Serviço de reconhecimento de voz Microsoft | Microsoft Docs
description: Utilize a API de reconhecimento de voz do Microsoft para adicionar condicionada por voz ações às suas aplicações, incluindo a interação com utilizadores em tempo real.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: c041132e992f07e94e4b6669ec7ce174f7c2d0dd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352207"
---
# <a name="microsoft-speech-api-overview"></a>Descrição geral da API de reconhecimento de voz da Microsoft

A API de reconhecimento de voz baseado na nuvem do Microsoft fornece aos programadores uma forma fácil de criar funcionalidades poderosas ativada de reconhecimento de voz nas respetivas aplicações, como o controlo de comando de voz, caixa de diálogo de utilizador utilizando a conversação de reconhecimento de voz natural e transcription de reconhecimento de voz e ditado. A API de reconhecimento de voz Microsoft suporta *reconhecimento de voz para texto* e *reconhecimento de voz do texto para* conversão.

- **Reconhecimento de voz para texto** API converte voz humana texto que pode ser utilizado como entrada ou de comandos para controlar a sua aplicação.
- **O reconhecimento de voz do texto para** API converte texto em fluxos de áudio que podem ser reproduzidos novamente para o utilizador da sua aplicação.

## <a name="speech-to-text-speech-recognition"></a>Reconhecimento de voz para texto (reconhecimento de voz)

O reconhecimento de voz Microsoft API *transcribes* fluxos de áudio texto que a aplicação pode apresentar ao utilizador ou agir como comando de entrada. Fornece duas formas para programadores adicionar o reconhecimento de voz para as suas aplicações: REST APIs **ou** bibliotecas de cliente baseada em Websocket.

- [RESTO das APIs](GetStarted/GetStartedREST.md): os programadores podem utilizar chamadas HTTP das aplicações para o serviço de reconhecimento de voz.
- [Bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md): para funcionalidades avançadas, os programadores podem transferir bibliotecas de cliente da Microsoft Speech e ligue para as suas aplicações.  As bibliotecas de cliente estão disponíveis em várias plataformas (Windows, Android, iOS) através de vários idiomas (c#, Java, JavaScript, ObjectiveC). Ao contrário das REST APIs, as bibliotecas de cliente utilizam procotol baseado em Websocket.

| Casos de utilização | [APIs REST](GetStarted/GetStartedREST.md) | [Bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Converter um áudio suma ditas, por exemplo, os comandos (comprimento áudio < 15 s) sem resultados intermédio | Sim | Sim |
| Converter um áudio longo (> 15 s) | Não | Sim |
| Áudio de fluxo com resultados provisórias pretendida | Não | Sim |
| Compreender o texto convertido a partir de áudio utilizando LUIS | Não | Sim |

Qualquer programadores abordagem escolha (REST APIs ou bibliotecas de cliente), o serviço de reconhecimento de voz Microsoft suporta o seguinte:

- Avançadas tecnologias de reconhecimento de voz da Microsoft que são utilizadas pelo Cortana, ditado do Office, Office tradutor e outros produtos Microsoft.
- Reconhecimento contínuo em tempo real. O API de reconhecimento de voz permite aos utilizadores transcribe áudio texto em tempo real e suporta para receber os resultados intermédios das palavras que tenham sido reconhecidos até ao momento. O serviço de reconhecimento de voz também suporta a deteção de fim de voz. Além disso, os utilizadores possam escolher capacidades de formatação adicionais, como maiúsculas/minúsculas e pontuação, máscara profanity e a normalização de texto.
- Suporte otimizado resultados de reconhecimento de voz para *interativa*, *conversação*, e *ditado* cenários. Para cenários de utilizador que necessite de modelos de idioma personalizado e acústica modelos, [serviço de reconhecimento de voz personalizadas](../custom-speech-service/cognitive-services-custom-speech-home.md) permite-lhe criar modelos de reconhecimento de voz e adaptada às suas aplicações e os seus utilizadores.
- Suporta muitos idiomas ditas dialetos vários. Para a lista completa dos idiomas suportados em cada modo de reconhecimento, consulte [idiomas reconhecimento](api-reference-rest/supportedlanguages.md).
- Integração com compreensão de idiomas. Para além de conversão de áudio entrado em texto, o *reconhecimento de voz para texto* proporciona uma capacidade adicional para compreender o que significa que o texto de aplicações. Utiliza o [Service(LUIS) inteligente de compreensão de idiomas](../LUIS/Home.md) para extrair pendentes e entidades do texto reconhecido.

### <a name="next-steps"></a>Passos Seguintes

- Começar a utilizar o serviço de reconhecimento de voz Microsoft com [REST APIs](GetStarted/GetStartedREST.md) ou [bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md).
- Veja [aplicações de exemplo](samples.md) no seu preferencial linguagem de programação.
- Aceda à secção de referência para localizar [Microsoft voz protocolo](API-Reference-REST/websocketprotocol.md) detalhes e referências de API.

## <a name="text-to-speech-speech-synthesis"></a>Texto para reconhecimento de voz (synthesis de reconhecimento de voz)

*O reconhecimento de voz do texto para* APIs utilizam REST para converter o texto estruturado para uma sequência de áudio. As APIs fornecem rápida reconhecimento de voz do texto para conversão em vários idiomas e voices. Além disso utilizadores tem também a capacidade de alterar as características de áudio pronunciação, volume, rápida etc. utilizar SSML etiquetas.

### <a name="next-steps"></a>Passos Seguintes

- Começar a utilizar o serviço de reconhecimento de voz do texto para Microsoft: [texto a referência da API de reconhecimento de voz](api-reference-rest/bingvoiceoutput.md). Para obter a lista completa de idiomas e voices suportados pelo texto conversão em voz, consulte [regiões suportadas e tipos de letra de voz](api-reference-rest/bingvoiceoutput.md#SupLocales).
