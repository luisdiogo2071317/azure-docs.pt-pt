---
title: Tutorial de ImageInsightsToken SDK da pesquisa Visual do Bing | Documentos da Microsoft
description: Como utilizar o SDK de pesquisa Visual do Bing para obter os URLs de imagens especificadas pelo ImageInsightsToken.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 8f6e7f7e88ae78fe7e8a9be4adefd689dd26d0f9
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "41994393"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Tutorial: ImageInsightsToken SDK da pesquisa Visual do Bing e resultados
O SDK de pesquisa Visual inclui uma opção para localizar imagens online a partir de uma pesquisa anterior, que retorna um `ImageInsightsToken`.  Neste exemplo obtém um `ImageInsightsToken` e utiliza o token numa pesquisa subsequente.  O código envia o `ImageInsightsToken` do Bing e devolve resultados que incluem URLs de pesquisa do Bing e URLs de imagens semelhantes encontradas online.

## <a name="prerequisites"></a>Pré-requisitos
O Visual Studio 2017. Se necessário, pode baixar a versão de Comunidade gratuito a partir daqui: https://www.visualstudio.com/vs/community/.
É necessária uma chave de API de serviços cognitivos para autenticar as chamadas SDK. Inscreva-se uma chave de avaliação gratuita. A chave de avaliação é bom para sete dias com uma chamada por segundo. Para cenários de produção, compre uma chave de acesso. Ver informações sobre preços também.
A capacidade de executar o .NET core SDK, aplicativos do .net core 1.1. Pode obter o NÚCLEO, o Framework e o tempo de execução a partir daqui: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Dependências de aplicações
Para configurar uma aplicação de consola utilizando o SDK de pesquisa Web Bing, navegue para a opção de gerir pacotes NuGet no Explorador de soluções no Visual Studio. Adicione:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Pacotes de Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage.

Também instalar o pacote do SDK de pesquisa Web NuGet instala as dependências, incluindo:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft

## <a name="get-the-imageinsightstoken-from-image-search"></a>Obtenha o ImageInsightsToken da pesquisa de imagens
Este exemplo utiliza um `ImageInsightsToken` obtido pelo método seguinte.  Para obter mais informações sobre esta chamada, consulte [início rápido imagem pesquisa SDK C#](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

O código procura os resultados de uma consulta para "Rockies canadense" e obtém um ImageInsightsToken. Imprime de url de token, miniaturas de informações a primeira imagem e o url da imagem de conteúdo.  O método retorna o `ImageInsightsToken`para uso num pedido de pesquisa Visual subsequente.

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

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Especifique o ImageInsightsToken para solicitação de pesquisa Visual
Este exemplo utiliza o token de informações retornado pelo método anterior. O código seguinte cria um `ImageInfo` de objeto do `ImageInsightsToken` e carrega o objeto de ImageInfo num `VisualSearchRequest`. Especifique `ImageInsightsToken` num `ImageInfo` para o `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Utilize a pesquisa Visual para localizar imagens a partir de um ImageInsightsToken
O `VisualSearchRequest` contém informações sobre a imagem para procurar no `ImageInfo` objeto.  O `VisualSearchMethodAsync` método obtém os resultados.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obter os dados de URL de ImageModuleAction
Resultados da pesquisa Visual estão `ImageTag` objetos.  Cada marca contém uma lista de `ImageAction` objetos.  Cada `ImageAction` contém uma `Data` campo que é uma lista de valores que dependem do tipo de ação:

Pode obter os vários tipos com o código a seguir:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
Devolve a aplicação concluída:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx, 5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx, 5831.1

Como mostrado na lista anterior, o `TopicResults` e `ImageResults` tipos conter consultas para imagens relacionadas. Os URLs na ligação de lista de resultados da pesquisa do Bing.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URLs de imagens encontradas por pesquisa Visual

Os URLs de imagem real requer uma conversão que lê um `ActionType` como `ImageModuleAction`, que contém um `Data` elemento com uma lista de valores.  Cada valor é o URL de uma imagem.  As conversões de seguir a `PagesIncluding` tipo de ação para `ImageModuleAction` e lê os valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Para obter mais informações sobre esses tipos de dados, consulte [imagens - pesquisa Visual](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).
## <a name="complete-code"></a>Código completo

O código a seguir executa exemplos anteriores. Envia a `ImageInsightsToken` num pedido post. Em seguida, imprime o Bing URLs de pesquisa para cada ActionType. Se for o ActionType `PagesIncluding`, o código obtém o `ImageObject` itens no `Data`.  O `Data` contém uma lista de valores, que são os URLs de imagens em páginas da Web.  Copie e cole resultante Visual pesquisa URLs ao navegador para mostrar os resultados. Copiar e colar itens de ContentUrl ao navegador para mostrar imagens.

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
## <a name="next-steps"></a>Passos Seguintes
[Resposta de pesquisa Visual](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
