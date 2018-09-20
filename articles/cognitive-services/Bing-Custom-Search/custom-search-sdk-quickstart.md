---
title: Personalizado pesquisa SDK C# guia de introdução | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Configure a aplicação de consola personalizado pesquisa SDK c#.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 6b41dfbde0c2af776ee2c35220f731e40de334a0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367853"
---
# <a name="custom-search-sdk-c-quickstart"></a>Início rápido de C# do SDK de Pesquisa Personalizada

O SDK de pesquisa personalizada do Bing contém a funcionalidade da API REST para pesquisa de entidades e os resultados de análise.

O código-fonte para este exemplo está disponível a partir [Github](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).
## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola com o SDK de pesquisa personalizada do Bing, navegue para o `Manage NuGet Packages` opção a partir do Explorador de soluções no Visual Studio. Adicione o pacote `Microsoft.Azure.CognitiveServices.Search.CustomSearch`.

Instalar o [NuGet a pesquisa personalizada](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pacote também instala as dependências, incluindo os assemblies a seguir:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Cliente de pesquisa de entidades

Para criar uma instância do cliente CustomSearchAPI, adicione com diretivas:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Criar uma instância do cliente de pesquisa personalizada: substituir `YOUR-CUSTOM-SEARCH-KEY` e `YOUR-CUSTOM-CONFIG-ID` com a sua chave de acesso e a configuração de ponto final de API ID atribuído ao [instâncias My](https://www.customsearch.ai/).
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
Utilize o cliente para pesquisar com um texto de consulta:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Analisar os resultados

O `SearchAsync` método devolve um `WebData` objeto que contém `WebPages` caso algum seja localizado para a consulta. Esse código encontra o primeiro resultado e obtém seu `Name` e `URL`.
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
 
Console.WriteLine("Searched for Query# \" Xbox \"");

 //WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Couldn't find web results!");
    }
}
else
{
    Console.WriteLine("Didn't see any Web data..");
}

```
## <a name="complete-console-application"></a>Concluir a aplicação de consola

O código a seguir procura na consulta "Xbox" e imprime `Name` e `URL` para o primeiro resultado da web.
```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

namespace CustomSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {

            var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

            try
            {
                // This will look up a single query (Xbox).
                var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
                Console.WriteLine("Searched for Query# \" Xbox \"");

                //WebPages
                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find web results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}


```

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK .NET nos serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
