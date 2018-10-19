---
title: Transcrição de lote do Azure API
description: Amostras
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: c6912b45bc62ce9492e8e33bd1ffd8e7147b9d17
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427792"
---
# <a name="batch-transcription"></a>Transcrição em lote

Transcrição de batch é ideal se tiver grandes quantidades de áudio no armazenamento. Utilizar a nossa API Rest, que pode apontar para arquivos de áudio pelo URI de SAS e assincronamente receber transcrições.

## <a name="batch-transcription-api"></a>API de transcrição de batch

A transcrição de Batch API oferece assíncrona de voz em transcrição de texto, juntamente com funcionalidades adicionais. É uma API de REST expor métodos para:

1. Criar pedidos de processamento de batch

2. Estado da Consulta 

3. Baixar o trnascriptions

> [!NOTE]
> A API de transcrição do Batch é ideal para os centros de chamada, que normalmente se acumular milhares de horas de áudio. A API é orientada por uma filosofia "disparar e esquecer", que facilita a transcrição de grande volume de gravações de áudio.

### <a name="supported-formats"></a>Formatos suportados

A API de transcrição do Batch suporta os seguintes formatos:

Nome| Canal  |
----|----------|
MP3 |   Mono   |   
MP3 |  Estéreo  | 
WAV |   Mono   |
WAV |  Estéreo  |

Para fluxos de áudio estéreo, transcrição de Batch divide o canal do esquerda e direito durante a transcrição. Os dois ficheiros JSON com o resultado são todos criados a partir de um canal único. Os carimbos de data / por expressão permitem aos programadores criar uma transcrição final ordenada. O exemplo JSON seguinte mostra a saída de um canal.

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
> A API de transcrição de Batch está a utilizar um serviço REST para pedir transcrições, o respetivo estado e resultados associados. Pode utilizar a API em qualquer linguagem. A seguinte secção descreve como são utilizadas.

## <a name="authorization-token"></a>Token de autorização

Com todas as funcionalidades do serviço de voz, criar uma chave de subscrição do [portal do Azure](https://portal.azure.com) seguintes nossa [guia de introdução](get-started.md). Se pretender obter transcrições de nossos modelos de linha de base, em seguida, isso é tudo o que precisa fazer. 

Se pretender personalizar e usando um modelo personalizado, em seguida, terá de adicionar esta chave subscritpion para o portal de voz personalizada da seguinte forma:

1. Inicie sessão no [conversão de voz personalizada](https://customspeech.ai).

2. Selecione **Subscrições**.

3. Selecione **ligar a subscrição existente**.

4. Adicionar a chave de subscrição e um alias na vista de que é apresentado

    ![Página de captura de ecrã de subscrições de voz personalizada](media/stt/Subscriptions.jpg)

5. Copie e cole essa chave no código do cliente no exemplo a seguir.

> [!NOTE]
> Se planeia utilizar um modelo personalizado, terá também o ID desse modelo. Tenha em atenção que este não é o ID de ponto final que encontrar na vista de detalhes do ponto final. É o ID de modelo que pode recuperar ao selecionar os detalhes desse modelo.

## <a name="sample-code"></a>Código de exemplo

Personalize o código de exemplo seguinte, com uma chave de assinatura e uma chave de API. Isto permite-lhe obter um token de portador.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);
         
            return new CrisClient(client);
        }
```

Depois de obter o token, tem de especificar o URI de SAS que aponta para o arquivo de áudio a necessidade de transcrição. O restante do código percorre o estado e apresenta os resultados. Um iria configurar inicialmente a chave, região, modelos de usar e o SA. como mostra o trecho de código abaixo. Isso é seguido pela Instanciação do cliente e o pedido POST. 

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // SAS URI 
            private const string RecordingsBlobUri = "some SAS URI";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("some guid");
            private static Guid AdaptedLanguageId = new Guid("some guid");

            // Creating a Batch transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Agora que o pedido se tornou o usuário pode consultar e transferir os resultados de transcrição tal como mostra o trecho de código.

```cs
  
            // get all transcriptions for the user
            transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            // for each transcription in the list we check the status
            foreach (var transcription in transcriptions)
            {
                switch(transcription.Status)
                {
                    case "Failed":
                    case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                        if (!createdTranscriptions.Contains(transcription.Id))
                        {
                            // not creted form here, continue
                            continue;
                        }
                            
                        completed++;
                            
                        // if the transcription was successfull, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succedded. Results: ");
                            Console.WriteLine(results);
                        }
                    
                    break;
                    case "Running":
                    running++;
                     break;
                    case "NotStarted":
                    notStarted++;
                    break;
                    
                    }
                }
            }
        }
```

Nosso [documento Swagger](https://westus.cris.ai/swagger/ui/index) fornece detalhes completos sobre as chamadas acima. O exemplo completo mostrado aqui é no [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> No código anterior, a chave de subscrição é o recurso de fala que criar no portal do Azure. Teclas de obtido a partir do recurso de serviço de voz personalizada não funciona.

Observe a configuração assíncrona para a postagem de áudio e receber o estado de transcrição. O cliente criado é um cliente de Http do .NET. Há uma `PostTranscriptions` método para enviar os detalhes do arquivo de áudio e um `GetTranscriptions` método para receber os resultados. `PostTranscriptions` Retorna um identificador, e `GetTranscriptions` utiliza esse identificador para criar um identificador para obter o estado de transcrição.

O código de exemplo atual não especifique quaisquer modelos personalizados. O serviço utiliza os modelos de linha de base para fotografar o ficheiro ou ficheiros. Para especificar os modelos, pode passar no mesmo método os IDs de modelo para o acústicos e o modelo de idioma. 

Se não pretender utilizar a linha de base, tem de passar os Ids de modelo para modelos de acústica e idioma.

> [!NOTE]
> Para transcrição de linha de base, não é necessário declarar os pontos de extremidade dos modelos de linha de base. Se pretender utilizar modelos personalizados, fornecer suas IDs de pontos de extremidade como o [exemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Se pretender utilizar uma linha de base acústica com um modelo de idioma de linha de base, só tem de declarar o ID de ponto final. do modelo personalizado Microsoft Deteta o modelo de linha de base de parceiro (ser ele acústico ou linguagem) e o utiliza para satisfazer o pedido de transcrição.

### <a name="supported-storage"></a>Armazenamento suportadas

Atualmente o armazenamento apenas suportado é o armazenamento de Blobs do Azure.

## <a name="downloading-the-sample"></a>Baixar o exemplo

O exemplo mostrado aqui é no [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Normalmente, uma transcrição de áudio requer um intervalo de tempo igual a duração do arquivo de áudio, mais uma sobrecarga de 2 a 3 minutos.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
