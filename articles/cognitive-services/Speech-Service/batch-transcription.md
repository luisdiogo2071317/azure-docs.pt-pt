---
title: Utilizar a API de transcrição do Azure Batch
titlesuffix: Azure Cognitive Services
description: Exemplos para fotografar grandes volumes de conteúdo de áudio.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: 8a180dfada9da92e0b8ed69373a20602b3b0a177
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495585"
---
# <a name="why-use-batch-transcription"></a>Por que usar a transcrição de Batch?

Transcrição de batch é ideal se tiver grandes quantidades de áudio no armazenamento. Ao utilizar a API de REST dedicada, que pode apontar para arquivos de áudio por uma assinatura de acesso partilhado (SAS) URI e assincronamente receber transcrições.

## <a name="the-batch-transcription-api"></a>A API de transcrição do Batch

A API de transcrição do Batch oferece assíncrona transcrição de voz em texto, juntamente com funcionalidades adicionais. É uma API REST que expõe métodos para:

1. Criar pedidos de processamento de batch
1. Estado de consulta 
1. Baixar o transcrições

> [!NOTE]
> A API de transcrição do Batch é ideal para os centros de chamada, que normalmente se acumular milhares de horas de áudio. A API é orientada por uma filosofia "disparar e esquecer", que facilita a transcrição de grandes volumes de gravações de áudio.

### <a name="supported-formats"></a>Formatos suportados

A API de transcrição do Batch suporta os seguintes formatos:

| Formato | Codec | Velocidade de transmissão | Taxa de exemplo |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 8 ou 16 estéreo mono, kHz, |
| MP3 | PCM | 16-bit | 8 ou 16 estéreo mono, kHz, |
| OGG | OPUS | 16-bit | 8 ou 16 estéreo mono, kHz, |

> [!NOTE]
> A API de transcrição do Batch requer uma chave de S0 (camada de pagar). Ele não funciona com uma chave gratuita (f0).

Para fluxos de áudio estéreo, a API de transcrição do Batch divide o canal do esquerda e direito durante a transcrição. Os dois ficheiros JSON com o resultado são todos criados a partir de um canal único. Os carimbos de data / por expressão permitem aos programadores criar uma transcrição final ordenada. O exemplo JSON seguinte mostra a saída de um canal, includuing propriedades para configurar o filtro de linguagem inapropriada e o modelo de pontuação.

```json
{
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "models": [],
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "description": "An optional description of the transcription.",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic"
  },
```

> [!NOTE]
> A API de transcrição do Batch utiliza um serviço REST para pedir transcrições, o respetivo estado e resultados associados. Pode utilizar a API em qualquer linguagem. A seguinte secção descreve como a API é utilizada.

### <a name="query-parameters"></a>Parâmetros de consulta

Esses parâmetros podem ser incluídos na cadeia de consulta da solicitação REST.

| Parâmetro | Descrição | Obrigatório / opcional |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Especifica como lidar com linguagem inapropriada nos resultados de reconhecimento. Aceite os valores são `none` que desativa a filtragem de palavras ofensivas `masked` que substitui a linguagem inapropriada por asteriscos, `removed` linguagem inapropriada todos os que remove o resultado ou `tags` que adiciona as etiquetas de "linguagem inapropriada". A predefinição é `masked`. | Opcional |
| `PunctuationMode` | Especifica como lidar com a pontuação nos resultados de reconhecimento. Aceite os valores são `none` que desativa a pontuação, `dictated` implica que pontuação explícita, `automatic` que permite que o Decodificador lidar com a pontuação, ou `dictatedandautomatic` que implica ditado marcas de pontuação ou automático. | Opcional |


## <a name="authorization-token"></a>Token de autorização

Com todas as funcionalidades do serviço de voz, criar uma chave de subscrição do [portal do Azure](https://portal.azure.com) seguindo nossas [guia de introdução](get-started.md). Se pretender obter transcrições de nossos modelos de linha de base, a criação de uma chave é tudo o que precisa fazer. 

Se pretender personalizar e utilizar um modelo personalizado, adicione a chave de subscrição para o portal de voz personalizada, fazendo o seguinte:

1. Inicie sessão no [conversão de voz personalizada](https://customspeech.ai).

2. Na parte superior direita, selecione **subscrições**.

3. Selecione **ligar a subscrição existente**.

4. Na janela de pop-up, adicione a chave de subscrição e um alias.

    ![A janela Adicionar subscrição](media/stt/Subscriptions.jpg)

5. Copie e cole essa chave no código do cliente no exemplo a seguir.

> [!NOTE]
> Se planeia utilizar um modelo personalizado, terá também o ID desse modelo. Este ID não é o ID de ponto final que encontrar na vista de detalhes do ponto final. É o ID de modelo que pode recuperar ao selecionar os detalhes desse modelo.

## <a name="sample-code"></a>Código de exemplo

Personalize o código de exemplo seguinte, com uma chave de assinatura e uma chave de API. Esta ação permite-lhe obter um token de portador.

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

Depois de obter o token, especifique o URI de SAS que aponta para o arquivo de áudio que necessita de transcrição. O restante do código percorre o estado e exibe os resultados. Em primeiro lugar, defina a chave, região, modelos de utilização e o SA, conforme mostrado no seguinte fragmento de código. Em seguida, instancio o cliente e o pedido POST. 

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // SAS URI 
            private const string RecordingsBlobUri = "SAS URI pointing to the file in Azure Blob Storage";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("guid of the acoustic adaptation model");
            private static Guid AdaptedLanguageId = new Guid("guid of the language model");

            // Creating a Batch Transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Agora que fez o pedido, pode consultar e transferir os resultados de transcrição, conforme mostrado no seguinte fragmento de código:

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
                            // not created from here, continue
                            continue;
                        }
                            
                        completed++;
                            
                        // if the transcription was successful, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succeeded. Results: ");
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

Para obter detalhes completos sobre as chamadas anteriores, consulte nosso [documento swagger](https://westus.cris.ai/swagger/ui/index). Para o exemplo completo mostrado aqui, aceda a [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> No código anterior, a chave de subscrição é o recurso de fala que criar no portal do Azure. Teclas de obtido no recurso de serviço de voz personalizada não funciona.

Tome nota da configuração assíncrona para áudio de lançamentos e a receção de estado de transcrição. O cliente que criou é um cliente de HTTP do .NET. Há uma `PostTranscriptions` método para enviar os detalhes do arquivo de áudio e um `GetTranscriptions` método para receber os resultados. `PostTranscriptions` Retorna um identificador, e `GetTranscriptions` utiliza para criar um identificador para obter o estado de transcrição.

O código de exemplo atual não especifica um modelo personalizado. O serviço utiliza os modelos de linha de base para fotografar o ficheiro ou ficheiros. Para especificar os modelos, pode passar o mesmo método que os IDs de modelo para o acústicos e o modelo de idioma. 

Se não pretender utilizar a linha de base, passe o IDs de modelo para modelos de acústica e idioma.

> [!NOTE]
> Para transcrições de linha de base, não é necessário declarar os pontos de extremidade dos modelos de linha de base. Se pretender utilizar modelos personalizados, fornecer suas IDs de pontos de extremidade como o [exemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Se pretender utilizar uma linha de base acústica com um modelo de idioma de linha de base, tem de declarar apenas o ID de ponto final do modelo personalizado Microsoft Deteta o modelo de linha de base do parceiro&mdash;se acústicos ou linguagem&mdash;e utiliza-o para satisfazer o pedido de transcrição.

### <a name="supported-storage"></a>Armazenamento suportadas

Atualmente, o armazenamento apenas suportado é o armazenamento de Blobs do Azure.

## <a name="download-the-sample"></a>Transferir o exemplo

Pode encontrar o exemplo neste artigo sobre [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Uma transcrição de áudio requer, normalmente, um intervalo de tempo igual a duração do arquivo de áudio, mais uma sobrecarga de dois a três minutos.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
