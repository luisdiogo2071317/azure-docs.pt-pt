---
title: Início rápido c# para os serviços do Azure cognitivos, Microsoft tradutor voz API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de reconhecimento de voz do Microsoft tradutor nos serviços cognitivos Microsoft no Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: 6489a0be72cedffdfa4f7021f889b2d39c1c358f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352196"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-c"></a>Início rápido para reconhecimento de voz do Microsoft tradutor API com c# 
<a name="HOLTop"></a>

Este artigo mostra como utilizar a API de reconhecimento de voz do Microsoft tradutor traduzir palavras ditas um ficheiro WAV.

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A edição da Comunidade gratuito irá funcionar.)

Irá necessitar de um ficheiro WAV com o nome "speak.wav" na mesma pasta que o executável que compilar a partir do código abaixo. Deve ter este ficheiro WAV PCM padrão, formato mono de 16 bits, 16kHz. Pode obter esses ficheiros WAV do [tradutor texto enunciar API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de reconhecimento de voz do Microsoft tradutor**. Precisa de uma chave de subscrição paga do seu [dashboard do Azure](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Converter o reconhecimento de voz

O seguinte código traduz reconhecimento de voz de um idioma para outro.

1. Crie um novo projeto c# no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `key` valor com uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```csharp
using System;
using System.IO;
using System.Net.WebSockets;
using System.Text;
using System.Threading;
using System.Threading.Tasks;

namespace TranslateSpeechQuickStart
{
    class Program
    {
        static string host = "wss://dev.microsofttranslator.com";
        static string path = "/speech/translate";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        async static Task Send (ClientWebSocket client, string input_path)
        {
            var audio = File.ReadAllBytes(input_path);
            var audio_out_buffer = new ArraySegment<byte>(audio);
            Console.WriteLine("Sending audio.");
            await client.SendAsync(audio_out_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            /* Make sure the audio file is followed by silence.
             * This lets the service know that the audio input is finished. */
            var silence = new byte[3200000];
            var silence_buffer = new ArraySegment<byte>(silence);
            await client.SendAsync(silence_buffer, WebSocketMessageType.Binary, true, CancellationToken.None);

            Console.WriteLine("Done sending.");
            await client.CloseAsync(WebSocketCloseStatus.NormalClosure, "", CancellationToken.None);
        }

        async static Task Receive(ClientWebSocket client, string output_path)
        {
            var inbuf = new byte[102400];
            var segment = new ArraySegment<byte>(inbuf);
            var stream = new FileStream(output_path, FileMode.Create);

            Console.WriteLine("Awaiting response.");
            while (client.State == WebSocketState.Open)
            {
                var result = await client.ReceiveAsync(segment, CancellationToken.None);
                switch (result.MessageType)
                {
                    case WebSocketMessageType.Close:
                        Console.WriteLine("Received close message. Status: " + result.CloseStatus + ". Description: " + result.CloseStatusDescription);
                        await client.CloseAsync(WebSocketCloseStatus.NormalClosure, string.Empty, CancellationToken.None);
                        break;
                    case WebSocketMessageType.Text:
                        Console.WriteLine("Received text.");
                        Console.WriteLine(Encoding.UTF8.GetString(inbuf).TrimEnd('\0'));
                        break;
                    case WebSocketMessageType.Binary:
                        Console.WriteLine("Received binary data: " + result.Count + " bytes.");
                        stream.Write(inbuf, 0, result.Count);
                        break;
                }
            }

            stream.Close();
            stream.Dispose();
        }

        async static void TranslateSpeech()
        {
            var client = new ClientWebSocket();
            client.Options.SetRequestHeader ("Ocp-Apim-Subscription-Key", key);

            string from = "en-US";
            string to = "it-IT";
            string features = "texttospeech";
            string voice = "it-IT-Elsa";
            string api = "1.0";

            string input_path = "speak.wav";
            string output_path = "speak2.wav";

            string uri = host + path +
                "?from=" + from +
                "&to=" + to +
                "&api-version=" + api +
                "&features=" + features +
                "&voice=" + voice;

            Console.WriteLine("uri: " + uri);
            Console.WriteLine("Opening connection.");
            await client.ConnectAsync(new Uri(uri), CancellationToken.None);
            Console.WriteLine("Connection open.");
            Task.WhenAll(Send(client, input_path), Receive(client, output_path)).Wait();
        }

        static void Main()
        {
            TranslateSpeech();
            Console.ReadLine();
        }

    }
}
```

**Converter a resposta de reconhecimento de voz**

Um resultado com êxito é a criação de um ficheiro com o nome "speak2.wav". O ficheiro contém a conversão de palavras ditas "speak.wav".

[Voltar ao início](#HOLTop)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de reconhecimento de voz tradutor](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de reconhecimento de voz tradutor](../overview.md)
[referência da API](http://docs.microsofttranslator.com/speech-translate.html)
