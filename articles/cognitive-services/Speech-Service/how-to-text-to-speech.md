---
title: Utilize texto conversão em voz utilizando os serviços de reconhecimento de voz | Microsoft Docs
description: Saiba como utilizar o texto a utilizar para reconhecimento de voz no serviço de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 6c358b5a40b1d8e91c2e1af5eb493b13604cf82e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045063"
---
# <a name="use-text-to-speech-in-speech-service"></a>Utilize "Reconhecimento de voz do texto para" no serviço de reconhecimento de voz

O serviço de reconhecimento de voz fornece uma funcionalidade de reconhecimento de voz do texto para através de um pedido HTTP simples. PUBLIQUE o texto a ser ditas para o ponto final adequado e o serviço devolve um ficheiro de áudio (`.wav`) que contém synthesized reconhecimento de voz. A aplicação, em seguida, pode utilizar este áudio como likes-lo.

O corpo da mensagem de pedido para reconhecimento de voz do texto para pode ser texto simples (ASCII ou UTF8) ou um [SSML](speech-synthesis-markup.md) documento. Pedidos de texto simples de autenticação são ditas com uma voz de predefinição. Na maioria dos casos, que pretende utilizar um corpo SSML. O pedido HTTP têm de incluir um token de autorização. 

Os pontos finais regional texto conversão em voz são mostrados aqui. Utilize o um adequada à sua subscrição.

[!include[](includes/endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Especifique uma voz

Para especificar uma voz, utilize o `<voice>` [SSML](speech-synthesis-markup.md) etiquetas. Por exemplo:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Consulte [voices de reconhecimento de voz do texto para](supported-languages.md#text-to-speech) para uma lista dos voices disponíveis e os respetivos nomes.

## <a name="make-a-request"></a>Efetuar um pedido

É efetuado um pedido de reconhecimento de voz do texto para HTTP no modo de mensagem com o texto a ser ditas no corpo do pedido. O comprimento máximo de corpo do pedido HTTP é de 1024 carateres. O pedido tem de ter os seguintes cabeçalhos: 

Cabeçalho|Valores|Comentários
-|-|-
|`Content-Type` | `application/ssml+xml` | O formato de texto de entrada.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | O formato de áudio de saída.
|`User-Agent`   |Nome da aplicação | O nome da aplicação é necessário e tem de ter menos de 255 carateres.
| `Authorization`   | Token de autorização obtido através da sua chave de subscrição para o serviço de token de apresentação. Cada token é válido durante dez minutos. Consulte [REST APIs: autenticação](rest-apis.md#authentication).

> [!NOTE]
> Se a sua voz seleccionada e o formato de saída tiverem taxas de bits diferentes, áudio é resampled conforme necessário. voices 24khz não suportam `audio-16khz-16kbps-mono-siren` e `riff-16khz-16kbps-mono-siren` formatos de saída. 

Um exemplo de pedido é mostrado abaixo.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

O corpo da resposta com um Estado de 200 contém áudio no formato de saída especificado.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Se ocorrer um erro, são utilizados os códigos de estado abaixo. O corpo da resposta de erro também contém uma descrição do problema.

|Código|Descrição|Problema|
|-|-|-|
400 |Pedido Incorreto |Um parâmetro necessário está em falta, estar vazio ou é nulo. Em alternativa, o valor transmitido como um parâmetro necessário ou opcional é inválido. Um problema comum é um cabeçalho que é demasiado longo.
401|Não autorizado |O pedido não está autorizado. Certifique-se a chave de subscrição ou token é válido.
413|Entidade do pedido é demasiado grande|A entrada SSML é superior a 1024 carateres.
|502|Gateway incorrecto    | Problema de rede ou do lado do servidor. Também pode indicar cabeçalhos inválidos.

Para obter mais informações sobre o texto a API de REST de reconhecimento de voz, consulte [REST APIs](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer o reconhecimento de voz em c#](quickstart-csharp-windows.md)
