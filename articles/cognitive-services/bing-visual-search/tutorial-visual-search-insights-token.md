---
title: 'Tutorial: ImageInsightsToken - Pesquisa Visual do Bing'
titlesuffix: Azure Cognitive Services
description: Como utilizar o SDK de Pesquisa Visual do Bing para obter os URLs de imagens especificados pelo ImageInsightsToken.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 06d6bc8e53276b5542210c2843d7221d6fd79c09
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386439"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Tutorial: ImageInsightsToken e resultados do SDK de Pesquisa Visual do Bing
O SDK de Pesquisa Visual inclui uma opção para localizar imagens online a partir de uma pesquisa anterior, que devolve um `ImageInsightsToken`.  Neste exemplo obtém um `ImageInsightsToken` e utiliza o token numa pesquisa subsequente.  O código envia o `ImageInsightsToken` para o Bing e devolve resultados que incluem os URLs de Pesquisa do Bing e os URLs de imagens semelhantes encontradas online.

## <a name="prerequisites"></a>Pré-requisitos
O Visual Studio 2017. Se necessário, pode transferir a versão gratuita da comunidade aqui: https://www.visualstudio.com/vs/community/.
É necessária uma chave de API de serviços cognitivos para autenticar as chamadas SDK. Inscreva-se numa chave de avaliação gratuita. A chave de avaliação é válida durante sete dias, com uma chamada por segundo. Para cenários de produção, compre uma chave de acesso. Veja também as informações sobre preços.
A capacidade de executar o SDK do .NET, aplicações do .net core 1.1. Pode obter o CORE, o Framework e o Runtime aqui: https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Dependências da aplicação
Para configurar a aplicação de consola através do SDK de Pesquisa na Web do Bing, aceda à opção Gerir Pacotes NuGet a partir do Explorador de Soluções no Visual Studio. Adicionar:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Pacotes de Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage.

A instalação do pacote do SDK de Pesquisa na Web do NuGet também instala as dependências, que incluem:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Obter o ImageInsightsToken da Pesquisa de Imagens

Este exemplo utiliza um `ImageInsightsToken` obtido pelo método seguinte.  Para obter mais informações sobre esta chamada, veja [Início rápido de C# no SDK de Pesquisa de Imagens](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

O código procura os resultados de uma consulta para "Canadian Rockies" e obtém um ImageInsightsToken. Imprime o token de informações, o url da miniatura e um url de conteúdo de imagem da primeira imagem.  O método devolve o `ImageInsightsToken` para utilização num pedido de Pesquisa Visual subsequente.

```
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }
```

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Especificar o ImageInsightsToken para pedido de Pesquisa Visual

Este exemplo utiliza o token de informações devolvido pelo método anterior. O código seguinte cria um objeto `ImageInfo` a partir do `ImageInsightsToken` e carrega o objeto ImageInfo para um `VisualSearchRequest`. Especificar `ImageInsightsToken` num `ImageInfo` para o `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Utilize a Pesquisa Visual para localizar imagens de um ImageInsightsToken

O `VisualSearchRequest` contém informações sobre a imagem a procurar no objeto `ImageInfo`.  O método `VisualSearchMethodAsync` obtém os resultados.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obter os dados de URL de ImageModuleAction
Os resultados da Pesquisa Visual são objetos `ImageTag`.  Cada etiqueta contém uma lista de objetos `ImageAction`.  Cada `ImageAction` contém um campo `Data`, que é uma lista de valores que dependem do tipo de ação:

Pode obter os vários tipos com o código seguinte:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
A aplicação concluída devolve:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1

Conforme mostrado na lista anterior, os tipos `TopicResults` e `ImageResults` contêm consultas de imagens relacionadas. Os URLs na lista ligam aos resultados da pesquisa do Bing.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>URLs de PagesIncluding ActionType de imagens encontradas pela Pesquisa Visual

Obter os URLs da imagem atual requer uma conversão que lê um `ActionType` como `ImageModuleAction`, que contém um elemento `Data` com uma lista de valores.  Cada valor é o URL de uma imagem.  O seguinte converte o tipo de ação `PagesIncluding` em `ImageModuleAction` e lê os valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Para obter mais informações sobre estes tipos de dados, veja [Imagens - Pesquisa Visual](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).

## <a name="complete-code"></a>Código completo

O código seguinte executa os exemplos anteriores. Envia o `ImageInsightsToken` num pedido post. Em seguida, imprime os URLs de pesquisa do Bing para cada ActionType. Se o ActionType for `PagesIncluding`, o código obtém os itens `ImageObject` no `Data`.  Os `Data` contêm uma lista de valores, que são os URLs de imagens em páginas Web.  Copie e cole os URLs resultantes da Pesquisa Visual no browser, para que os resultados sejam apresentados. Copie e cole os itens de ContentUrl no browser, para que as imagens sejam apresentadas.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            insightsToken = ImageResults(subscriptionKey);

            VisualSearchInsightsToken(subscriptionKey, insightsToken);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        // Searches for results on query (Canadian Rockies) and gets an ImageInsightsToken.
        // Also prints first image insights token, thumbnail url, and image content url.
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }


        // This method will get Visual Search results from an imageInsightsToken obtained from the return value of the ImageResults method.
        // The method includes imageInsightsToken the in a knowledgeRequest parameter, along with a cropArea object. 
        // It prints out the imageInsightsToken uploaded in the request.
        // Finally the example prints URLs of images found using the imageInsightsToken.

        public static void VisualSearchInsightsToken(string subscriptionKey, string insightsTok)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object.
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);

                // An image binary is not necessary here, as the image is specified by insights token.
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with imageInsightsToken and knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {insightsTok}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    if (visualSearchResults.Tags.Count > 0)
                    {
                        // List of tags
                        foreach (ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            {
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }
                        }
                    }

                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }

                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```

## <a name="next-steps"></a>Passos seguintes
[Resposta da Pesquisa Visual](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
