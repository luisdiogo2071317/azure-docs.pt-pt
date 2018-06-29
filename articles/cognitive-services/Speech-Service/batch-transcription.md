---
title: O Azure Batch Transcription API | Azure Microsoft Docs
description: Amostras
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 01bbf4ca19b0fb702aa76d5149fb0e38389fe455
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054828"
---
# <a name="batch-transcription"></a>Transcription do batch

Transcription do batch é ideal para casos de utilização com grandes quantidades de áudio. Permite que o programador apontar para ficheiros de áudio e voltar a transcriptions no modo assíncrono.

## <a name="batch-transcription-api"></a>Batch transcription API

A API de transcription Batch possibilita o cenário acima. Oferece assíncrono reconhecimento de voz para transcription de texto, juntamente com funcionalidades adicionais.

> [!NOTE]
> A API de transcription do Batch é ideal para os centros de chamada que, normalmente, acumular milhares de horas de áudio. O philosophy Fire & esquecer da API torna mais fácil transcribe grande volume de gravações de áudio.

### <a name="supported-formats"></a>Formatos suportados

A API de transcription Batch visa fiquem o facto de Alemanha para todos os cenários de relacionados com o Centro de chamada offline e oferecem suporte para formatos de todos os relacionados. Formatos atualmente suportados:

Nome| Canal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Stereo  | 
WAV |   Mono   |
WAV |  Stereo  |

Para fluxos de áudio stereo, Batch transcription irá dividir o canal esquerdo e direito durante o transcription. Os dois ficheiros JSON com o resultado cada criados a partir de um canal único. Os carimbos por utterance ativar o programador criar um transcript final ordenada. O exemplo JSON seguinte mostra a saída de um canal.

    ```
       {
        "recordingsUrl": "https://mystorage.blob.core.windows.net/cris-e2e-datasets/TranscriptionsDataset/small_sentence.wav?st=2018-04-19T15:56:00Z&se=2040-04-21T15:56:00Z&sp=rl&sv=2017-04-17&sr=b&sig=DtvXbMYquDWQ2OkhAenGuyZI%2BYgaa3cyvdQoHKIBGdQ%3D",
        "resultsUrls": {
            "channel_0": "https://mystorage.blob.core.windows.net/bestor-87a0286f-304c-4636-b6bd-b3a96166df28/TranscriptionData/24265e4c-e459-4384-b572-5e3e7795221f?sv=2017-04-17&sr=b&sig=IY2qd%2Fkgtz2PwRe2C88BphH4Hv%2F1VCb1UVJ33xsw%2BEY%3D&se=2018-04-23T14:48:24Z&sp=r"
        },
        "statusMessage": "None.",
        "id": "0bb95356-ff06-469d-acc7-81f9144a269a",
        "createdDateTime": "2018-04-20T14:11:57.167",
        "lastActionDateTime": "2018-04-20T14:12:54.643",
        "status": "Succeeded",
        "locale": "en-US"
    },
    ```

> [!NOTE]
> A API de transcription Batch está a utilizar um serviço REST para pedir transcriptions, respetivo estado e resultados associados. Este é baseada no .NET e não tem quaisquer dependências externas. A seguinte secção descreve como são utilizadas.

## <a name="authorization-token"></a>Token de autorização

Como com todas as funcionalidades do serviço de reconhecimento de voz unificado, o utilizador tem de criar uma chave de subscrição do [portal do Azure](https://portal.azure.com). Além disso, uma chave de API tem de ser obtido a partir do Portal de reconhecimento de voz. Os passos para gerar uma chave de API:

1. Inicie sessão no https://customspeech.ai.

2. Clique em subscrições.

3. Clique na opção de `Generate API Key`.

    ![A vista de carregamento](media/stt/Subscriptions.jpg)

4. Copie e cole essa chave com o código de cliente no exemplo abaixo.

> [!NOTE]
> Se planeia utilizar um modelo personalizado, em seguida, terá do ID desse modelo demasiado. Tenha em atenção que isto não é a implementação ou o ID de ponto final que encontrará na vista de detalhes do ponto final, mas o ID de modelo que pode obter ao clicar nos detalhes desse modelo

## <a name="sample-code"></a>Código de exemplo

Efetuar a utilizar a API é bastante diretamente reencaminhar. O código de exemplo abaixo tem de ser personalizadas com uma chave de subscrição e uma chave de API.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating a Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscripition key>", // Subscription Key
                    new Uri("<SAS URI to your file>")).ConfigureAwait(false);

            var transcription = await client.GetTranscriptionAsync(newLocation).ConfigureAwait(false);

            while (true)
            {
                transcription = await client.GetTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                if (transcription.Status == "Failed" || transcription.Status == "Succeeded")
                {
                    Console.WriteLine("Transcription complete!");

                    if (transcription.Status == "Succeeded")
                    {
                        var resultsUri = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri, filename);

                        var results = File.ReadAllText(filename);
                        Console.WriteLine(results);
                    }

                    await client.DeleteTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                    break;
                }
                else
                {
                    Console.WriteLine("Transcription status: " + transcription.Status);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.ReadLine();
        }
```

> [!NOTE]
> A chave de subscrição foi mencionada no fragmento de código acima é a chave de recurso Speech(Preview) que criar no portal do Azure. Chaves obtidas a partir do recurso de serviço de reconhecimento de voz personalizadas não irão funcionar.


Tenha em atenção a configuração para publicar áudio e a receção de estado de transcription assíncrona. O cliente criado é um cliente NET Http. Não existe um `PostTranscriptions` método para enviar os detalhes do ficheiro de áudio e um `GetTranscriptions` método para receber os resultados. `PostTranscriptions` Devolve um identificador e `GetTranscriptions` método está a utilizar este identificador para criar um identificador para obter o estado de transcription.

O código de exemplo atual não especifique quaisquer modelos personalizados. O serviço irá utilizar os modelos de linha de base para transcribing os ficheiros. Se o utilizador pretende especificar os modelos, um pode passar o mesmo método modelIDs para o acústica e o modelo de linguagem. 

Se um não pretende utilizar a linha de base, um tem de passar os Ids de modelo para modelos acústica e idiomas.

> [!NOTE]
> Para a linha de base transcription o utilizador não tem de declarar os pontos finais dos modelos de linha de base. Se o utilizador pretenda utilizar modelos personalizados ele teria de fornecer os respetivos IDs de pontos finais, como o [exemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Se o utilizador pretenda utilizar uma linha de base acústica com um modelo de linguagem de linha de base, em seguida, ele só teria de declarar o ID de ponto final. o modelo personalizado Internamente o nosso sistema descobrir o modelo de linha de base de parceiro (ser acústica-lo ou idioma) e utilize-a para fullfill o pedido de transcription.

### <a name="supported-storage"></a>Armazenamento suportadas

Atualmente, o armazenamento apenas suportado é blob do Azure.

## <a name="downloading-the-sample"></a>Transferir o exemplo

O exemplo apresentado aqui está no [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Um transcription áudio requer, normalmente, um intervalo de tempo igual a duração do ficheiro de áudio, uma sobrecarga de 2 a 3 minutos.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
