---
title: 'Início rápido: SDK de Pesquisa Personalizada, C#'
titleSuffix: Azure Cognitive Services
description: Configurar a aplicação da consola C# no SDK de Pesquisa Personalizada.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 6dc10bc2dedfe99573b5ad646461e66827c6df9e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320174"
---
# <a name="quickstart-using-the-bing-custom-search-sdk-with-c"></a>Início Rápido: Utilizar o SDK de Pesquisa Personalizada do Bing com C#

O SDK de Pesquisa Personalizada do Bing fornece um modelo de programação mais simples do que a API REST da Pesquisa Personalizada do Bing. Esta secção explica como fazer as suas primeiras chamadas de Pesquisa Personalizada com o SDK com C#.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada e pronta a utilizar. Consulte [Criar a sua primeira instância de Pesquisa Personalizada do Bing](quick-start.md).  
  
- Uma chave de subscrição. Pode obter uma chave de subscrição quando ativa a sua [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ou pode utilizar uma chave de subscrição paga do dashboard do Azure (consulte [Conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  
  
- Visual Studio 2017 instalado. Se ainda não a tem, pode descarregar a versão **gratuita** [do Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).  
  
- O pacote de [Pesquisa Personalizada do NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)instalado. No Explorador de Soluções do Visual Studio, clique com o botão direito do rato no seu projeto e selecione `Manage NuGet Packages` no menu. Instale o pacote `Microsoft.Azure.CognitiveServices.Search.CustomSearch`.

Quando instala o pacote de Pesquisa Personalizada do NuGet também instala as assemblagens seguintes:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Executar o código

> [!TIP]
> Obter o código mais recente como uma solução do Visual Studio a partir do [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

Para executar este exemplo, siga estes passos:

1. Abra o Visual Studio 2017.
  
2. Clique no menu **Ficheiro**, clique em **Novo**, **Projeto** e, em seguida, no modelo da **Aplicação da Consola Visual C#**.
  
3. Denomine a sua solução de CustomSearchSolution e navegue até à pasta para a colocar lá dentro.
  
4. Clique em OK para criar a solução.  
  
4. A partir do Explorador de Soluções, clique com o botão direito do rato no seu projeto (tem o mesmo nome que a solução) e selecione `Manage NuGet Packages`. Clique em **Procurar** no Gestor de Pacotes NuGet. Introduza Microsoft.Azure.CognitiveServices.Search.CustomSearch na caixa de pesquisa e carregue em Enter. Selecione o pacote e clique em Instalar.  
  
4. Copie o código seguinte para o ficheiro Program.cs. Substitua **YOUR-SUBSCRIPTION-KEY** e **YOUR-CUSTOM-CONFIG-ID** pela sua chave de subscrição e pelo ID de configuração.  
  
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




## <a name="breaking-it-down"></a>Reparti-la

Depois de instalar o pacote de Pesquisa Personalizada do NuGet , tem de adicionar uma diretiva de utilização para o mesmo.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Em seguida, crie uma instância do cliente de pesquisa personalizada, que pode utilizar para fazer pedidos de pesquisa. Certifique-se de que substitui `YOUR-SUBSCRIPTION-KEY` pela sua chave.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Agora, pode utilizar o cliente para enviar um pedido de pesquisa. Certifique-se de que substitui o seu `YOUR-CUSTOM-CONFIG-ID` pelo ID de configuração da sua instância (pode encontrar o ID no [portal de Pesquisa Personalizada](https://www.customsearch.ai/)). Este exemplo efetua uma pesquisa da Xbox. Atualize conforme necessário.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

O método `SearchAsync`devolve um objeto `WebData`. Utilize `WebData` para iterar através de quaisquer `WebPages` que tenham sido detetados. Este código localiza o primeiro resultado de página Web, imprime-a `Name` e `URL`.

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


## <a name="next-steps"></a>Passos seguintes

Veja exemplos do SDK. Cada exemplo inclui um arquivo Leia-me com detalhes sobre os pré-requisitos e instalação/execução dos exemplos.

* Introdução aos [exemplos de .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [Pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Consulte também [bibliotecas .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) para definições e dependências.
* Introdução aos [exemplos de NodeJS](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte também [bibliotecas NodeJS](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) para definições e dependências.
* Introdução aos [exemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Veja também [bibliotecas Java](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) para obter as definições e as dependências.
* Introdução aos [exemplos de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte também [bibliotecas Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) para definições e dependências.

