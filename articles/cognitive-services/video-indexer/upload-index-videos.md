---
title: Carregar e indexar os seus vídeos com o indexador de vídeo do Azure | Documentos da Microsoft
description: Este tópico demonstra como utilizar as APIs para carregar e indexar os seus vídeos com o indexador de vídeo do Azure
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: 3cf5a32d95b028664f29b82b14e2294d58ae9925
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580008"
---
# <a name="upload-and-index-your-videos"></a>Carregar e indexar os seus vídeos  

Este artigo mostra como carregar um vídeo com o indexador de vídeos do Azure. A API do Video Indexer fornece duas opções de carregamento: 

* Carregue o vídeo a partir de uma URL (preferida),
* envie o ficheiro de vídeo como uma matriz de bytes no corpo do pedido.

O artigo mostra como utilizar o [carregar vídeo](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) API para carregar e indexar os seus vídeos com base numa URL. O código de exemplo no artigo inclui a comentado o código que mostra como carregar a matriz de bytes.  

Ele também discute alguns dos parâmetros que pode definir a API para alterar o processo e a saída da API.

> [!Note]
> Ao criar uma conta do Video Indexer, pode escolher uma conta de avaliação gratuita (onde pode obter um determinado número de minutos de indexação gratuitos) ou uma opção paga (em que não está limitado pela quota). <br/>Com a avaliação gratuita, Video Indexer fornece até 600 minutos de indexação gratuito para os utilizadores do Web site e até 2400 minutos de gratuitos de indexação para os utilizadores de API. Com uma opção paga, cria uma conta do Video Indexer que seja [ligado à sua subscrição do Azure e uma conta de Media Services do Azure](connect-to-azure.md). Paga minutos indexados, bem como a conta de suporte de dados relacionados com custos. 

## <a name="uploading-considerations"></a>Considerações sobre a carregar
    
- Quando carregar o vídeo com base na URL (preferida) o ponto final deve ser protegido com TLS 1.2 (ou posterior)
- A opção de matriz de bytes é limitada a 2GB e expira após 30 minutos
- O URL fornecido no `videoURL` param tem de ser codificado

## <a name="configurations-and-params"></a>Configurações e parâmetros

Esta seção descreve alguns dos parâmetros opcionais e quando deve defini-las.

### <a name="externalid"></a>externalID 

Este parâmetro permite-lhe especificar um ID que será associado com o vídeo. O ID pode ser aplicado a integração de sistemas externa "Vídeo gerenciamento de conteúdo" (VCM). Os vídeos que se encontram no portal do Video Indexer podem ser pesquisados com o ID externo especificado.

### <a name="indexingpreset"></a>indexingPreset

Utilize este parâmetro se não processadas ou externas gravações contêm o barulho de fundo. Este parâmetro é utilizado para configurar o processo de indexação. Pode especificar os seguintes valores:

- `Default` – De índice e extrair informações com áudio e vídeo
- `AudioOnly` – De índice e extrair informações usando apenas áudio (vídeo ignorar)
- `DefaultWithNoiseReduction` – De índice e extrair informações de áudio e vídeo, ao aplicar algoritmos de redução de ruído no fluxo de áudio

Preço depende da opção de indexação selecionada.  

### <a name="callbackurl"></a>callbackUrl

Um URL de POST para o notificar quando a indexação é concluído. O Video Indexer adiciona dois parâmetros de cadeia de caracteres a ele de consulta: id e o estado. Por exemplo, se o url de retorno de chamada é 'https://test.com/notifyme?projectName=MyProject', a notificação será enviada com parâmetros adicionais para'https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed'.

Também pode adicionar mais parâmetros para o URL antes de lançar a chamada ao Video Indexer e estes parâmetros serão incluídos no retorno de chamada. Mais tarde, no seu código que pode analisar a cadeia de consulta e começar a fazer uma cópia de todos os parâmetros especificados na cadeia de consulta (dados que tinha originalmente anexado ao URL mais as informações do Video Indexer fornecido.) O URL tem de ser codificado.

### <a name="streamingpreset"></a>streamingPreset

Assim que o seu vídeo foi carregado, o indexador de vídeos, codifica opcionalmente o vídeo. Em seguida, prossegue para a indexação e a análise de vídeo. Quando o indexador de vídeos é feito analisar, receberá uma notificação com o ID de vídeo.  

Ao utilizar o [carregar vídeo](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) ou [voltar a indexar vídeo](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) API, um dos parâmetros opcionais é `streamingPreset`. Se definir `streamingPreset` para `Default`, `SingleBitrate`, ou `AdaptiveBitrate`, o processo de codificação é acionado. Depois de terminar a indexação e a codificação de tarefas, o vídeo ser publicado para que também pode transmitir o vídeo. O ponto de final de transmissão em fluxo do qual quer transmitir o vídeo tem de constar da **em execução** estado.

Para executar a indexação e a codificação de tarefas, o [conta de Media Services do Azure ligada à sua conta do Video Indexer](connect-to-azure.md), necessita de unidades reservadas. Para obter mais informações, consulte [Dimensionar processamento de multimédia](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Uma vez que estas são tarefas de computação intensiva, o tipo de unidade de S3 é altamente recomendado. O número de RUs define o número máximo de tarefas que podem ser executadas em paralelo. A recomendação de linha de base é 10 Urs de S3. 

Se só deseja indexar o seu vídeo, mas não codificá-lo, defina `streamingPreset`para `NoStreaming`.

### <a name="videourl"></a>videoUrl

Um URL do ficheiro de vídeo/áudio ser indexados. O URL tem de apontar para um arquivo de mídia (páginas HTML não são suportadas). O ficheiro pode ser protegido por um token de acesso fornecido como parte do URI e o ponto final que serve o ficheiro tem de ser protegida com TLS 1.2 ou superior. O URL tem de ser codificado. 

Se o `videoUrl` não for especificado, o indexador de vídeo espera que passar o ficheiro como um conteúdo do corpo multipart/form.

## <a name="code-sample"></a>Exemplo de código

O seguinte trecho de código do c# demonstra a utilização de todas as APIs do indexador de vídeo em conjunto.

```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var location = "westus2";
    var apiKey = "...";

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    // take the relevant account, here we simply take the first
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Debug.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =newbyte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(newByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video id from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Debug.WriteLine("Uploaded");
    Debug.WriteLine("Video ID:");
    Debug.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Debug.WriteLine("");
        Debug.WriteLine("State:");
        Debug.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Debug.WriteLine("");
            Debug.WriteLine("Full JSON:");
            Debug.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Debug.WriteLine("");
    Debug.WriteLine("Search:");
    Debug.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Debug.WriteLine("Insights Widget url:");
    Debug.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
    Debug.WriteLine("");
    Debug.WriteLine("Player Widget url:");
    Debug.WriteLine(playerWidgetLink);
}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```



## <a name="next-steps"></a>Passos Seguintes

[Examine a saída do indexador de vídeo do Azure produzida pela v2 API](video-indexer-output-json-v2.md)
