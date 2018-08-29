---
title: Utilizar texto em voz com serviços de voz
description: Saiba como utilizar o texto em voz no serviço de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d19b71542f032111bbd3d9c7f3fe246110377b5d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127591"
---
# <a name="use-text-to-speech-in-speech-service"></a>Utilizar "Texto em voz" no serviço de voz

O serviço de voz fornece uma funcionalidade de texto em voz através de um pedido HTTP simples. POSTAR o texto seja falado para o ponto final adequado e o serviço retorna um arquivo de áudio (`.wav`) que contém sintetizadas voz. Seu aplicativo, em seguida, pode utilizar este áudio como gosta.

O corpo da mensagem de pedido para texto em voz podem ser texto simples (ASCII ou UTF8) ou uma [SSML](speech-synthesis-markup.md) documento. Pedidos de texto sem formatação são ditas com uma voz de predefinição. Na maioria dos casos, queira usar um corpo SSML. O pedido HTTP tem de incluir uma [autorização](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#authentication) token. 

Os pontos de extremidade regional texto em voz são mostrados aqui. Utilize o que é adequado à sua subscrição.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Especifique uma voz

Para especificar uma voz, utilize o `<voice>` [SSML](speech-synthesis-markup.md) marca. Por exemplo:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Ver [vozes de texto em voz](supported-languages.md#text-to-speech) para obter uma lista das vozes disponíveis e seus nomes.

## <a name="make-a-request"></a>Fazer um pedido

É efetuado um pedido de texto em voz HTTP no modo de mensagem com o texto seja falado no corpo do pedido. O comprimento máximo de corpo do pedido HTTP é de 1024 caracteres. O pedido tem de ter os seguintes cabeçalhos: 

Cabeçalho|Valores|Comentários
-|-|-
|`Content-Type` | `application/ssml+xml` | O formato de texto de entrada.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | O formato de áudio de saída.
|`User-Agent`   |Nome da aplicação | O nome da aplicação é necessário e tem de ter menos de 255 carateres.
| `Authorization`   | Token de autorização obtido através da sua chave de subscrição para o serviço de token de apresentação. Cada token é válido durante dez minutos. Ver [REST APIs: autenticação](rest-apis.md#authentication).

> [!NOTE]
> Se sua voz selecionado e o formato de saída tiverem taxas de bits diferentes, o áudio é resampled conforme necessário. vozes de 24khz não suportam `audio-16khz-16kbps-mono-siren` e `riff-16khz-16kbps-mono-siren` formatos de saída. 

Um pedido de exemplo é mostrado abaixo.

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

O corpo de resposta com o estado de 200 contém áudio no formato de saída especificado.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Se ocorrer um erro, são utilizados os códigos de estado abaixo. O corpo da resposta para o erro também contém uma descrição do problema.

|Código|Descrição|Problema|
|-|-|-|
400 |Pedido Inválido |Um parâmetro necessário está em falta, vazios ou nulos. Em alternativa, o valor transmitido como um parâmetro obrigatório ou opcional é inválido. Um problema comum é um cabeçalho que é demasiado longo.
401|Não autorizado |O pedido não está autorizado. Certifique-se a chave de subscrição ou token é válido.
413|Entidade do pedido demasiado grande|A entrada SSML é superior a 1024 carateres.
|502|Gateway incorrecto    | Problema de rede ou do lado do servidor. Também pode indicar a cabeçalhos inválidos.

Para obter mais informações sobre o texto a API de REST de voz, consulte [REST APIs](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer a conversão de voz em C++](quickstart-cpp-windows.md)
- [Reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
- [Reconhecer a conversão de voz em Java](quickstart-java-android.md)
