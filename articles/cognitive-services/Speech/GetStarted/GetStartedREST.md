---
title: Começar com a API de reconhecimento de voz do Microsoft utilizando REST | Microsoft Docs
description: Utilize REST para aceder a API de reconhecimento de voz no Microsoft serviços cognitivos converter áudio ditas texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 53785cdfd75c23910802f2be20e6305817b3b097
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352268"
---
# <a name="get-started-with-speech-recognition-by-using-the-rest-api"></a>Começar com o reconhecimento de voz, utilizando a API de REST

Com o serviço de reconhecimento de voz baseado na nuvem, pode desenvolver aplicações utilizando a API REST para converter áudio ditas em texto.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

> [!IMPORTANT]
>* Obter uma chave de subscrição. Antes de poder aceder a API REST, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).
>
>* Utilize a sua chave de subscrição. Nos exemplos seguintes de REST, substitua YOUR_SUBSCRIPTION_KEY sua própria chave de subscrição. 
>
>* Consulte o [autenticação](../how-to/how-to-authentication.md) página para saber como obter uma chave de subscrição.

### <a name="prerecorded-audio-file"></a>Ficheiro de áudio prerecorded

Neste exemplo, vamos utilizar um ficheiro de áudio registado para ilustrar como utilizar a API REST. Registe um ficheiro de áudio de si próprio indicar uma expressão de curta. Por exemplo, imagine "Qual é a meteorologia como hoje?" ou "Encontrar filmes funny para ver". O API de reconhecimento de voz também suporta a entrada de microfone externo.

> [!NOTE]
> O exemplo requer que áudio é registado como um ficheiro WAV com **PCM único canal (mono), 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Criar um pedido de reconhecimento e enviá-lo para o serviço de reconhecimento de voz

O passo seguinte para reconhecimento de voz é para enviar um pedido POST para os pontos finais de HTTP de reconhecimento de voz com o cabeçalho de pedido adequado e corpo.

### <a name="service-uri"></a>URI de serviço

O serviço de reconhecimento de voz URI está definido com base no [modos de reconhecimento](../concepts.md#recognition-modes) e [idiomas reconhecimento](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` Especifica o modo de reconhecimento e tem de ser um dos seguintes valores: `interactive`, `conversation`, ou `dictation`. É um caminho de recurso necessário no URI. Para obter mais informações, consulte [modos de reconhecimento](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` é um parâmetro necessário na cadeia de consulta. Define o idioma de destino para a conversão de áudio: por exemplo, `en-US` para inglês (Estados Unidos). Para obter mais informações, consulte [idiomas reconhecimento](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` é um parâmetro opcional na cadeia de consulta. Os valores permitidos são `simple` e `detailed`. Por predefinição, o serviço devolve resultados no `simple` formato. Para obter mais informações, consulte [formato de saída](../concepts.md#output-format).

Alguns exemplos de serviço URIs estão listados na seguinte tabela.

| Modo de reconhecimento  | Idioma | Formato de saída | URI de serviço |
|---|---|---|---|
| `interactive` | pt-BR | Predefinição | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | pt-PT | Detalhado |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed |
| `dictation` | FR-FR | Simples | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR&format=simple |

> [!NOTE]
> O URI do serviço é necessário apenas quando a sua aplicação utiliza a REST APIs para chamar o serviço de reconhecimento de voz. Se utilizar um do [bibliotecas de cliente](GetStartedClientLibraries.md), normalmente, não precisa de saber qual URI é utilizado. As bibliotecas de cliente podem utilizar outro serviço URIs, que são aplicáveis apenas para uma biblioteca de cliente específico. Para obter mais informações, consulte a biblioteca de clientes à sua escolha.

### <a name="request-headers"></a>Cabeçalhos do pedido

Os seguintes campos tem de ser definidos no cabeçalho do pedido:

- `Ocp-Apim-Subscription-Key`: Sempre que chamar o serviço, tem de passar a chave de subscrição no `Ocp-Apim-Subscription-Key` cabeçalho. Serviço de reconhecimento de voz também suporta a transmissão autorização tokens em vez de chaves de subscrição. Para obter mais informações, consulte [autenticação](../How-to/how-to-authentication.md).
- `Content-type`: O `Content-type` campo descreve o formato e codec do fluxo de áudio. Atualmente, apenas ficheiro WAV PCM Mono 16000 codificação é suportada. O valor de tipo de conteúdo para este formato é `audio/wav; codec=audio/pcm; samplerate=16000`.

O `Transfer-Encoding` campo é opcional. Se definir este campo para `chunked`, pode chop áudio em pequenos segmentos. Para obter mais informações, consulte [partes transferência](../How-to/how-to-chunked-transfer.md).

Segue-se um cabeçalho de pedido de exemplo:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="send-a-request-to-the-service"></a>Enviar um pedido para o serviço

O exemplo seguinte mostra como enviar um pedido de reconhecimento de voz para pontos finais REST de reconhecimento de voz. Utiliza o `interactive` modo de reconhecimento.

> [!NOTE]
> Substitua `YOUR_AUDIO_FILE` com o caminho para o ficheiro de áudio prerecorded. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua própria chave de subscrição.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authorization token returned by the token service.
$RecoRequestHeader = @{
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY';
  'Transfer-Encoding' = 'chunked';
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

O exemplo utiliza o curl no Linux com bash. Se não estiver disponível na sua plataforma, poderá ter de instalar o curl. O exemplo também funciona em Cygwin no Windows, Git Bash, zsh e outros shells.

> [!NOTE]
> Manter o `@` antes do nome do ficheiro de áudio quando substituir `YOUR_AUDIO_FILE` com o caminho para o ficheiro de áudio prerecorded, dado que indica que o valor de `--data-binary` é um nome de ficheiro em vez de dados.

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

// Send an audio file by 1024 byte chunks
using (FileStream fs = new FileStream(YOUR_AUDIO_FILE, FileMode.Open, FileAccess.Read))
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

---

## <a name="process-the-speech-recognition-response"></a>Processo de resposta de reconhecimento de voz

Após o processamento do pedido, o serviço de reconhecimento de voz devolve os resultados numa resposta como formato JSON.

> [!NOTE]
> Se o código anterior devolve um erro, consulte o artigo [resolução de problemas](../troubleshooting.md) para localizar a causa.

O fragmento de código seguinte mostra um exemplo de como pode ler a resposta do fluxo.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[curl](#tab/curl)

Neste exemplo, o curl devolve diretamente a mensagem de resposta de uma cadeia. Se pretender mostrá-lo no formato JSON, pode utilizar ferramentas adicionais, por exemplo, jq.

```
curl -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE | jq
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
/*
* Get the response from the service.
*/
Console.WriteLine("Response:");
using (WebResponse response = request.GetResponse())
{
    Console.WriteLine(((HttpWebResponse)response).StatusCode);

    using (StreamReader sr = new StreamReader(response.GetResponseStream()))
    {
        responseString = sr.ReadToEnd();
    }

    Console.WriteLine(responseString);
    Console.ReadLine();
}
```

---

O exemplo seguinte é uma resposta JSON:

```json
OK
{
  "RecognitionStatus": "Success",
  "Offset": 22500000,
  "Duration": 21000000,
  "NBest": [{
    "Confidence": 0.941552162,
    "Lexical": "find a funny movie to watch",
    "ITN": "find a funny movie to watch",
    "MaskedITN": "find a funny movie to watch",
    "Display": "Find a funny movie to watch."
  }]
}
```

## <a name="limitations"></a>Limitações

A API REST tem algumas limitações:

- Suporta apenas até ao 15 segundos a sequência de áudio.
- Não suporta resultados intermédios durante reconhecimento. Os utilizadores recebem apenas o resultado final reconhecimento.

Para remover estas limitações, utilize o reconhecimento de voz [bibliotecas de cliente](GetStartedClientLibraries.md). Ou pode trabalhar diretamente com o [protocolo WebSocket de reconhecimento de voz](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Passos seguintes

- Para ver como utilizar a API de REST em c#, Java, etc., veja estes [aplicações de exemplo](../samples.md).
- Para localizar e corrigir os erros, consulte [resolução de problemas](../troubleshooting.md).
- Para utilizar funcionalidades mais avançadas, consulte como começar com o reconhecimento de voz [bibliotecas de cliente](GetStartedClientLibraries.md).

### <a name="license"></a>Licença

Todos os cognitivos de serviços de SDKs e ferramentas são licenciadas com a licença MIT. Para obter mais informações, consulte [licença](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
