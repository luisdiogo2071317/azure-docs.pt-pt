---
title: 'Início rápido: Procure imagens usando o SDK de pesquisa de imagens do Bing e do c#'
description: Utilize este guia de introdução para procurar e localizar imagens na web com o SDK de pesquisa de imagens do Bing e c#.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 9e0decb29224b5ad684e1242b8f93e091e08e6b6
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579255"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-c"></a>Início rápido: Pesquisa de imagens com o SDK de pesquisa de imagens do Bing e do c#

Utilize este guia de introdução para tornar a sua primeira pesquisa de imagens com o SDK de pesquisa de imagens do Bing, que é um wrapper para a API e contém os mesmos recursos. Esta aplicação de c# simple envia uma consulta de pesquisa de imagem, analisa a resposta JSON e apresenta o URL da imagem do primeiro devolvido.

O código-fonte para este exemplo está disponível [no GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) com tratamento de erros adicionais e anotações.

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição dos [Visual Studio 2017](https://visualstudio.microsoft.com/vs/whatsnew/).
* O [pacote NuGet de pesquisa de imagem cognitivos](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0).

Para instalar o SDK de pesquisa de imagens do Bing no visual studio, utilize o `Manage NuGet Packages` opção a partir do Explorador de soluções no Visual Studio.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

Primeiro, crie um novo aplicativo de console do c# no Visual Studio. Em seguida, adicione os seguintes pacotes ao seu projeto.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

O método principal de seu projeto, crie variáveis para a sua chave de subscrição válido, os resultados de imagem a ser devolvida pelo Bing e um termo de pesquisa. Em seguida, instancio o cliente de pesquisa de imagem a utilizar a chave.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";
//initialize the client
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Enviar uma consulta de pesquisa com o cliente

Utilize o cliente para pesquisar com um texto de consulta:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Analisar e visualizar o primeiro resultado de imagem

Analisar os resultados de imagem devolvidos na resposta.
Se a resposta contém os resultados da pesquisa, armazenar o primeiro resultado e imprimir os detalhes, como uma miniatura devolveu o URL, o URL original, juntamente com o número total de imagens.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte também 

* [O que é a pesquisa de imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Tente uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso de serviços cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Amostras do .NET para o SDK dos serviços cognitivos do Azure](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Documentação dos serviços cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)