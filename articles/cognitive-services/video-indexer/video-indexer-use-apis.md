---
title: Utilize o indexador de vídeo do Azure API | Microsoft Docs
description: Este artigo mostra como começar a utilizar a API do indexador de vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 06/04/2018
ms.author: juliako
ms.openlocfilehash: d378934a0c085910475c366f4bdb538f09efc12b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356479"
---
# <a name="use-azure-video-indexer-api"></a>Utilize o indexador de vídeo do Azure API

Indexador de vídeo consolida várias tecnologias de áudio e vídeo artificial intelligence (AI) disponibilizadas pela Microsoft num serviço integrado, tornando o desenvolvimento mais simples. As APIs foram concebidas para ativar chegar aos programadores focarem-se no consumir tecnologias de AI do suporte de dados sem se preocupar escala, global, disponibilidade e fiabilidade de plataforma em nuvem. Pode utilizar a API para carregar os ficheiros, obter informações detalhadas de vídeos, obter os URLs de conhecimentos aprofundados e jogador widgets para incorporá-los na sua aplicação e outras tarefas.

> [!Note]
> A versão de avaliação gratuita tem um limite de carregamento diário de 100 ficheiros. Pode verificar a mensagem de erro devolvida para obter mais detalhes. Tenha em atenção que poderá alterar o limite diário.

Este artigo mostra como os programadores podem tirar partido de [vídeo indexador API](https://api-portal.videoindexer.ai/). Para ler uma descrição mais detalhada do serviço indexador de vídeo, consulte o [descrição geral](video-indexer-overview.md) artigo.

## <a name="subscribe-to-the-api"></a>Subscrever a API

1. Inicie sessão.

    Para começar a desenvolver com as vídeo indexador, terá primeiro de sessão para o [vídeo indexador](https://api-portal.videoindexer.ai/) portal. 
    
    ![Inscrever-se](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > 1. Tem de utilizar o mesmo fornecedor utilizado quando se inscreveu no indexador de vídeo.
    > 2. Antes de utilizadores do Azure AD de um domínio podem iniciar sessão, o administrador de domínio do AAD tem de ativar registo esse domínio [aqui](https://api-portal.videoindexer.ai/aadadminconsent).
    > 3. Google pessoais e da Microsoft (outlook/live) só podem ser utilizadas contas para contas de avaliação. As contas ligadas ao Azure requerem AAD.

2. Subscreve.

    Selecione o [produtos](https://api-portal.videoindexer.ai/products) separador. Em seguida, selecione de autorização e subscrever. 
    
    ![Inscrever-se](./media/video-indexer-use-apis/video-indexer-api02.png)
    
    Depois de subscrever, será capaz de ver a sua subscrição e as chaves primários e secundários. As chaves devem ser protegidas. As chaves só devem ser utilizadas pelo seu código de servidor. Não deve estar disponíveis no lado do cliente (. js,. HTML, etc.).

    ![Inscrever-se](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Obter o token de acesso utilizando a API de autorização

Depois de subscrever a API de autorização, poderá obter os tokens de acesso. Estes tokens de acesso são utilizadas para autenticar face a API de operações. 

Cada chamada à API do Operations deve estar associada um token de acesso, o âmbito de autorização da chamada de correspondência.

- Nível de utilizador - o tokens de acesso de nível de utilizador permitem-lhe efetuar operações no **utilizador** nível. Por exemplo, get contas associadas.
- Nível de conta – o tokens de acesso de nível de conta permitem-lhe efetuar operações no **conta** nível ou **vídeo** nível. Por exemplo, carregar as vídeo, lista de todos os vídeos, obter as vídeos insights, etc.
- Nível de vídeo – o tokens de acesso de nível de vídeo permitem-lhe efetuar operações em específico **vídeo**. Por exemplo, obter conhecimentos aprofundados vídeos, transferir legendas, obter widgets, etc. 

Pode controlar se estes tokens são só de leitura ou permitir a edição especificando **valor de allowEdit = true/false**.

A maioria dos cenários de servidor para servidor, irá provavelmente, utilizar o mesmo **conta** token, uma vez que abrange ambos **conta** operações e **vídeo** operações. No entanto, se estiver a planear fazer do lado do cliente (por exemplo, a partir de javascript) as chamadas para o indexador vídeo, seria pretende utilizar um **vídeo** token de acesso, para impedir os clientes de acesso à conta de toda a obter. Que também é o motivo que, ao incorporar o código de cliente VideoIndexer no seu cliente (por exemplo, utilizando **obter Widget de Insights** ou **obter Widget de leitor**) tem de fornecer um **vídeo**token de acesso.

Para facilitar as coisas, pode utilizar o **autorização** API > **GetAccounts** para obter as contas sem obtenção de um utilizador token primeiro. Pode também colocar obter as contas com tokens válidos, permitindo-lhe ignorar uma chamada adicional para obter um token de conta.

Os tokens de acesso expiram após 1 hora. Certifique-se que o seu token de acesso é válido antes de utilizar a API de operações. Se expirar, chamar a API de autorização novamente para obter um novo token de acesso.
 
Está pronto para começar a integrar a API. Localizar [a descrição detalhada da API de REST de indexador cada vídeo](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Localização

Operação de todas as APIs requerem um parâmetro de localização, que indica a região para que a chamada deve ser encaminhada e em que a conta foi criada.

Os valores descritos na seguinte tabela aplicam-se. O **Param valor** é o valor quando passa através da API.

|**Nome**|**Valor de param**|**Descrição**|
|---|---|---|
|Avaliação|Versão de avaliação|Utilizado para contas de avaliação.|
|EUA Oeste|westus2|Utilizado para a região do Azure EUA oeste 2.|
|Europa do Norte |europanorte|Utilizado para a região Europa do Norte de Azure.|
|Ásia Oriental|ásiaoriental|Utilizado para a região do Azure Ásia Oriental.|

## <a name="account-id"></a>ID da Conta 

O parâmetro de ID de conta é necessária em todas as chamadas de API operacionais. ID de conta é um GUID que pode ser obtido de uma das seguintes formas:

* Utilize o portal de indexador de vídeo para obter o ID de conta:

    1. Inicie sessão no [videoindexer](https://www.videoindexer.ai/).
    2. Navegue para o **definições** página.
    3. Copie o ID de conta.

        ![ID da Conta](./media/video-indexer-use-apis/account-id.png)

* Utilizar a API para programaticamente obter o ID de conta.

    Utilize o [obter contas](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > Pode gerar tokens de acesso para as contas através da definição `generateAccessTokens=true`.
    
* Obter o ID de conta do URL de uma página de leitor na sua conta.

    Ao ver um vídeo, o ID de aparece após o `accounts` secção e antes do `videos` secção.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Recomendações

Esta secção lista algumas recomendações quando utilizar a API do indexador de vídeo.

- Se estiver a planear carregar um vídeo, recomenda-se colocar o ficheiro numa localização algumas rede pública (por exemplo, o OneDrive). Obter a ligação para a vídeo e forneça o URL como o param de ficheiros de carregamento. 

    O URL fornecido ao vídeo indexador tem de apontar para um ficheiro de multimédia (vídeo ou áudio). Algumas das ligações de gerados pelo OneDrive destinam-se uma página HTML que contém o ficheiro. Uma verificação fácil para o URL seria cole-o num browser – se o ficheiro inicia a transferência, é provável que um URL válido. Se o browser estiver a compor algumas visualização, provavelmente, não é uma ligação para um ficheiro, mas uma página HTML.
    
- Quando chamar a API que obtém as vídeos insights para o vídeo especificado, obtenha uma saída JSON detalhada como o conteúdo da resposta. [Ver detalhes sobre o JSON devolvido neste tópico](video-indexer-output-json.md).

## <a name="code-sample"></a>Exemplo de código

O fragmento de código do c# seguinte demonstra a utilização de todas as APIs de indexador as vídeo em conjunto.

```csharp
var apiUrl = "https://api.videoindexer.ai";
var accountId = "..."; 
var location = "westus2";
var apiKey = "..."; 

System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

// create the http client
var handler = new HttpClientHandler(); 
handler.AllowAutoRedirect = false; 
var client = new HttpClient(handler);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey); 

// obtain account access token
var accountAccessTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/AccessToken?allowEdit=true").Result;
var accountAccessToken = accountAccessTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// upload a video
var content = new MultipartFormDataContent();
Debug.WriteLine("Uploading...");
// get the video from URL
var videoUrl = "VIDEO_URL"; // replace with the video URL

// as an alternative to specifying video URL, you can upload a file.
// remove the videoUrl parameter from the query string below and add the following lines:
  //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
  //byte[] buffer =newbyte[video.Length];
  //video.Read(buffer, 0, buffer.Length);
  //content.Add(newByteArrayContent(buffer));

var uploadRequestResult = client.PostAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos?accessToken={accountAccessToken}&name=some_name&description=some_description&privacy=private&partition=some_partition&videoUrl={videoUrl}", content).Result;
var uploadResult = uploadRequestResult.Content.ReadAsStringAsync().Result;

// get the video id from the upload result
var videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
Debug.WriteLine("Uploaded");
Debug.WriteLine("Video ID: " + videoId);           

// obtain video access token            
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
var videoTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/Videos/{videoId}/AccessToken?allowEdit=true").Result;
var videoAccessToken = videoTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// wait for the video index to finish
while (true)
{
  Thread.Sleep(10000);

  var videoGetIndexRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/Index?accessToken={videoAccessToken}&language=English").Result;
  var videoGetIndexResult = videoGetIndexRequestResult.Content.ReadAsStringAsync().Result;

  var processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

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
var searchRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/Search?accessToken={accountAccessToken}&id={videoId}").Result;
var searchResult = searchRequestResult.Content.ReadAsStringAsync().Result;
Debug.WriteLine("");
Debug.WriteLine("Search:");
Debug.WriteLine(searchResult);

// get insights widget url
var insightsWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?accessToken={videoAccessToken}&widgetType=Keywords&allowEdit=true").Result;
var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
Debug.WriteLine("Insights Widget url:");
Debug.WriteLine(insightsWidgetLink);

// get player widget url
var playerWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?accessToken={videoAccessToken}").Result;
var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
Debug.WriteLine("");
Debug.WriteLine("Player Widget url:");
Debug.WriteLine(playerWidgetLink);

```

## <a name="next-steps"></a>Passos Seguintes

[Analise os detalhes do resultado JSON](video-indexer-output-json.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md)
