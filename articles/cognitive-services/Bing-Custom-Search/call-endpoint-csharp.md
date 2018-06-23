---
title: Ponto final da chamada utilizando c# - pesquisa de personalizados do Bing - Microsoft serviços cognitivos
description: Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada utilizando c# para chamar o ponto final de pesquisa personalizados do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: be4cc79d16b9a22124f16878b11ca04a916f98ae
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352856"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Ponto final de pesquisa do Bing personalizada de chamada (c#)

Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada utilizando c# para chamar o ponto final de pesquisa personalizados do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

-  Uma instância de pesquisa personalizada prontos a utilizar. Consulte [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) instalado.
- A [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.  

  >[!NOTE]  
  >Os clientes de pesquisa do Bing personalizado existentes que tem uma chave de pré-visualização aprovisionada no ou antes do dia 15 de Outubro de 2017 poderá utilizar as respetivas chaves até 30 de Novembro de 2017, ou até que possam tem esgotado o número máximo de consultas permitido. Seguidamente, precisam de migrar para a versão geralmente disponível no Azure. 
 
## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Crie uma pasta para o seu código.
2. Uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Execute os seguintes comandos:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
   ```

4. Copie o seguinte código a Program.cs.
5. Substitua **sua-SUBSCRIPTION-KEY** e **sua-personalizada-CONFIG-ID** com o ID de chave e a configuração.

    ``` CSharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    
    namespace bing_custom_search_example_dotnet
    {
        class Program
        {
            static void Main(string[] args)
            {
                var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
                var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
                var searchTerm = args.Length > 0 ? args[0]: "microsoft";            
    
                var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                    "q=" + searchTerm +
                    "&customconfig=" + customConfigId;
    
                var client = new HttpClient();
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                var httpResponseMessage = client.GetAsync(url).Result;
                var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
                BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
                
                for(int i = 0; i < response.webPages.value.Length; i++)
                {                
                    var webPage = response.webPages.value[i];
                    
                    Console.WriteLine("name: " + webPage.name);
                    Console.WriteLine("url: " + webPage.url);                
                    Console.WriteLine("displayUrl: " + webPage.displayUrl);
                    Console.WriteLine("snippet: " + webPage.snippet);
                    Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
                    Console.WriteLine();
                }            
            }
        }
    
        public class BingCustomSearchResponse
        {        
            public string _type{ get; set; }            
            public WebPages webPages { get; set; }
        }
    
        public class WebPages
        {
            public string webSearchUrl { get; set; }
            public int totalEstimatedMatches { get; set; }
            public WebPage[] value { get; set; }        
        }
    
        public class WebPage
        {
            public string name { get; set; }
            public string url { get; set; }
            public string displayUrl { get; set; }
            public string snippet { get; set; }
            public DateTime dateLastCrawled { get; set; }
            public string cachedPageUrl { get; set; }
            public OpenGraphImage openGraphImage { get; set; }        
        }
        
        public class OpenGraphImage
        {
            public string contentUrl { get; set; }
            public int width { get; set; }
            public int height { get; set; }
        }
    }
    ```
6. Crie a aplicação utilizando o seguinte comando. Tenha em atenção o caminho da dll referenciado pela saída do comando.
    <pre>
    dotnet build 
    </pre>
7. Executar a aplicação utilizando o seguinte comando substituir **caminho a saída** com o caminho referenciado pelo passo de compilação.
    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)
