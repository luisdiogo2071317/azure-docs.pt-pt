---
title: Personalizado pesquisa SDK c# guia de introdução | Microsoft Docs
titleSuffix: Cognitive Services
description: Configure a aplicação de consola personalizada pesquisa SDK c#.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 01/31/2018
ms.author: rosh
ms.openlocfilehash: 59b208b53bec974433c50c0e2304dc96bd9bd09e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351422"
---
# <a name="custom-search-sdk-c-quickstart"></a>Personalizado pesquisa SDK c# início rápido

O SDK de pesquisa do Bing personalizado contém as funcionalidades da API REST para pesquisa de entidade e analisar os resultados.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing personalizada, navegue para o `Manage NuGet Packages` opção do Explorador de soluções no Visual Studio. Adicionar o `Microsoft.Azure.CognitiveServices.Search.CustomSearch` pacote.

Instalar o [NuGet personalizada pesquisa](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pacote também instala as dependências, incluindo as assemblagens seguintes:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft

## <a name="entity-search-client"></a>Cliente de pesquisa de entidade

Para criar uma instância do cliente CustomSearchAPI, adicione as diretivas de a utilizar:
```
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

```

Instanciar o cliente de pesquisa personalizada: substituir `YOUR-CUSTOM-SEARCH-KEY` e `YOUR-CUSTOM-CONFIG-ID` com a sua chave de acesso e a configuração de ponto final de API ID atribuído ao [instâncias My](https://www.customsearch.ai/).
```
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));

```
Utilize o cliente para a pesquisa com um texto de consulta:
```
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;

```
## <a name="parse-the-results"></a>Analisar os resultados

O `SearchAsync` método devolve um `WebData` objeto que contenha `WebPages` caso algum seja localizado para a consulta. Este código localiza o resultado primeiro e obtém o respetivo `Name` e `URL`.
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
## <a name="complete-console-application"></a>Aplicação de consola concluída

O seguinte código procura na consulta "Xbox" e imprime `Name` e `URL` para primeiro resultado da web.
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

[Serviços cognitivos amostras de SDK do .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
