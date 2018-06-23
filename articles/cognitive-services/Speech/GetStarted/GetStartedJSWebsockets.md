---
title: Introdução à API de reconhecimento de voz Microsoft em JavaScript | Microsoft Docs
description: Utilize a API de reconhecimento de voz do Microsoft serviços cognitivos para desenvolver aplicações que continuamente converter áudio ditas em texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 12/21/2017
ms.author: zhouwang
ms.openlocfilehash: 56c41fd7f6a00d80bc6bccd61894654e057e926e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352189"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>Introdução à API de reconhecimento de voz em JavaScript

Pode desenvolver aplicações que converter áudio ditas texto, utilizando a API de reconhecimento de voz. A biblioteca de clientes JavaScript utiliza o [protocolo WebSocket do serviço de reconhecimento de voz](../API-Reference-REST/websocketprotocol.md), que lhe permite comunicar com e receber transcribed texto em simultâneo. Este artigo ajuda-o a começar com a API de reconhecimento de voz em JavaScript.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos. Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

> [!IMPORTANT]
> Obter uma chave de subscrição. Antes de poder utilizar bibliotecas de cliente de reconhecimento de voz, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>Introdução

Nesta secção, irá guiá-lo os passos necessários para carregar uma página HTML de exemplo. O exemplo está localizado no nosso [repositório do github](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Pode **abra o exemplo diretamente** do repositório, ou **abra o exemplo a partir de uma cópia local** do repositório. 

> [!NOTE]
> Alguns browsers bloqueiam o acesso microfone desproteger origem. Por isso, recomenda-se para alojar o exemplo / 'a aplicação' em https para pôr a funcionar em todos os browsers suportados. 

### <a name="open-the-sample-directly"></a>Abra o exemplo diretamente

Adquirir uma chave de subscrição, conforme descrito acima. Em seguida, abra o [ligação para o exemplo](https://htmlpreview.github.io/?https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Isto irá carregar a página no browser predefinido (composto utilizando [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Abra o exemplo a partir de uma cópia local

Para experimentar o exemplo localmente, clone este repositório:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

Compilar as origens de TypeScript e pacote/browserfy-las num único ficheiro JavaScript ([npm](https://www.npmjs.com/) tem de ser instalado no seu computador). Mude para a raiz do repositório clonado e execute os comandos:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Abra `samples\browser\Sample.html` no seu browser favorito.

## <a name="next-steps"></a>Passos Seguintes

Estão disponíveis mais informações sobre como incluir o SDK no seu próprio página Web [aqui](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Observações

- A API de reconhecimento de voz suporta três [modos de reconhecimento](../concepts.md#recognition-modes). Pode alternar o modo atualizando o **Setup()** função encontrado no ficheiro de Sample.html. O exemplo define o modo como `Interactive` por predefinição. Para alterar o modo, atualize o parâmetro `SR.RecognitionMode.Interactive` para outro modo. Por exemplo, altere o parâmetro para `SR.RecognitionMode.Conversation`.
- Para obter uma lista completa de idiomas suportados, consulte [idiomas suportados](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>Tópicos relacionados

- [Repositório de amostra da API de reconhecimento de voz de JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [Introdução à REST API](GetStartedREST.md)
