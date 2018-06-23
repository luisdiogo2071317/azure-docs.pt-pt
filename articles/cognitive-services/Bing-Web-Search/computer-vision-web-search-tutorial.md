---
title: Tutorial de aplicação móvel de pesquisa Visual
description: Abra a aplicação origem c# implementar pesquisa visual utilizando a estrutura de plataforma cognitivos API dos serviços do computador visão, API de pesquisa do Bing Web e xamarin. Forms.
services: bing-web-search, computer-vision
author: Aristoddle
manager: bking
ms.service: cognitive-services
ms.devlang: csharp
ms.topic: article
ms.date: 06/22/2017
ms.author: t-jolan
ms.openlocfilehash: 54dabaeaad2e49ba7cc138636054bc3dfa4b8379
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352514"
---
# <a name="visual-search-mobile-app-tutorial"></a>Tutorial de aplicação móvel de pesquisa Visual

## <a name="introduction"></a>Introdução  
Este tutorial explicar o [computador visão API](https://azure.microsoft.com/services/cognitive-services/computer-vision/) e [API de pesquisa do Bing Web](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) pontos finais e mostra como pode ser utilizados para criar uma aplicação de pesquisa de visual basic com [xamarin. Forms](https://developer.xamarin.com/guides/xamarin-forms/).  Em geral, este tutorial abrange os seguintes tópicos: 

> [!div class="checklist"]
> * Configurar o Visual Studio para desenvolver aplicações xamarin. Forms
> * Utilizar o [Plug-in de suporte de dados de Xamarin](https://github.com/jamesmontemagno/MediaPlugin) para capturar e importar dados de imagem
> * Análise de texto a partir de uma imagem utilizando as APIs de imagem digitalizada
> * Enviar pedidos de pesquisa para a API de pesquisa do Bing Web
> * Análise de respostas JSON a partir de ambos os APIs com [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) (com LINQ e o modelo de anulação da serialização de objetos)
> * Criar uma interface de utilizador do xamarin. Forms de plataforma para a pesquisa de visual 

## <a name="prerequisites"></a>Pré-requisitos

Este exemplo foi desenvolvido utilizando xamarin. Forms com [Visual Studio 2017](https://www.visualstudio.com/downloads/). A Comunidade edição do Visual Studio 2017 pode ser utilizado. Para obter mais informações sobre como utilizar o Xamarin com o Visual Studio, consulte o [Xamarin documentação](https://developer.xamarin.com/guides/cross-platform/getting_started/).

Este exemplo faz com que a utilização de pacotes de NuGet seguintes:

> [!div class="checklist"]
> * [Plug-in do suporte de dados de Xamarin](https://github.com/jamesmontemagno/MediaPlugin)
> * [Json.NET](https://github.com/JamesNK/Newtonsoft.Json)

A aplicação utiliza as APIs de serviços cognitivos seguintes:

> [!div class="checklist"]
> * [Pesquisa na Web Bing API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) 
> * [API de Imagem Digitalizada](https://azure.microsoft.com/services/cognitive-services/computer-vision/)

Para obter chaves de avaliação de 30 dias para estas APIs, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/). Para obter mais informações sobre a obtenção de chaves para fins comerciais, consulte [preços](https://azure.microsoft.com/pricing/calculator/).

## <a name="setup-for-development"></a>Configuração para o desenvolvimento  

### <a name="installing-xamarin-on-windows"></a>Instalar Xamarin para Windows
Com qualquer edição do Visual Studio 2017, instalado, abra o instalador de Visual Studio, selecione o menu de hamburger associado com a instalação do Visual Studio e escolha **modificar**.  

![Instalador do Visual Studio](./media/computer-vision-web-search-tutorial/visual-studio-installer.png) 

Desloque-se para baixo para jogos & móveis e ativar **desenvolvimento móvel com o .NET**, se já não está ativada.

![Visual instalador Studio com xamarin. Forms selecionado](./media/computer-vision-web-search-tutorial/xamarin-forms-is-enabled.png)

Por fim, clique em **modificar** no canto inferior direito da janela e aguarde Xamarin instalar.

### <a name="installing-xamarin-on-macos"></a>Instalar o Xamarin no macOS
Xamarin vem pré-empacotadas com o Visual Studio para Mac. Não deve ser é necessário instalar.

## <a name="building-and-running-the-app"></a>Criar e executar a aplicação

Um ficheiro de solução do Visual Studio *(. sln)* para a pesquisa Visual aplicação pode ser transferida do [Visual de pesquisa de aplicação com serviços cognitivos](https://azure.microsoft.com/resources/samples/cognitive-services-xamarin-forms-computer-vision-search/). Pode transferir o arquivo ZIP de utilizando um browser, clone-o para a estação de trabalho a partir do GitHub ou transferi-lo com o Visual Studio.

Para começar a trabalhar com o exemplo, abra `VisualSearchApp.sln` no Visual Studio.  A inicializar os componentes necessários pode demorar um pouco.

A aplicação requer duas bibliotecas de terceiros: **Json.NET** e **Plug-in de suporte de dados de Xamarin**. Pode instalar estas direito de bibliotecas no Visual Studio com o Gestor de pacotes NuGet. Escolha **ferramentas > Gestor de pacotes NuGet > Gerir NuGet pacotes para a solução**, ou clique com o botão direito a solução no Explorador de soluções e escolha **gerir pacotes NuGet** no menu de contexto.

Na janela do NuGet, procurar e instalar **Plug-in do suporte de dados de Xamarin** (Xam.Plugin.Media) versão 2.6.2 e **Json.NET** (newtonsoft) 10.0.3. Lembre-se de que selecione todos os projetos durante a instalação.

Para criar a aplicação para todas as plataformas disponíveis, prima **Ctrl + Shift + B**, ou clique em **criar** no menu do Friso e escolha **compilar solução**.  Para compilar e testar código para iOS a partir de um sistema Windows, consulte [neste guia](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/) para obter ajuda.

Antes de executar a aplicação, tem de selecionar uma configuração de destino, a plataforma e o projeto.  Compilam aplicações xamarin. Forms para código nativo para o Windows, Android e iOS. Este tutorial inclui capturas de ecrã da versão do Windows de exemplo, mas todas as versões são funcionalmente equivalentes. As definições de implementação utilizadas neste guia são mostradas aqui.  

![Visual Studio configurado para compilar para telemóvel Android](./media/computer-vision-web-search-tutorial/configuration-selection.png)

Depois do processo de compilação foi bem sucedida e tiver selecionado uma plataforma de destino, clique em de **iniciar** botão na barra de ferramentas ou prima **F5**. Visual Studio implementa a solução para a plataforma de destino e inicia-lo.

É apresentada a página de adicionar as chaves. (Esta página está definida no `AddKeysPage.xaml`.)  

![Imagem de adicionar as chaves de página](./media/computer-vision-web-search-tutorial/add-keys-page.png)  

Aqui, cole as chaves de visão do computador e a API de pesquisa do Bing Web. A API de visão do computador também necessita de escolher o servidor onde está alojado o ponto final.

> [!TIP]
> Para ignorar esta página, introduza esta informação nas localizações adequadas no `App.xaml.cs`. 

A aplicação valida as suas chaves efetuando uma consulta de teste, em seguida, é-lhe selecionar OCR página (definido no `OcrSelectPage.xaml`).
   
![Imagem da página selecionar OCR](./media/computer-vision-web-search-tutorial/ocr-select-page.png)  

Na parte superior neste ecrã, pode escolher se o texto que pretende para reconhecer é impressas ou handwritten.

> [!TIP]
> Manter o item a partir do qual está a tentar reconhecer texto, como nível quanto possível e certifique-se de que é uniformemente lit com nenhuma reflections. Encontrámos que handwritten OCR, por vezes, funciona melhor para tipos de letra do script ou outro texto "fancy".

Em seguida, clique em **tirar fotografias** ou **fotografia de importação** tirar uma fotografia com a câmara do dispositivo, ou escolha uma fotografia armazenada no seu dispositivo.

Depois de uma fotografia é executada ou escolhida, a imagem é transferida para a API de visão do computador. A página de palavras encontrado (definido no `OcrResultsPage.xaml`) apresenta qualquer palavras reconhecidas na imagem.

![Imagem da página de resultados OCR](./media/computer-vision-web-search-tutorial/ocr-results-page.png)  

> [!NOTE]
> É a imagem são utilizadas para esses resultados no repositório de origem como `SamplePhotos\TestImage.jpg`.

Quando clique num item encontrado de palavras página, a página de resultados da Web (`WebResultsPage.xaml`) for apresentada, que mostra a parte superior Bing resultados para essa procura.

![Imagem da página de resultados Web](./media/computer-vision-web-search-tutorial/web-results-page.png)  
Por fim, escolha um item na página de resultados de Web para abrir a hiperligação num WebView incorporado. (Ou navegue de volta para escolher um resultado diferente)

![Imagem da página de vista Web](./media/computer-vision-web-search-tutorial/web-view-page.png)  

Pode interagir com a página como teria qualquer browser ou navegue de volta para a página de resultados de Web. 

## <a name="review-and-learn"></a>Reveja e saiba mais

Agora que já executada a aplicação de pesquisa Visual para uma giratórias, vamos explorar exatamente como estamos a utilizar APIs dois serviços cognitivos de Microsoft.  Se estiver a utilizar este exemplo como ponto de partida para a sua própria aplicação ou simplesmente como um procedimentos para as APIs, é útil para percorrer aplicação ecrã-por-ecrã para examinar exatamente como funciona.

### <a name="add-keys-page"></a>Adicionar página de chaves
A página de adicionar chaves IU está definida no `AddKeysPage.xaml`, e a respetiva lógica principal está definida no `AddKeysPage.xaml.cs`. Uma vez que as chaves são validadas ao efetuar um pedido de teste, a hora é ripe estabelecer como utilizar pontos finais de serviços cognitivos em c#.  

A estrutura básica desta interação é: 

1. Instanciar *HttpResponseMessage* e *HttpClient* objetos de *System.Net.Http*
2. Anexar qualquer cabeçalhos pretendidos (definidos na referência da API de cada ponto final) para o *HttpClient* objeto
3. Enviar um pedido GET ou POST com os seus dados, a adição de quaisquer parâmetros necessários para o ponto final de URI
4. Certifique-se de que a resposta foi concluída com êxito
5. Transmitir a resposta para processamento adicional

A função que verifica a validade da chave de API de pesquisa do Bing é `CheckBingSearchKey()`, apresentados aqui.

```csharp
async Task CheckBingSearchKey(object sender = null, EventArgs e = null)
{
    HttpResponseMessage response;
    HttpClient SearchApiClient = new HttpClient();

    SearchApiClient.DefaultRequestHeaders.Add(AppConstants.OcpApimSubscriptionKey, BingSearchKeyEntry.Text);

    try
    {
        response = await SearchApiClient.GetAsync(AppConstants.BingWebSearchApiUrl + "q=test");

        if (response.StatusCode != System.Net.HttpStatusCode.Unauthorized)
        {
            BingSearchKeyEntry.BackgroundColor = Color.Green;
            AppConstants.BingWebSearchApiKey = BingSearchKeyEntry.Text;
            bingSearchKeyWorks = true;
        }
        else
        {
            BingSearchKeyEntry.BackgroundColor = Color.Red;
            bingSearchKeyWorks = false;
        }
    }
    catch( Exception exception )
    {
        BingSearchKeyEntry.BackgroundColor = Color.Red;
        Console.WriteLine($"ERROR: {exception.Message}");
    }
}
```

Uma função semelhante, `CheckComputerVisionKey()`, é utilizado para validar a chave de computador visão.

### <a name="ocr-select-page"></a>Página selecionar OCR

Página selecionar OCR (`OcrSelectPage.xaml`) tem duas funções importantes. Em primeiro lugar, determina o tipo de OCR é efetuado a fotografia de destino. Segundo, o permite ao utilizador capturar ou abrir a imagem que pretendem processar.

A segunda tarefa é tradicionalmente complexa numa aplicação de várias plataformas, porque cada plataforma necessita de código diferentes. O plug-in de suporte do Xamarin processa com apenas alguns linhas de código.

A seguinte função incorpora um exemplo de como utilizar o plug-in de suporte do Xamarin para captura de fotografias.  Aqui, iremos:

1. Certifique-se de que uma câmara do dispositivo atual
2. Instanciar um `StoreCameraMediaOptions` objeto para especificar onde pretende guardar a imagem capturada
3. Capturar uma imagem e obter um `MediaFile` objeto que contém os dados de imagem
4. Descompacte o `MediaFile` para uma matriz de bytes
5. Devolver a matriz de bytes para processamento adicional

Eis `TakePhoto()`, a função que utiliza o plug-in de suporte do Xamarin para captura de fotografias.  

```csharp
async Task<byte[]> TakePhoto()
{
    MediaFile photoMediaFile = null;
    byte[] photoByteArray = null;

    if (CrossMedia.Current.IsCameraAvailable)
    {
        var mediaOptions = new StoreCameraMediaOptions
        {
            PhotoSize = PhotoSize.Medium,
            AllowCropping = true,
            SaveToAlbum = true,
            Name = $"{DateTime.UtcNow}.jpg"
        };
        photoMediaFile = await CrossMedia.Current.TakePhotoAsync(mediaOptions);
        photoByteArray = MediaFileToByteArray(photoMediaFile);
    }
    else
    {
        await DisplayAlert("Error", "No camera found", "OK");
        Console.WriteLine($"ERROR: No camera found");
    }
    return photoByteArray;
}
```
A fotografia importar utilitário funciona de forma semelhante. Podem encontrar ambas as peças de funcionalidade `OcrSelectPage.xaml.cs`. 
 
> [!NOTE]
> O ponto final Handwritten OCR só pode processar fotografias que são mais pequenas de 4 MB. Para evitar problemas de tamanho de ficheiro, iremos reduzir a fotografia a 50% do seu tamanho original definindo a opção `PhotoSize = PhotoSize.Medium` no `StoreCameraMediaOptions` objeto.  Se o dispositivo demora exceptionally de alta qualidade fotografias e está a obter erros, pode experimentar `PhotoSize = PhotoSize.Small` em vez disso.

Segue-se a função de utilitário utilizada para converter um *MediaFile* para uma matriz de bytes.

```csharp
byte[] MediaFileToByteArray(MediaFile photoMediaFile)
{
    using (var memStream = new MemoryStream())
    {
        photoMediaFile.GetStream().CopyTo(memStream);
        return memStream.ToArray();
    }
}
```

### <a name="ocr-results-page"></a>Página de resultados de OCR

A página de resultados de OCR mostra texto extraído da imagem a utilizar o **Json.NET** [SelectToken método](http://www.newtonsoft.com/json/help/html/SelectToken.htm).  Os dois pontos finais de OCR funcionam de forma ligeiramente diferente, pelo que é útil para discutir ambos.  

Porque a API de visão do computador só é alojada em algumas localizações de servidor, seu ponto final tem de ser construído dinamicamente durante a execução. A função `SetOcrLocation` (parte da estáticos *AppConstants* encontrar a classe no `AppConstants.cs`) define a localização do ponto final URI. Corresponde à seleção do utilizador na página de chaves de adicionar ou utiliza o valor definido `App.xaml.cs`. 

```csharp
public static void SetOcrLocation(string location)
{
    ComputerVisionApiOcrUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/ocr?language=en&detectOrientation=true";
    ComputerVisionApiHandwritingUrl = $"https://{location}.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true";
}
```

Vamos um olhar estes pedidos de API. A API de OCR de visão do computador é capaz de análise de texto de uma determinada linguagem, mas podemos dizê-lo para procurar texto em inglês melhorar os resultados. Podemos também permitem a API detetar orientação de texto para-nos. Utilizar uma orientação fixa poderá melhorar o nosso resultados da análise, mas a deteção de orientação pode ser útil para uma aplicação móvel.

Pode saber mais sobre os parâmetros disponíveis com este ponto final do [impressão referência da API de reconhecimento de caráter Optical](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fc).  

A API de OCR Handwritten está ainda em pré-visualização e, atualmente só funciona com o texto em inglês.  Por este motivo, o parâmetro só é atualmente um sinalizador que indica se deve ou não ao analisar o texto handwritten de todo. A API de OCR handwritten pode analisar ambos máquina impressos e handwritten texto, mas `handwriting=false` fornece uma melhor resultados no texto impressos. 

Uma vez que esta aplicação em inglês, iremos foi ter utilizado Handwritten OCR apenas para este exemplo e defina o `handwriting` sinalizador de acordo com a qual o utilizador informa-nos para reconhecer.  Utilizamos os dois pontos finais para fins de ilustrativa.  

Pode saber mais sobre os parâmetros disponíveis com este ponto final do [Handwritten referência da API de reconhecimento de caráter Optical](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).

Agora, vamos ver as funções que chamar estas APIs.

`FetchPrintedWordList()` utiliza o ponto final de impressão OCR ao analisar o texto impressos de imagens. O pedido HTTP segue uma estrutura semelhante da chamada que utilizado na página de adicionar as chaves para validar a chave, mas é necessário enviar uma fotografia. Uma fotografia é demasiado grande para passar uma cadeia de consulta, pelo que iremos tem de utilizar um pedido POST de HTTP em vez de um pedido GET. É necessário para codificar o nosso fotografia (o que existe na memória como uma matriz de bytes) para um `ByteArrayContent` de objeto e adicionar um cabeçalho para este objeto definir o tipo de dados que estamos a enviar. 

Pode ler sobre os tipos de conteúdo aceitáveis o [referência da API de visão do computador](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/587f2c6a154055056008f200).  

```csharp
async Task<ObservableCollection<string>> FetchPrintedWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiOcrUrl, content);
        }

        string ResponseString = await response.Content.ReadAsStringAsync();
        JObject json = JObject.Parse(ResponseString);

        // Here, we pull down each "line" of text and then join it to 
        // make a string representing the entirety of each line.  
        // In the Handwritten endpoint, you are able to extract the 
        // "line" without any further processing.  If you would like 
        // to simply get a list of all extracted words,* you can do 
        // this with:
        // json.SelectTokens("$.regions[*].lines[*].words[*].text) 
        IEnumerable<JToken> lines = json.SelectTokens("$.regions[*].lines[*]");
        if (lines != null)
        {
            foreach (JToken line in lines)
            {
                IEnumerable<JToken> words = line.SelectTokens("$.words[*].text");
                if (words != null)
                {
                    wordList.Add(string.Join(" ", words.Select(x => x.ToString())));
                }
            }
        }
    }
    return wordList;
}
```
> [!TIP]
> Tenha em atenção a como estamos a utilizar o **Json.NET** [SelectToken método](http://www.newtonsoft.com/json/help/html/SelectToken.htm) para extraia o texto a partir do objeto de resposta. `SelectToken` é ideal quando tem apenas uma parte específica da resposta JSON, que é, em seguida, pode passar para a função next. Em outras partes desse código, iremos anular a serialização de respostas JSON em objetos de modelo definidos no `ModelObjects.cs`.

A principal diferença entre o OCR de impressão e o pedido de Handwritten OCR é que o serviço de impressão OCR devolve o texto reconhecido como parte da resposta HTTP, enquanto o serviço de Handwritten OCR requer um pedido para obter as informações adicional. O pedido de Handwritten OCR inicial devolve um Estado de HTTP 202, indica apenas que o processamento foi iniciada. Esta resposta contém um ponto final que o cliente tem de verificar para obter a resposta completa, quando estiver disponível. Consulte `FetchHandwrittenWordList()` para ver como todos os funciona.

```csharp
async Task<ObservableCollection<string>> FetchHandwrittenWordList()
{
    ObservableCollection<string> wordList = new ObservableCollection<string>();
    if (photo != null)
    {
        // Make the POST request to the handwriting recognition URL
        HttpResponseMessage response = null;
        using (var content = new ByteArrayContent(photo))
        {
            // The media type of the body sent to the API. 
            // "application/octet-stream" defines an image represented 
            // as a byte array
            content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
            response = await visionApiClient.PostAsync(AppConstants.ComputerVisionApiHandwritingUrl, content);
        }

        // Fetch results
        IEnumerable<string> operationLocationValues;
        string statusUri = string.Empty;
        if (response.Headers.TryGetValues("Operation-Location", out operationLocationValues))
        {
            statusUri = operationLocationValues.FirstOrDefault();

            // Ping status URL, wait for processing to finish 
            JObject obj = await FetchResultFromStatusUri(statusUri);
            IEnumerable<JToken> strings = obj.SelectTokens("$.recognitionResult.lines[*].text");
            foreach (string s in strings)
            {
                wordList.Add((string)s);
            }
        }
    }
    return wordList;
}
```

`FetchResultFromStatusUri()` é a segunda parte do processo de escrita OCR. Ping sobre o URI extraído de metadados de resposta iniciais ou até um resultado é obtido ou a função exceder o tempo limite.  É importante que esta função é chamada assíncrona no seu próprio thread. Caso contrário, este método seria bloquear cópias de segurança a interface de utilizador, até que o processamento foi concluído.

```csharp
// Takes in the url to check for handwritten text parsing results, and pings it per second until processing is finished
// Returns the JObject holding data for a successful parse
async Task<JObject> FetchResultFromStatusUri(string statusUri)
{
    JObject obj = null;
    int timeoutcounter = 0;
    HttpResponseMessage response = await visionApiClient.GetAsync(statusUri);
    string responseString = await response.Content.ReadAsStringAsync();
    obj = JObject.Parse(responseString);
    while ((!((string)obj.SelectToken("status")).Equals("Succeeded")) && (timeoutcounter++ < 60))
    {
        await Task.Delay(1000);
        response = await visionApiClient.GetAsync(statusUri);
        responseString = await response.Content.ReadAsStringAsync();
        obj = JObject.Parse(responseString);
    } 
    return obj;
}
```

### <a name="web-results-page"></a>Página de resultados da Web
Depois do utilizador seleciona um termo de pesquisa apresentado na página de resultados de OCR, iremos avançar ao longo para a página de resultados de Web.  Aqui vamos construir um [API de pesquisa do Bing Web](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/) pedido, enviá-lo para o ponto final do serviço e anular a serialização da resposta JSON utilizando o **Json.NET** [DeserializeObject](http://www.newtonsoft.com/json/help/html/DeserializeObject.htm) método.  

```csharp
async Task<WebResultsList> GetQueryResults()
{
    // URL-encode the query term
    var queryString = System.Net.WebUtility.UrlEncode(queryTerm);

    // Here we encode the URL that will be used for the GET request to 
    // find query results.  Its arguments are as follows:
    // 
    // - [count=20] This sets the number of webpage objects returned at 
    //   "$.webpages" in the JSON response.  Currently, the API asks for 
    //   20 webpages in the response
    //
    // - [mkt=en-US] This sets the market where the results come from.
    //   Currently, the API looks for english results based in the 
    //   United States.
    //
    // - [q=queryString] This sets the string queried using the Search 
    //   API.   
    //
    // - [responseFilter=Webpages] This filters the response to only 
    //   include Webpage results.  This tag can take a comma seperated 
    //   list of response types that you are looking for.  If left 
    //   blank, all responses (webPages, News, Videos, etc) are 
    //   returned.
    //
    // - [setLang=en] This sets the languge for user interface strings. 
    //   To learn more about UI strings, check the Web Search API 
    //   reference.
    //
    // - [API Reference] https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference
    string uri = AppConstants.BingWebSearchApiUrl + $"count=20&mkt=en-US&q={queryString}&responseFilter=Webpages&setLang=en";

    // Make the HTTP Request
    WebResultsList webResults = null;
    HttpResponseMessage httpResponseMessage = await searchApiClient.GetAsync(uri);
    var responseContentString = await httpResponseMessage.Content.ReadAsStringAsync();
    JObject json = JObject.Parse(responseContentString);
    JToken resultBlock = json.SelectToken("$.webPages");
    if (resultBlock != null)
    {
        webResults = JsonConvert.DeserializeObject<WebResultsList>(resultBlock.ToString());
    }
    return webResults;
}
```

A API de pesquisa do Bing Web funciona melhor quando o utilizador fornecer o mesmo informações como pode sobre o que o utilizador poderá querer. Em particular, deve utilizar quase sempre o `mkt` e `setLang` paramaters (aqui definido para inglês) para identificar a localização e o idioma preferencial do utilizador.

> [!NOTE]
> Iremos mantida simples para certificar-se de que o código de origem foi fáceis de compreender a nossa consulta de pesquisa na Web Bing.  Numa aplicação real, deve adicionar os seguintes cabeçalhos para o pedido de HTTP para os melhores resultados. 
> * Agente de utilizador  
> * X-MSEdge-ClientID  
> * X-pesquisa-ClientIP  
> * Localização de pesquisa X  
>
> Pode saber mais sobre estes valores de cabeçalho no [referência da API Web pesquisa do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers)

## <a name="next-steps"></a>Próximos Passos
Serviços cognitivos Microsoft fornecem vários serviços adicionais facilmente foi integrar esta aplicação.  Por exemplo, pode:

* Adicionar [pesquisa do Bing entidade](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) para melhorar os resultados da pesquisa web
* Comutação em [pesquisa do Bing personalizada](https://azure.microsoft.com/services/cognitive-services/bing-custom-search/) em vez de pesquisa na Web Bing
* Utilize o [pesquisa do Bing imagem](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) imagem capacidade insights para obter mais informações sobre a imagem capturada e localizar as imagens semelhantes na web
* Utilizar [Bing de verificação ortográfica](https://azure.microsoft.com/services/cognitive-services/spell-check/) para melhorar ainda mais a qualidade do seu texto analisado
* Integrar o [Microsoft Translator](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) para ver o texto extraído em idiomas diferentes
* Combinar e misturar os outros serviços do [Portal de serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/); o sky do limite!
