---
title: O Azure Batch transcrição API
description: Amostras
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: f21973855ceb3a257627c147490ac50465c54020
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281944"
---
# <a name="batch-transcription"></a>Transcrição em lote

Transcrição de batch é ideal para casos de utilização com grandes quantidades de áudio. Permite que o desenvolvedor apontar para arquivos de áudio e voltar transcrições no modo assíncrono.

## <a name="batch-transcription-api"></a>API de transcrição de batch

A transcrição de Batch API possibilita a cenário acima. Ele oferece a conversão de voz assíncrona em transcrição de texto, juntamente com funcionalidades adicionais.

> [!NOTE]
> A API de transcrição do Batch é ideal para os centros de chamada que normalmente se acumular milhares de horas de áudio. A filosofia Fire & Forget da API facilita a transcrição de grande volume de gravações de áudio.

### <a name="supported-formats"></a>Formatos suportados

A API de transcrição do Batch tem como objetivo tornar-se a real para todos os cenários relacionados com o Centro de chamada offline e oferece suporte para formatos de todas as respetivas. Formatos suportados atualmente:

Nome| Canal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Estéreo  | 
WAV |   Mono   |
WAV |  Estéreo  |

Para fluxos de áudio estéreo, transcrição de Batch dividirá o canal do esquerda e direito durante a transcrição. Os dois ficheiros JSON com o resultado são todos criados a partir de um canal único. Os carimbos de data / por expressão permitem aos programadores criar uma transcrição final ordenada. O exemplo JSON seguinte mostra a saída de um canal.

```json
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
> A API de transcrição de Batch está a utilizar um serviço REST para pedir transcrições, o respetivo estado e resultados associados. A API pode ser utilizada em qualquer linguagem. A seguinte secção descreve como são utilizadas.

## <a name="authorization-token"></a>Token de autorização

Como com todas as funcionalidades do serviço de voz unificação de mensagens em fila, o utilizador tem de criar uma chave de subscrição do [portal do Azure](https://portal.azure.com). Além disso, uma chave de API tem de ser compradas a partir do Portal da voz. Os passos para gerar uma chave de API:

1. Inicie sessão no https://customspeech.ai.

2. Clique em subscrições.

3. Clique na opção `Generate API Key`.

    ![A vista de carregamento](media/stt/Subscriptions.jpg)

4. Copie e cole essa chave no código do cliente no exemplo abaixo.

> [!NOTE]
> Se planeia utilizar um modelo personalizado, em seguida, terá do ID desse modelo demasiado. Tenha em atenção que isto não é a implementação ou o ID de ponto final que encontrar na vista de detalhes do ponto final, mas o ID de modelo que pode ser obtida quando clica nos detalhes desse modelo

## <a name="sample-code"></a>Código de exemplo

Fazendo uso da API é bastante simples. Mostra o código de exemplo abaixo tem de ser personalizadas com uma chave de assinatura e uma chave de API, que por sua vez, permite ao desenvolvedor obter um token de portador, como o seguinte trecho de código:

```cs
    public static async Task<CrisClient> CreateApiV1ClientAsync(string username, string key, string hostName, int port)
        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;

            var tokenProviderPath = "/oauth/ctoken";
            var clientToken = await CreateClientTokenAsync(client, hostName, port, tokenProviderPath, username, key).ConfigureAwait(false);
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", clientToken.AccessToken);

            return new CrisClient(client);
        }
```

Assim que o token é obtido o desenvolvedor deve especificar o Uri de SAS que aponta para o arquivo de áudio a necessidade de transcrição. O restante do código simplesmente percorre o estado e apresenta os resultados.

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
                    "<your subscription key>", // Subscription Key
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
> A chave de subscrição mencionada no fragmento de código acima é a chave do recurso de Speech(Preview) que criar no portal do Azure. Chaves de obteve do recurso do serviço de voz personalizada não funcionará.


Observe a configuração assíncrona para a postagem de áudio e receber o estado de transcrição. O cliente criado é um cliente de Http do .NET. Há uma `PostTranscriptions` método para enviar os detalhes do arquivo de áudio e um `GetTranscriptions` método para receber os resultados. `PostTranscriptions` Retorna um identificador, e `GetTranscriptions` método consiste em utilizar este identificador para criar um identificador para obter o estado de transcrição.

O código de exemplo atual não especifique quaisquer modelos personalizados. O serviço irá utilizar os modelos de linha de base para fotografar os ficheiros. Se o utilizador pretende especificar os modelos, um transmitir o mesmo método modelIDs para o acústicos e o modelo de idioma. 

Se um não pretende utilizar a linha de base, tem de passar o Ids de modelo para modelos de acústica e idioma-um.

> [!NOTE]
> Para a linha de base de transcrição do utilizador não tem de declarar os pontos de extremidade dos modelos de linha de base. Se o usuário deseja usar modelos personalizados ele teria de fornecer suas IDs de pontos de extremidade como o [exemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Se o usuário deseja usar uma linha de base acústica com um modelo de idioma de linha de base, em seguida, ele só teria de declarar o ID de ponto final. do modelo personalizado Internamente, nosso sistema irá descobrir o modelo de linha de base do parceiro (ser ele acústico ou idioma) e usá-lo para satisfazer o pedido de transcrição.

### <a name="supported-storage"></a>Armazenamento suportadas

Atualmente o armazenamento apenas suportado é de Blobs do Azure.

## <a name="downloading-the-sample"></a>Baixar o exemplo

O exemplo apresentado aqui é no [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Uma transcrição de áudio requer, normalmente, um intervalo de tempo igual a duração do arquivo de áudio mais uma sobrecarga de 2 a 3 minutos.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
