---
title: Conceitos de AudioInputStream
description: Uma descrição geral das funcionalidades da AudioInputStream API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: b3e12fbc616c8d67b557102c6094467e119a23f1
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281910"
---
# <a name="about-the-audio-input-stream-api"></a>API do stream sobre a entrada de áudio

O **Stream de entrada de áudio** API fornece uma maneira para transmitir fluxos de áudio para os reconhecedores tenham em vez de utilizar o microfone ou o ficheiro de entrada de APIs.

## <a name="api-overview"></a>Descrição geral da API

A API utiliza dois componentes, o `AudioInputStream` (os dados não processados áudio) e o `AudioInputStreamFormat`.

O `AudioInputStreamFormat` define o formato dos dados de áudio. Pode ser comparada ao padrão `WAVEFORMAT` estrutura para ficheiros de onda no Windows.

  - `FormatTag`

    O formato do áudio. O SDK de voz atualmente apenas suporta `format 1` (PCM - little-endian).

  - `Channels`

    O número de canais. O serviço de voz atual suporta material de áudio (mono) de canal de apenas um.

  - `SamplesPerSec`

    A taxa de amostragem. Uma gravação de microfone típica tem 16000 exemplos por segundo.

  - `AvgBytesPerSec`

    Média de bytes por segundo, calculada como `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Média de bytes por segundo pode ser diferente para os fluxos de áudio que utilizam as velocidades de transmissão de variável.

  - `BlockAlign`

    O tamanho de um único quadro, calculada como `Channels * ceil(wBitsPerSample, 8)`. Devido a preenchimento, o valor real pode ser superior a este valor.

  - `BitsPerSample`

    Os bits por amostra. Um fluxo de áudio típico usa 16 bits por amostra (qualidade de CD).

O `AudioInputStream` classe base será substituída pela sua placa de fluxos personalizados. Esta placa tem de implementar essas funções:

   - `GetFormat()`

     Esta função é chamada para obter o formato do fluxo de áudio. Obtém um ponteiro para o buffer de AudioInputStreamFormat.

   - `Read()`

     Esta função é chamada para obter dados de transmissão de áudio. Um parâmetro é um ponteiro para o buffer para copiar os dados de áudio em. O segundo parâmetro é o tamanho do buffer. A função devolve o número de bytes copiados para a memória intermédia. Um valor de retorno `0` indica o fim do fluxo.

   - `Close()`

     Esta função é chamada para fechar o fluxo de áudio.

## <a name="usage-examples"></a>Exemplos de utilização

Em geral, as etapas a seguir são envolvidas ao utilizar fluxos de entrada de áudio:

  - Identifica o formato do fluxo de áudio. O formato tem de ser suportado pelo SDK e o serviço de voz. Atualmente é suportada a seguinte configuração:

    Etiqueta de um formato de áudio (PCM), um canal, 16000 exemplos por segundo, 32000 bytes por segundo, dois blocos alinhar (incluindo o preenchimento de um exemplo de 16 bits), de 16 bits por amostra

  - Certifique-se de que seu código pode fornecer os dados de áudio não processados sobre as especificações identificados acima. Se os dados de origem de áudio não corresponderem os formatos suportados, o áudio tem de ser transcodificados no formato necessário.

  - Derivar a classe de fluxos de entrada de áudio personalizados de `AudioInputStream`. Implementar o `GetFormat()`, `Read()`, e `Close()` operação. A assinatura de função exata é dependente de idioma, mas o código será semelhante ao seguinte exemplo de código::

    ```
     public class ContosoAudioStream : AudioInputStream {
        ContosoConfig config;

        public ContosoAudioStream(const ContosoConfig& config) {
            this.config = config;
        }

        public void GetFormat(AudioInputStreamFormat& format) {
            // returns format data to the caller.
            // e.g. format.FormatTag = config.XXX;
            // ...
        }

        public size_t Read(byte *buffer, size_t size) {
            // returns audio data to the caller.
            // e.g. return read(config.YYY, buffer, size);
        }

        public void Close() {
            // close and cleanup resources.
        }
     };
    ```

  - Utilize o seu fluxo de entrada de áudio:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    var result = await recognizer.RecognizeAsync();

    var text = result.GetText();

    // In some languages you need to delete the stream explicitly.
    // delete contosoStream;
    ```

  - Em alguns idiomas, o `contosoStream` têm de ser eliminadas explicitamente após a conclusão do reconhecimento. Não é possível libertar o AudioStream antes da entrada completa é de leitura. Num cenário com `StopContinuousRecognitionAsync` e `StopContinuousRecognitionAsync` requer um conceito ilustrado neste exemplo:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    await recognizer.StartContinuousRecognitionAsync();

    // ERROR: do not delete the contosoStream before ending recognition!
    // delete contosoStream;

    await recognizer.StopContinuousRecognitionAsync();

    // OK: Safe to delete the contosoStream.
    // delete contosoStream;
    ```

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
