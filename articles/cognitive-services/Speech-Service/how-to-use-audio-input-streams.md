---
title: Conceitos de AudioInputStream | Microsoft Docs
description: Uma descrição geral das funcionalidades da AudioInputStream API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
manager: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: 528356473c4221a815fa68cbec3426866c4cbd23
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356295"
---
# <a name="about-the-audio-input-stream-api"></a>Sobre a entrada de áudio transmitido em fluxo API

O **fluxo de entrada de áudio** API fornece uma forma para transmitir os fluxos de áudio em recognizers em vez de utilizar o microfone ou o ficheiro de entrada de APIs.

## <a name="api-overview"></a>Descrição geral da API

A API utiliza dois componentes, o `AudioInputStream` (os dados não processados áudio) e o `AudioInputStreamFormat`.

O `AudioInputStreamFormat` define o formato dos dados de áudio. Este pode ser comparado com a norma `WAVEFORMAT` estrutura para wave os ficheiros no Windows.

  - `FormatTag`

    O formato de áudio. O SDK de reconhecimento de voz atualmente suporta apenas `format 1` (PCM - little endian).

  - `Channels`

    O número de canais. O serviço de reconhecimento de voz atual suporta material de áudio (mono) apenas um canal.

  - `SamplesPerSec`

    A frequência de amostragem. Gravar um microfone típico tem 16000 amostras por segundo.

  - `AvgBytesPerSec`

    Média de bytes por segundo, calculados como `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. Médios bytes por segundo podem ser diferentes para fluxos de áudio que utilizam a variável de forma.

  - `BlockAlign`

    O tamanho de um único período, calculados como `Channels * ceil(wBitsPerSample, 8)`. Devido a preenchimento, o valor real pode ser superior a este valor.

  - `BitsPerSample`

    Bits por exemplo. Um fluxo típico de áudio utiliza 16 bits por exemplo (CD qualidade).

O `AudioInputStream` classe base irá ser substituída pelo seu adaptador de fluxo personalizado. Esta placa tem de implementar estas funções:

   - `GetFormat()`

     Esta função é chamada para obter o formato do fluxo de áudio. Obtém um ponteiro para a memória intermédia AudioInputStreamFormat.

   - `Read()`

     Esta função é chamada para obter dados a partir da sequência de áudio. Um parâmetro. existe um ponteiro para a memória intermédia para copiar os dados de áudio em. O segundo parâmetro é o tamanho da memória intermédia. A função devolve o número de bytes copiados para a memória intermédia. Um valor de retorno da `0` indica o fim da sequência.

   - `Close()`

     Esta função é chamada para fechar a sequência de áudio.

## <a name="usage-examples"></a>Exemplos de utilização

Em geral, os seguintes passos envolvidos ao utilizar sequências de entrada de áudio:

  - Identifique o formato do fluxo de áudio. O formato tem de ser suportado pelo SDK e o serviço de reconhecimento de voz. Atualmente, a seguinte configuração é suportada:

    Etiqueta de um formato de áudio (PCM), um canal, 16000 amostras por segundo, 32000 bytes por segundo, dois blocos alinhar (incluindo o preenchimento para um exemplo de 16 bits), 16 bits por exemplo

  - Certifique-se de que o seu código pode fornecer os dados de áudio não processados sobre as especificações identificados acima. Se os dados de origem de áudio não correspondem os formatos suportados, áudio tem de ser transcodificação para o formato necessário.

  - Derivar a classe do fluxo de entrada de áudio personalizado do `AudioInputStream`. Implementar o `GetFormat()`, `Read()`, e `Close()` operação. A assinatura da função exato é dependente de idioma, mas o código será semelhante ao seguinte exemplo de código:

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

  - Em alguns idiomas, o `contosoStream` tem de ser eliminado explicitamente após a conclusão de reconhecimento. Não é possível libertar o AudioStream antes de leitura de entrada completa. Num cenário com `StopContinuousRecognitionAsync` e `StopContinuousRecognitionAsync` requer um conceito ilustrado no seguinte exemplo:

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

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)
