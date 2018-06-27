---
title: Pesquisa Visual SDK c# início rápido | Microsoft Docs
description: O programa de configuração para a pesquisa de Visual aplicação de consola c# do SDK.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: 65102862caf3ff2af0d4d7bfdb26d720d17c50ea
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018846"
---
# <a name="visual-search-sdk-c-quickstart"></a>SDK de pesquisa Visual c# início rápido

O SDK do Bing Visual Search utiliza a funcionalidade da REST API para pedidos web e os resultados da análise.
O [origem código para amostras de c# Bing Visual SDK Search](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch) está disponível no Hub de Git.

Cenários de código estão documentados nos cabeçalhos seguintes:
* [Cliente de pesquisa Visual](#client)
* [Aplicação de consola concluída](#complete)
* [Post binário de imagem com cropArea](#binary-crop)
* [Parâmetro KnowledgeRequest](#knowledge-req)
* [As etiquetas, ações e actionType](#tags-actions)
* [Número de etiquetas, número de ações e actionType primeiro](#num-tags-actions)

## <a name="prerequisites"></a>Pré-requisitos
* O Visual Studio 2017. Se necessário, pode transferir a versão de Comunidade gratuita a partir daqui: https://www.visualstudio.com/vs/community/.
* É necessária uma chave de API de serviços cognitivos para autenticar chamadas SDK. Inscrever-se um [chave de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=search-api-v7). A chave de avaliação é boa durante sete dias com uma chamada por segundo. Para o cenário de produção, [comprar a chave de acesso](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7). Consulte também [obter informações sobre preços](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/visual/).
* A capacidade de executar .NET core SDK, as aplicações do .net core 1.1. Pode obter NÚCLEOS, Framework e tempo de execução a partir daqui: https://www.microsoft.com/net/download/.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing Web, navegue para o `Manage NuGet Packages` opção do Explorador de soluções no Visual Studio.  Adicionar o `Microsoft.Azure.CognitiveServices.Search.VisualSearch` pacote.

Instalar o [pacote NuGet Web pesquisa SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.VisualSearch/1.0) também instala as dependências, incluindo:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft

<a name="client"></a> 
## <a name="visual-search-client"></a>Cliente de pesquisa Visual
Para criar uma instância do `VisualSearchAPI` cliente, adicione as diretivas de a utilizar:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;

```
Em seguida, instanciar o cliente:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Utilize o cliente para imagens de pesquisa:
```
 System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
 // The knowledgeRequest parameter is not required if an image binary is passed in the request body
 var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;

```
Analisar os resultados da consulta anterior:
```
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }

    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags.First();
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions.First();
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }

```

<a name="complete"></a> 
## <a name="complete-console-application"></a>Aplicação de consola concluída

A aplicação de consola seguinte executa a consulta definida anteriormente e analisa os resultados:
```
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using System;
using System.IO;
using System.Linq;

namespace VisualSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            String subKey = "YOUR-SUBSCRIPTION-KEY";

            VisualSearchImageBinary(subKey);
            //VisualSearchImageBinaryWithCropArea(subKey);
            //VisualSearchUrlWithFilters(subKey);
            //VisualSearchInsightsTokenWithCropArea(subKey);
            //VisualSearchUrlWithJson(subKey);

            Console.WriteLine("\r\nAny key to quit...");
            Console.ReadKey();
        }


        // This will send an image binary in the body of the post request and print out the imageInsightsToken, 
        //  the number of tags, the number of actions, and the first actionType.

        public static void VisualSearchImageBinary(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The knowledgeRequest parameter is not required if an image binary is passed in the request body
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
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

Os exemplos de pesquisa do Bing demonstram diversas funcionalidades do SDK.  Adicione as seguintes funções para definida anteriormente `VisualSrchSDK` classe.

<a name="binary-crop"></a>
## <a name="image-binary-post-with-croparea"></a>Post binário de imagem com cropArea

O seguinte código envia uma imagem binário no corpo do pedido post, juntamente com um objeto de cropArea.  Em seguida, imprime o imageInsightsToken, o número de etiquetas, o número de ações e o actionType primeiro.

```
        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
                {
                    // The ImageInfo struct contains a crop area specifying a region to crop in the uploaded image
                    CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                    ImageInfo ImageInfo = new ImageInfo(cropArea: CropArea);
                    VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);

                    // The knowledgeRequest here holds additional information about the image, which is passed in in binary form
                    var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: VisualSearchRequest).Result;
                    Console.WriteLine("\r\nVisual search request with binary of image with knowledgeRequest of CropArea");

                    if (visualSearchResults == null)
                    {
                        Console.WriteLine("No visual search result data.");
                    }
                    else
                    {
                        // Visual Search results
                        if (visualSearchResults.Image?.ImageInsightsToken != null)
                        {
                            Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image insights token!");
                        }

                        // List of tags
                        if (visualSearchResults.Tags.Count > 0)
                        {
                            var firstTagResult = visualSearchResults.Tags.First();
                            Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                            // List of actions in first tag
                            if (firstTagResult.Actions.Count > 0)
                            {
                                var firstActionResult = firstTagResult.Actions.First();
                                Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                                Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                            }
                            else
                            {
                                Console.WriteLine("Couldn't find tag actions!");
                            }

                        }
                        else
                        {
                            Console.WriteLine("Couldn't find image tags!");
                        }
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
<a name="knowledge-req"></a>
## <a name="knowledgerequest-parameter"></a>Parâmetro KnowledgeRequest

O seguinte código envia um url de imagem `knowledgeRequest` parâmetro, juntamente com um \"site: www.bing.com\" filtro.  Em seguida, imprime os `imageInsightsToken`, o número de etiquetas, o número de ações e o actionType primeiro.
```
        public static void VisualSearchUrlWithFilters(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via URL, in the ImageInfo object
                var ImageUrl = "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80";
                ImageInfo ImageInfo = new ImageInfo(url: ImageUrl);

                // Optional filters inside the knowledgeRequest will restrict similar products and images to certain domains
                Filters Filters = new Filters(site: "www.bing.com");
                KnowledgeRequest KnowledgeRequest = new KnowledgeRequest(filters: Filters);

                // An image binary is not necessary here, as the image is specified via URL
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo, knowledgeRequest: KnowledgeRequest);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with url of image and knowledgeRequest based on filters");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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

```
<a name="tags-actions"></a>
## <a name="tags-actions-and-actiontype"></a>As etiquetas, ações e actionType

O seguinte código envia um token de informações de imagem no parâmetro knowledgeRequest, juntamente com um objeto de cropArea.  Em seguida, imprime o imageInsightsToken, o número de etiquetas, o número de ações e o actionType primeiro.

```
        public static void VisualSearchInsightsTokenWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object
                var ImageInsightsToken = "bcid_113F29C079F18F385732D8046EC80145*ccid_oV/QcH95*mid_687689FAFA449B35BC11A1AE6CEAB6F9A9B53708*thid_R.113F29C079F18F385732D8046EC80145";

                // An optional crop area can be passed in to define a region of interest in the image
                CropArea CropArea = new CropArea(top: (float)0.1, bottom: (float)0.5, left: (float)0.1, right: (float)0.9);
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: ImageInsightsToken, cropArea: CropArea);

                // An image binary is not necessary here, as the image is specified via insights token
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(imageInfo: ImageInfo);
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with ImageInsightsToken and knowledgeRequest based on imageInfo of cropArea");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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
```
<a name="num-tags-actions"></a>
## <a name="number-of-tags-number-of-actions-and-first-actiontype"></a>Número de etiquetas, número de ações e actionType primeiro
O seguinte código envia um url de imagem no parâmetro knowledgeRequest, juntamente com uma área de cortar.  Em seguida, imprime o imageInsightsToken, o número de etiquetas, o número de ações e o actionType primeiro.
```
        public static void VisualSearchUrlWithJson(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {

                //The visual search request can be passed in as a JSON string
                //The image is specified via URL in the ImageInfo object, along with a crop area as shown below:
                
                //     "imageInfo": {
                //         "url": "https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80",
                //     "cropArea": {
                //           "top": 0.1,
                //           "bottom": 0.5,
                //           "left": 0.1,
                //           "right": 0.9
                //         }
                //     },
                //     "knowledgeRequest": {
                //        "filters": {
                //            "site": "www.bing.com"
                //        }              
                //     }

                var VisualSearchRequestJSON = "{\"imageInfo\":{\"url\":\"https://images.unsplash.com/photo-1512546148165-e50d714a565a?w=600&q=80\",\"cropArea\":{\"top\":0.1,\"bottom\":0.5,\"left\":0.1,\"right\":0.9}},\"knowledgeRequest\":{\"filters\":{\"site\":\"www.bing.com\"}}}";

                // An image binary is not necessary here, as the image is specified by URL in JSON text
                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequestJSON).Result;
                Console.WriteLine("\r\nVisual search request with image url specified by JSON text");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {
                        var firstTagResult = visualSearchResults.Tags.First();
                        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

                        // List of actions in first tag
                        if (firstTagResult.Actions.Count > 0)
                        {
                            var firstActionResult = firstTagResult.Actions.First();
                            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
                            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
                        }
                        else
                        {
                            Console.WriteLine("Couldn't find tag actions!");
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
```

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK do .NET Services cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7).