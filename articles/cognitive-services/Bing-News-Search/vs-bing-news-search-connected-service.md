---
title: Bing notícias pesquisa tutorial de c# | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Ligar-se para de pesquisa de notícias do Bing dos serviços cognitivos a partir de uma aplicação web ASP.NET Core.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 5cfa82067d28b05f32bd87e0e83d55a03da8d508
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095434"
---
# <a name="connect-to-bing-news-search-api-by-using-connected-services-in-visual-studio"></a>Ligar à API de pesquisa de notícias do Bing com serviços ligados no Visual Studio

Ao utilizar a pesquisa de notícias do Bing, pode ativar a aplicações e serviços para tirar partido do poder de um motor de pesquisa sem anúncios de âmbito para a web. Pesquisa de notícias do Bing é um dos serviços de pesquisa disponíveis com os serviços cognitivos.

Este artigo fornece detalhes para utilizar a funcionalidade de serviço ligado do Visual Studio para pesquisa de notícias do Bing. A capacidade está disponível no 15.7 de 2017 do Visual Studio, ou posterior, com a extensão de serviços cognitivos instalada.

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 versão 15.7, com a carga de trabalho de desenvolvimento para a Web instalada. [Baixe agora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-bing-news-search-api"></a>Adicionar suporte ao seu projeto para a API de pesquisa de notícias do Bing

1. Crie um novo projeto de web do ASP.NET Core com o nome MyWebApplication. Utilize o **aplicação Web (Model-View-Controller)** modelo de projeto, com todas as predefinições. É importante nomear o projeto MyWebApplication, para que o espaço de nomes corresponde ao copiar o código no projeto. 

1. Na **Explorador de soluções**, escolha **Add** > **serviço ligado**.
   É apresentada a página de serviço ligado, com os serviços que pode adicionar ao seu projeto.

   ![Item de menu de captura de ecrã de Adicionar serviço ligado](../media/vs-common/Connected-Service-Menu.PNG)

1. No menu de serviços disponíveis, escolha **traga inteligente pesquisa para seus aplicativos**.

   ![Captura de ecrã da lista de serviços ligados](./media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-0.PNG)

   Se já tem sessão iniciada no Visual Studio e uma subscrição do Azure associado à sua conta, é apresentada uma página com uma lista suspensa com as suas subscrições. Selecione a subscrição que pretende utilizar e, em seguida, escolha um nome para a API de pesquisa de notícias do Bing. Também pode escolher **editar** para modificar o nome gerado automaticamente.

   ![Captura de ecrã da subscrição e o nome de campos](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-1.PNG)

1. Escolha o grupo de recursos e o escalão de preço.

   ![Captura de ecrã do grupo de recursos e os campos de escalão de preço](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-2.PNG)

   Se pretender obter mais detalhes sobre os escalões de preços, selecione **preços de revisão**.

1. Escolher **adicionar** para adicionar suporte para o serviço ligado.
   Visual Studio modifica seu projeto para adicionar os pacotes NuGet, entradas de ficheiro de configuração e outras alterações para dar suporte a uma conexão com a API de pesquisa de notícias do Bing. A saída mostra o log de que está a acontecer ao seu projeto. Deverá ver algo semelhante ao seguinte:

   ```output
   [5/4/2018 12:41:21.084 PM] Adding Intelligent Search to the project.
   [5/4/2018 12:41:21.271 PM] Creating new Intelligent Search...
   [5/4/2018 12:41:24.128 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.ImageSearch' version 1.2.0...
   [5/4/2018 12:41:24.135 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.NewsSearch' version 1.2.0...
   [5/4/2018 12:41:24.154 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.WebSearch' version 1.2.0...
   [5/4/2018 12:41:24.168 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.CustomSearch' version 1.2.0...
   [5/4/2018 12:41:24.187 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.VideoSearch' version 1.2.0...
   [5/4/2018 12:42:07.287 PM] Retrieving keys...
   [5/4/2018 12:42:07.741 PM] Updating appsettings.json setting: 'ServiceKey' = 'c271412f3e4c4e1dacc7c4145fa0572a'
   [5/4/2018 12:42:07.745 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://api.cognitive.microsoft.com/bing/v7.0'
   [5/4/2018 12:42:07.749 PM] Updating appsettings.json setting: 'Name' = 'WebApplicationCore-Search_IntelligentSearch'
   [5/4/2018 12:42:10.217 PM] Successfully added Intelligent Search to the project.
   ```

   O ficheiro appSettings agora contém as seguintes definições novas:

   ```json
   "CognitiveServices": {
     "IntelligentSearch": {
       "ServiceKey": "<your service key>",
       "ServiceEndPoint": "https://api.cognitive.microsoft.com/bing/v7.0",
       "Name": "WebApplicationCore-Search_IntelligentSearch"
     }
   }
   ```
 
## <a name="use-the-bing-news-search-api-to-add-search-functionality-to-a-web-page"></a>Utilize a API de pesquisa de notícias do Bing para adicionar funcionalidade de pesquisa a uma página da web

Agora que adicionou suporte para a API de pesquisa de notícias do Bing ao seu projeto, eis como utilizar a API para adicionar a pesquisa inteligente para uma página da web.

1.  Na *Startup.cs*, na `ConfigureServices` método, adicione uma chamada para `IServiceCollection.AddSingleton`. Desta forma, o objeto de configuração que contém as definições de chaves disponíveis para o código no seu projeto.
 
   ```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }
   ```


1. Adicione um novo arquivo de classe sob o **modelos** pasta, chamada *BingNewsModel.cs*. Se chama o seu projeto de forma diferente, utilize o espaço de nomes do seu próprio projeto, em vez de MyWebApplication. Substitua os conteúdos com o código a seguir:
 
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch.Models;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    
    namespace MyWebApplication.Models
    {
        public class BingNewsModel
        {
            public News SearchResult { get; set; } 
            public string SearchText { get; set; }
        }
    }
    ```

   Este modelo é utilizado para armazenar os resultados de uma chamada para o serviço de pesquisa de notícias do Bing.
 
1. Na **controladores** pasta, adicione um novo arquivo de classe chamado *IntelligentSearchController.cs*. Substitua os conteúdos com o código a seguir:

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using MyWebApplication.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    using Microsoft.Extensions.Configuration;
    
    namespace MyWebApplication.Controllers
    {
        // A controller to handle News Search requests
        public class IntelligentSearchController : Controller
        {
            private IConfiguration configuration;
  
            // Set up the configuration that contains the keys
            // (from the appsettings.json file)
            // that you will use to access the service  
            public IntelligentSearchController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }

            // Call the Bing News Search API and put the result in the model object.    
            public async Task<IActionResult> BingSearchResult(BingNewsModel model)
            {
                if (!string.IsNullOrWhiteSpace(model.SearchText))
                { 
                    INewsSearchAPI client = this.GetNewsSearchClient(new MyHandler());
                    model.SearchResult = await client.News.SearchAsync(model.SearchText);
                }
                return View(model);
            }
    
            // Forward requests to the Search endpoint to the BingSearchResult method
            [HttpPost("Search")]
            public IActionResult Search(BingNewsModel model)
            {
                return RedirectToAction("BingSearchResult", model);
            }

            // Get the search client object
            private INewsSearchAPI GetNewsSearchClient(DelegatingHandler handler)
            {
                string key =
                   configuration.GetSection("CognitiveServices")["IntelligentSearch:ServiceKey"];
    
                INewsSearchAPI client = new NewsSearchAPI(
                   new ApiKeyServiceClientCredentials(key), handlers: handler);
    
                return client;
            }
        }
    }
   ```

   Nesse código, o construtor configura o objeto de configuração que contém as chaves. O método para o `Search` rota é apenas um redirecionamento para o `BingSearchResult` função. Isso chama o `GetNewsSearchClient` método para obter o `NewsSearchAPI` objeto cliente.  O `NewsSearchAPI` contém o objeto de cliente do `SearchAsync` método, que, na verdade, chama o serviço e retorna os resultados no `SearchResult` modelo que acabou de criar. 

1. Adicionar uma classe, `MyHandler`, que foi referenciado no código anterior. Delega a chamada assíncrona para o serviço de pesquisa à sua classe base, `DelegatingHandler`.

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Threading;

    class MyHandler : DelegatingHandler
    {
        protected async override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Call the inner handler.
            var response = await base.SendAsync(request, cancellationToken);
            
            return response;
        }
    }
   ```

1. Para adicionar suporte para o envio de pesquisas e visualizando os resultados, na **vistas** pasta, criar uma nova pasta chamada **IntelligentSearch**. Nessa nova pasta, adicionar uma exibição *BingSearchResult.cshtml*. Copie o código a seguir:

    ```cshtml
    @using System
    @model MyWebApplication.Models.BingNewsModel
    
    @{
        ViewData["Title"] = "BingSearchResult";
    }
    
    <h2>Search News</h2>
    
    <div class="row">
        <section>
            <form asp-controller="IntelligentSearch" asp-action="Search" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width ="90%">
                    <tr>
                        <td>
                            <input type="text" name="SearchText" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Search</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    <h2>Search Result</h2=
    <table>
    @if (!string.IsNullOrEmpty(Model.SearchText)) {
        foreach (var item in Model.SearchResult.Value) {
        <tr>
            <td rowspan="2" width="90">
               <img src=@item?.Image?.Thumbnail?.ContentUrl width="80" height="80" />
            </td>
            <td><a href=@item.Url>@item.Name</a></td>
        </tr>   
        <tr>
            <td>@item.Description</td>
        </tr>
        <tr height="10">
            <td/><td/>
         </tr>
        } }
     </table>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    ```

1. Iniciar a aplicação web localmente, introduza o URL para a página que acabou de criar (/ IntelligentSearch/BingSearchResult) e publicar um pedido de pesquisa usando o botão de pesquisa.

   ![Resultados de captura de ecrã de pesquisa do Bing notícias](media/vs-bing-news-search-connected-service/Cog-News-Search-Results.PNG)
           
## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário o grupo de recursos, pode eliminá-la. Esta ação elimina o serviço cognitivo e recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. Introduza o nome do seu grupo de recursos na caixa de pesquisa na parte superior do portal. Selecione o grupo de recursos que pretende eliminar.
2. Selecione **Eliminar grupo de recursos**.
3. Na **escreva o nome do grupo de recursos** caixa, introduza o nome do grupo de recursos e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a API de pesquisa de notícias do Bing, veja [o que é a pesquisa de notícias do Bing?](index.yml).
