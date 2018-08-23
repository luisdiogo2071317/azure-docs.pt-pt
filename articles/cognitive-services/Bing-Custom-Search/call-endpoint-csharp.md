---
title: Chamar o ponto de extremidade usando o c# - pesquisa personalizada do Bing - serviços cognitivos da Microsoft
description: Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada com o c# para chamar o ponto final de pesquisa personalizada do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 87970e1c5e8487f9afca2acc680bdfeb610dc89f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "41987505"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Chamar o ponto final de pesquisa personalizada do Bing (c#)

Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada com o c# para chamar o ponto final de pesquisa personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

-  Uma instância de pesquisa personalizada do prontos a utilizar. Ver [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) instalado.
- R [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode usar uma chave de subscrição paga do dashboard do Azure.  

  >[!NOTE]  
  >Os clientes de pesquisa personalizada do Bing existentes que tem uma chave de pré-visualização aprovisionada em ou antes de 15 de Outubro de 2017 será capazes de utilizar as respetivas chaves até 30 de Novembro de 2017, ou até que eles tem esgotado o número máximo de consultas permitido. Em seguida, terá de migrar para a versão disponível em geral no Azure. 
 
## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Crie uma pasta para o seu código.
2. A partir de uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Execute os seguintes comandos:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
   ```

4. Copie o seguinte código a Program.cs.
5. Substitua **seu-SUBSCRIPTION-KEY** e **seu-personalizada-CONFIG-ID** com o ID de chave e a configuração.

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
6. Crie a aplicação com o seguinte comando. Tenha em atenção o caminho da dll referenciado pela saída do comando.

    <pre>
    dotnet build 
    </pre>
7. Executar o aplicativo usando o comando a seguir, substituindo **caminho para saída** com o caminho referenciado pelo passo de compilação.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de interface do Usuário alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)
