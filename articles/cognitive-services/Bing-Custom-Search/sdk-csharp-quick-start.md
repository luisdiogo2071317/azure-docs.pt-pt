---
title: Personalizado pesquisa SDK C# guia de introdução | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Configure a aplicação de consola personalizado pesquisa SDK c#.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6c9917e3a63515f36b386e444edcc53de07799fc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949932"
---
# <a name="c-sdk-quickstart"></a>Início rápido do SDK do c#

O SDK de pesquisa personalizada do Bing fornece um modelo de programação mais fácil do que a API de REST de pesquisa do Bing personalizado. Esta secção explica como fazer sua primeira chamadas de pesquisa personalizada com o SDK c#.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada do prontos a utilizar. Ver [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).  
  
- Uma chave de subscrição. Pode obter uma chave de subscrição ao ativar o seu [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), ou pode utilizar uma chave de subscrição paga do dashboard do Azure (consulte [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  
  
- Visual Studio 2017 instalado. Se não o tiver, pode baixar o **gratuita** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).  
  
- O [NuGet a pesquisa personalizada](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) pacote instalado. No Explorador de soluções no Visual Studio, faça duplo clique no seu projeto e selecione `Manage NuGet Packages` no menu. Instale o pacote `Microsoft.Azure.CognitiveServices.Search.CustomSearch`.

Também é instalar o pacote NuGet a pesquisa personalizada instala as assemblagens seguintes:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Abra o Visual Studio 2017.
  
2. Clique a **arquivo** menu, clique em **New**, **projeto**e, em seguida, o **Visual c# Console Application** modelo.
  
3. Nomeie a sua solução CustomSearchSolution e navegue até à pasta colocá-lo na.
  
4. Clique em OK para criar a solução.  
  
4. No Explorador de soluções, faça duplo clique no seu projeto (tem o mesmo nome que a solução) e selecione `Manage NuGet Packages`. Clique em **procurar** no Gestor de pacotes NuGet. Introduza Microsoft.Azure.CognitiveServices.Search.CustomSearch na caixa de pesquisa e prima enter. Selecione o pacote e clique em instalar.  
  
4. Copie o seguinte código para o ficheiro Program.cs. Substitua **seu-SUBSCRIPTION-KEY** e **seu-personalizada-CONFIG-ID** com sua chave de subscrição e a configuração de ID.  
  
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

    namespace CustomSrchSDK
    {
        class Program
        {
            static void Main(string[] args)
            {

                var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));

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
                            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
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

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

        }
    }
    ```  
  
5. Compilar e executar a solução (depuração). 




## <a name="breaking-it-down"></a>Segmentar a Modelagem

Depois de instalar o pacote NuGet a pesquisa personalizada, precisa adicionar uma diretiva para o mesmo.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Em seguida, crie uma instância do cliente de pesquisa personalizada, que utiliza para fazer pedidos de pesquisa. Certifique-se de que substitua `YOUR-SUBSCRIPTION-KEY` com a sua chave.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Agora, pode utilizar o cliente para enviar uma solicitação de pesquisa. Certifique-se de que substitua seu `YOUR-CUSTOM-CONFIG-ID` com o ID de configuração da sua instância (pode encontrar o ID no [portal da pesquisa personalizada](https://www.customsearch.ai/)). Neste exemplo procura Xbox. Atualização conforme necessário.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

O `SearchAsync` método devolve um `WebData` objeto. Uso `WebData` para iterar por meio de qualquer `WebPages` que foram encontrados. Esse código localiza o primeiro resultado de página Web e imprime a página da Web `Name` e `URL`.

```csharp
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


## <a name="next-steps"></a>Passos Seguintes

Veja exemplos do SDK. Cada exemplo inclui um arquivo Leiame com detalhes sobre os pré-requisitos e instalação/em execução os exemplos.

* Introdução ao [amostras do .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [Pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Consulte também [bibliotecas .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) para definições e as dependências.
* Introdução ao [exemplos de NodeJS](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte também [bibliotecas de NodeJS](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) para definições e as dependências.
* Introdução ao [exemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Consulte também [bibliotecas Java](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) para definições e as dependências.
* Introdução ao [amostras de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte também [bibliotecas Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) para definições e as dependências.

