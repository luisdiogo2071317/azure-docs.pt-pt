---
title: Utilizar a API do indexador de vídeo do Azure | Documentos da Microsoft
description: Este artigo mostra como começar a utilizar a API do Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: bd0f9e01257bec2d39ef0c8e13b68c4a7a13637d
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542934"
---
# <a name="use-azure-video-indexer-api"></a>Utilizar a API do indexador de vídeo do Azure

> [!Note]
> A API do Video Indexer V1 foi preterido no dia 1 de Agosto de 2018. Agora, deve utilizar a API do Video Indexer v2. <br/>Para programar com APIs do Video Indexer v2, veja as instruções presentes [aqui](https://api-portal.videoindexer.ai/). 

O Video Indexer consolida várias tecnologias de áudio e vídeo inteligência artificial (IA) oferecidas pela Microsoft num serviço integrado, tornando o desenvolvimento mais simples. As APIs foram concebidas para ativar alcance de desenvolvedores se concentrem nas tecnologias de ia de suporte de dados de consumo sem se preocupar em escala, global, disponibilidade e fiabilidade de plataforma em nuvem. Pode usar a API para carregar os ficheiros, obter informações detalhadas de vídeo, obter os URLs de widgets de informações e player para incorporá-los a sua aplicação e outras tarefas.

Ao criar uma conta do Video Indexer, pode escolher uma conta de avaliação gratuita (onde pode obter um determinado número de minutos de indexação gratuitos) ou uma opção paga (em que não está limitado pela quota). Com a avaliação gratuita, Video Indexer fornece até 600 minutos de indexação gratuito para os utilizadores do Web site e até 2400 minutos de gratuitos de indexação para os utilizadores de API. Com uma opção paga, cria uma conta do Video Indexer que seja [ligado à sua subscrição do Azure e uma conta de Media Services do Azure](connect-to-azure.md). Paga minutos indexados, bem como os serviços de multimédia do Azure conta custos relacionados. 

Este artigo mostra como os desenvolvedores podem aproveitar o [API do Video Indexer](https://api-portal.videoindexer.ai/).

## <a name="subscribe-to-the-api"></a>Subscrever a API

1. Inicie sessão no [Portal de programador do indexador de vídeos](https://api-portal.videoindexer.ai/).
    
    ![Iniciar sessão](./media/video-indexer-use-apis/video-indexer-api01.png)

    > [!Important]
    > * Tem de utilizar o mesmo provedor que utilizou quando se inscreveu no Video Indexer.
    > * Google pessoais e da Microsoft (outlook/live) contas só podem ser utilizadas para contas de avaliação. Contas ligadas ao Azure necessitam do Azure AD.
    > * Pode haver apenas uma conta Active Directory por email. Se um utilizador tentar iniciar sessão com user@gmail.com LinkedIn e depois disso com user@gmail.com para o mais tarde, o Google irá apresentar uma página de erro, dizendo que o utilizador já existe.

2. Inscreva-se.

    Selecione o [produtos](https://api-portal.videoindexer.ai/products) separador. Em seguida, selecione a autorização e subscrever. 
    
    ![Inscrever-se](./media/video-indexer-use-apis/video-indexer-api02.png)

    > [!NOTE]
    > Novos utilizadores subscrevem automaticamente autorização.
    
    Depois de subscrever, será capaz de ver a sua subscrição e as chaves primárias e secundárias. As chaves devem ser protegidas. As chaves só devem ser utilizadas pelo seu código de servidor. Não deve estar disponíveis no lado do cliente (. js,. HTML, etc.).

    ![Inscrever-se](./media/video-indexer-use-apis/video-indexer-api03.png)

## <a name="obtain-access-token-using-the-authorization-api"></a>Obter o token de acesso com a API de autorização

Uma vez subscrito para a API de autorização, poderá obter tokens de acesso. Estes tokens de acesso são utilizados para autenticar com a API de operações. 

Cada chamada à API de operações deve ser associada um token de acesso, o âmbito de autorização da chamada de correspondência.

- Nível de usuário - o tokens de acesso de nível de utilizador permitem-lhe efetuar operações no **utilizador** nível. Por exemplo, obter contas associadas.
- Nível de conta – o tokens de acesso de nível de conta permitem-lhe efetuar operações no **conta** nível ou o **vídeo** nível. Por exemplo, carregar vídeo, listar todos os vídeos, obter informações de vídeo, etc.
- Nível de vídeo – o tokens de acesso de nível de vídeo permitem-lhe efetuar operações num específico **vídeo**. Por exemplo, obter informações de vídeo, transferir legendas, obter widgets, etc. 

Pode controlar se estes tokens são só de leitura ou permitir a edição, especificando **pokud vlastnost AllowEdit má = verdadeiro/falso**.

Na maioria dos cenários de servidor para servidor, provavelmente usará as mesmas **conta** token, uma vez que ele abrange ambos **conta** operações e **vídeo** operações. No entanto, se estiver a planear tornar o lado do cliente chamadas para o Video Indexer (por exemplo, a partir de javascript), iria querer usar uma **vídeo** token de acesso, para impedir que os clientes obter acesso a toda a conta. Que também é o motivo que, ao incorporar o código de cliente VideoIndexer no seu cliente (por exemplo, utilizando **obter o Insights Widget** ou **obter Widget do leitor**) tem de fornecer um **vídeo**token de acesso.

Para facilitar as coisas, pode utilizar o **autorização** API > **GetAccounts** para obter as suas contas sem obtenção de um utilizador token pela primeira vez. Também pode fazer obter as contas com tokens válidos, permitindo-lhe ignorar uma chamada adicional para obter um token de conta.

Tokens de acesso expiram após 1 hora. Certifique-se de que o seu token de acesso é válido antes de utilizar a API de operações. Se expirar, chamar a API de autorização novamente para receber um novo token de acesso.
 
Está pronto para iniciar a integração com a API. Encontrar [a descrição detalhada de cada API de REST do Video Indexer](http://api-portal.videoindexer.ai/).

## <a name="location"></a>Localização

Operação de todas as APIs requerem um parâmetro de localização, que indica a região ao qual a chamada deve ser roteada e, em que a conta foi criada.

Os valores descritos na tabela a seguir se aplicam. O **valor de parâmetro** é o valor que quando passa através da API.

|**Nome**|**Valor de parâmetro**|**Descrição**|
|---|---|---|
|Avaliação|Versão de avaliação|Utilizado para contas de avaliação.|
|EUA Oeste|westus2|Utilizado para a região do Azure E.u.a. oeste 2.|
|Europa do Norte |europanorte|Utilizado para a região da Europa do Norte de Azure.|
|Ásia Oriental|ásiaoriental|Utilizado para a região do Azure Ásia Oriental.|

## <a name="account-id"></a>ID da Conta 

O parâmetro de ID de conta é necessária em todas as chamadas de API operacionais. ID da conta é um GUID que pode ser obtido de uma das seguintes formas:

* Utilize o **Web site do Video Indexer** para obter o ID de conta:

    1. Navegue para o [Video Indexer](https://www.videoindexer.ai/) Web site e o início de sessão.
    2. Navegue para o **definições** página.
    3. Copie o ID de conta.

        ![ID da Conta](./media/video-indexer-use-apis/account-id.png)

* Uso **Portal do Programador de indexador de vídeo** para obter programaticamente o ID de conta.

    Utilize o [obter contas](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Accounts?) API.
    
    > [!TIP]
    > Pode gerar tokens de acesso para as contas através da definição `generateAccessTokens=true`.
    
* Obtenha o ID de conta a partir do URL de uma página de leitor na sua conta.

    Ao ver um vídeo, o ID aparece a seguir a `accounts` secção e antes do `videos` secção.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Recomendações

Esta secção lista algumas recomendações quando utilizar a API do Video Indexer.

- Se estiver a planear carregar um vídeo, recomenda-se para colocar o arquivo em algum local de rede pública (por exemplo, o OneDrive). Obter a ligação para o vídeo e forneça o URL como o parâmetro de ficheiro de carregamento. 

    O URL fornecido ao indexador de vídeo tem de apontar para um ficheiro de multimédia (áudio ou vídeo). Alguns dos links gerados pelo OneDrive são para uma página HTML que contém o ficheiro. Uma verificação simples para o URL seria colá-lo num navegador – se o ficheiro de começar a baixar, provavelmente é um URL de bom. Se o navegador está renderizando alguma visualização, provavelmente não é uma ligação para um ficheiro, mas uma página HTML.
    
- Quando chama a API que obtém informações de vídeo para o vídeo especificado, obtém uma saída JSON detalhada como o conteúdo de resposta. [Ver detalhes sobre o JSON devolvido neste tópico](video-indexer-output-json.md).

## <a name="code-sample"></a>Exemplo de código

O seguinte trecho de código do c# demonstra a utilização de todas as APIs do indexador de vídeo em conjunto.

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

[Examine os detalhes da saída JSON](video-indexer-output-json.md).

[Descrição geral do indexador vídeo](video-indexer-overview.md)
