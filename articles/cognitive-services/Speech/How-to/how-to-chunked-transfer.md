---
title: Como a sequência de áudio de transferência de partes | Microsoft Docs
description: Como utilizar trasfer fragmentada para enviar a sequência de áudio para o serviço de reconhecimento de voz
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 7d02340932dfc547893c4c40cbe08978b7b93756
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352171"
---
# <a name="chunked-transfer-encoding"></a>Codificação de transferência de partes

Para transcribe reconhecimento de voz para texto, o reconhecimento de voz Microsoft API permite-lhe para enviar áudio como um segmento de todo ou para chop áudio em pequenos segmentos. Para a transmissão de áudio eficiente e reduzindo a latência de transcription, é recomendado que utilize [codificação de transferência de partes](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) para transmitir áudio ao serviço. Noutras implementações poderão resultar numa maior latência percetível de utilizador. Para obter mais informações, consulte o [fluxos de áudio](../concepts.md#audio-streams) página.

> [!NOTE]
> Não pode carregar mais de 10 segundos de áudio em qualquer um pedido e a duração total do pedido não pode exceder os segundos 14.
> [!NOTE]
> Tem de especificar a transferência de partes codificação apenas se utilizar o [REST APIs](../GetStarted/GetStartedREST.md) para chamar o serviço de reconhecimento de voz. As aplicações que utilizam [bibliotecas de cliente](../GetStarted/GetStartedClientLibraries.md) não é necessário configurar a codificação de transferência de partes.

O código seguinte mostra como definir a codificação de transferência de segmentos e enviar um ficheiro de áudio que está a ser partes em segmentos de 1024 bytes.

```cs

HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```
