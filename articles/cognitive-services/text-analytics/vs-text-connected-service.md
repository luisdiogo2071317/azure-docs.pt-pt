---
title: Tutorial de texto Analytics c# | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Ligar à análise de texto a partir de uma aplicação web ASP.NET Core.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: ghogen
ms.openlocfilehash: c97f75e0a41a4bf314963dc26c6424a0b773822b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665235"
---
# <a name="connect-to-the-text-analytics-service-by-using-connected-services-in-visual-studio"></a>Ligar para o serviço de análise de texto com os serviços ligados no Visual Studio

Ao utilizar o serviço de análise de texto, pode extrair informações importantes para categorizar e processar dados visuais e realizar moderação assistida por computador de imagens para ajudar a melhorar os seus serviços.

Este artigo e os respetivos artigos complementar fornecem detalhes para utilizar a funcionalidade de serviço ligado do Visual Studio para o serviço de análise de texto. A capacidade está disponível em ambos os 15.7 de 2017 Visual Studio ou posterior, com a extensão de serviços cognitivos instalada.

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 versão 15.7, com a carga de trabalho de desenvolvimento para a Web instalada. [Baixe agora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-the-text-analytics-service"></a>Adicionar suporte ao seu projeto para o serviço de análise de texto

1. Crie um novo projeto de web do ASP.NET Core chamado TextAnalyticsDemo. Utilize o modelo de projeto de aplicativo Web (Model-View-Controller) com todas as predefinições. É importante nomear o projeto MyWebApplication, para que o espaço de nomes corresponde ao copiar o código no projeto.  O exemplo este artigo utiliza o MVC, mas pode utilizar o serviço de ligado de análise de texto com qualquer tipo de projeto do ASP.NET.

1. Na **Explorador de soluções**, faça duplo clique no **serviço ligado** item.
   É apresentada a página de serviço ligado, com os serviços que pode adicionar ao seu projeto.

   ![Captura de ecrã do serviço ligado no Explorador de soluções](../media/vs-common/Connected-Services-Solution-Explorer.PNG)

1. No menu de serviços disponíveis, escolha **avaliar sentimentos com análise de texto**.

   ![Ecrã de captura de ecrã de serviços ligados](./media/vs-text-connected-service/Cog-Text-Connected-Service-0.PNG)

   Se já tem sessão iniciada no Visual Studio e uma subscrição do Azure associado à sua conta, é apresentada uma página com uma lista suspensa com as suas subscrições.

   ![Ecrã de captura de ecrã de texto ligados serviço de análise](media/vs-text-connected-service/Cog-Text-Connected-Service-1.PNG)

1. Selecione a subscrição que pretende utilizar e, em seguida, escolha um nome para o serviço de análise de texto ou escolha o **editar** ligação para modificar o nome gerado automaticamente, selecione o grupo de recursos e o escalão de preço.

   ![Captura de ecrã do grupo de recursos e os campos de escalão de preço](media/vs-text-connected-service/Cog-Text-Connected-Service-2.PNG)

   Siga a ligação para obter detalhes sobre os escalões de preço.

1. Escolher **adicionar** para adicionar suporte para o serviço ligado.
   Visual Studio modifica seu projeto para adicionar os pacotes NuGet, entradas de ficheiro de configuração e outras alterações para dar suporte a uma ligação para o serviço de análise de texto. O **janela de saída** mostra o registo das que está a acontecer ao seu projeto. Deverá ver algo semelhante ao seguinte:

   ```output
    [6/1/2018 3:04:02.347 PM] Adding Text Analytics to the project.
    [6/1/2018 3:04:02.906 PM] Creating new Text Analytics...
    [6/1/2018 3:04:06.314 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Language' version 1.0.0-preview...
    [6/1/2018 3:04:56.759 PM] Retrieving keys...
    [6/1/2018 3:04:57.822 PM] Updating appsettings.json setting: 'ServiceKey' = '<service key>'
    [6/1/2018 3:04:57.827 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0'
    [6/1/2018 3:04:57.832 PM] Updating appsettings.json setting: 'Name' = 'TextAnalyticsDemo'
    [6/1/2018 3:05:01.840 PM] Successfully added Text Analytics to the project.
    ```
 
## <a name="use-the-text-analytics-service-to-detect-the-language-for-a-text-sample"></a>Utilize o serviço de análise de texto para detetar o idioma para obter um exemplo de texto.

1. Adicione as seguintes instruções "using" no Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   ```
 
1. Adicione um campo de configuração e adicione um construtor que inicializa o campo de configuração na classe de inicialização para ativar a configuração em seu programa.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Adicione um arquivo de classe na pasta controladores chamada DemoTextAnalyzeController e substitua o respetivo conteúdo pelo seguinte código:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Extensions.Configuration;
    using TextAnalyticsDemo.Models;
    
    namespace TextAnalyticsDemo.Controllers
    {
        public class DemoTextAnalyzeController : Controller
        {
            private IConfiguration configuration;
    
            public DemoTextAnalyzeController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }
    
            public IActionResult TextAnalyzeResult(TextAnalyzeModel model)
            {
    
                if (!string.IsNullOrWhiteSpace(model.TextStr))
                {
                    ITextAnalyticsAPI client = this.GetTextAnalyzeClient(new MyHandler());
                    model.AnalyzeResult = client.DetectLanguage(
                        new BatchInput(
                            new List<Input>()
                            {
                                new Input("id",model.TextStr)
                            }));
                }
                return View(model);
            }
    
            [HttpPost("Analyze")]
            public IActionResult Analyze(TextAnalyzeModel model)
            {
                return RedirectToAction("TextAnalyzeResult", model);
            }
    
            // Using the ServiceKey from the configuration file,
            // get an instance of the Text Analytics client.
            private ITextAnalyticsAPI GetTextAnalyzeClient(DelegatingHandler handler)
            {
                string key = configuration.GetSection("CognitiveServices")["TextAnalytics:ServiceKey"];
    
                ITextAnalyticsAPI client = new TextAnalyticsAPI( handlers: handler);
                client.SubscriptionKey = key;
                client.AzureRegion = AzureRegions.Westus;
    
                return client;
            }
        }
    }
    ```
    
    O código inclui GetTextAnalyzeClient para instalar o cliente objeto que pode ser usado para chamar a API de análise de texto e um manipulador de solicitação que chama DetectLanguage num determinado texto.

1. Adicione a classe de programa auxiliar de MyHandler que é utilizada pelo código anterior.

    ```csharp
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

1. Na pasta Modelos, adicione uma classe para o modelo.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    
    namespace Demo.Models
    {
        public class TextAnalyzeModel
        {
            public string TextStr { get; set; }
    
            public LanguageBatchResult AnalyzeResult { get; set; }
    
            public SentimentBatchResult AnalyzeResult2 { get; set; }
        }
    }
    ```

1. Adicione a vista que mostra o texto analisado, o idioma de determinado e a pontuação que representa o nível de confiança na análise.
    
    ```cshtml
    @using System
    @model Demo.Models.TextAnalyzeModel
    
    @{
        ViewData["Title"] = "TextAnalyzeResult";
    }
    
    <h2>Text Language</h2>
    
    <div class="row">
        <section>
            <form asp-controller="DemoTextAnalyze" asp-action="Analyze" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width="90%">
                    <tr>
                        <td>
                            <input type="text" name="TextStr" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Analyze</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    
    <h2>Result</h2>
    <div>
        <dl class="dl-horizontal">
            <dt>
                Text :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.TextStr)
            </dd>
            <dt>
                Language Name :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Name)
            </dd>
            <dt>
                Score :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Score)
            </dd>
        </dl>
    </div>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    
    ```
 
1. Crie e execute o exemplo localmente. Introduza algum texto e ver que idioma Deteta de análise de texto.
   
## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos. Esta ação elimina o serviço cognitivo e recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste tutorial nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o serviço de análise de texto com a leitura a [documentação do serviço de análise de texto](index.yml).
