---
title: Tutorial de computador visão c# | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Ligar-se a visão do computador de serviços cognitivos a partir de uma aplicação web ASP.NET Core.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: ghogen
ms.openlocfilehash: 76ca1215144a5caa40971e1eda23f6462f7bf27b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665256"
---
# <a name="connecting-to-cognitive-services-computer-vision-api-by-using-connected-services-in-visual-studio"></a>Ligar à API de imagem digitalizada dos serviços cognitivos com os serviços ligados no Visual Studio

Ao utilizar a API de visão de computador de serviços cognitivos, pode extrair informações importantes para categorizar e processar dados visuais e realizar moderação assistida por computador de imagens para ajudar a melhorar os seus serviços.

Este artigo e os respetivos artigos complementar fornecem detalhes para utilizar a funcionalidade de serviço ligado do Visual Studio para a API de imagem digitalizada dos serviços cognitivos. A capacidade está disponível em ambos os 15.7 de 2017 Visual Studio ou posterior, com a extensão de serviços cognitivos instalada.

## <a name="prerequisites"></a>Pré-requisitos

- **Uma subscrição do Azure**. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versão 15.7** com o **desenvolvimento Web** instalada da carga de trabalho. [Baixe agora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-cognitive-services-computer-vision-api"></a>Adicionar suporte ao seu projeto para a API de imagem digitalizada dos serviços cognitivos

1. Crie um novo projeto de web do ASP.NET Core. Utilize o modelo de projeto vazio. 

1. Na **Explorador de soluções**, escolha **Add** > **serviço ligado**.
   Será exibida a página de serviço ligado com os serviços que pode adicionar ao seu projeto.

   ![Adicionar item de menu do serviço ligado](../media/vs-common/Connected-Service-Menu.PNG)

1. No menu de serviços disponíveis, escolha **API de imagem digitalizada dos serviços cognitivos**.

   ![Escolher o serviço para ligar a](./media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-0.PNG)

   Se já tem sessão iniciada no Visual Studio e uma subscrição do Azure associado à sua conta, é apresentada uma página com uma lista suspensa com as suas subscrições.

   ![Selecione a sua subscrição](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-1.PNG)

1. Selecione a subscrição que pretende utilizar e, em seguida, escolha um nome para a API de imagem digitalizada ou escolha a ligação de edição para modificar o nome gerado automaticamente, selecione o grupo de recursos e o escalão de preço.

   ![Editar detalhes do serviço ligado](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-2.PNG)

   Siga a ligação para obter detalhes sobre os escalões de preço.

1. Escolha adicionar para adicionar suporte para o serviço ligado.
   Visual Studio modifica seu projeto para adicionar os pacotes NuGet, entradas de ficheiro de configuração e outras alterações para dar suporte a uma ligação a API de imagem digitalizada. A janela de saída mostra o log de que está a acontecer ao seu projeto. Deverá ver algo semelhante ao seguinte:

   ```output
   [4/26/2018 5:15:31.664 PM] Adding Computer Vision API to the project.
   [4/26/2018 5:15:32.084 PM] Creating new ComputerVision...
   [4/26/2018 5:15:32.153 PM] Creating new Resource Group...
   [4/26/2018 5:15:40.286 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' version 1.0.2-preview.
   [4/26/2018 5:15:44.117 PM] Retrieving keys...
   [4/26/2018 5:15:45.602 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceKey=<service key>
   [4/26/2018 5:15:45.606 PM] Changing appsettings.json setting: ComputerVisionAPI_ServiceEndPoint=https://australiaeast.api.cognitive.microsoft.com/vision/v2.0
   [4/26/2018 5:15:45.609 PM] Changing appsettings.json setting: ComputerVisionAPI_Name=WebApplication-Core-ComputerVision_ComputerVisionAPI
   [4/26/2018 5:15:46.747 PM] Successfully added Computer Vision API to the project.
   ```
 
## <a name="use-the-computer-vision-api-to-detect-attributes-of-an-image"></a>Utilizar a API de imagem digitalizada para detetar os atributos de uma imagem

1. Adicione as seguintes instruções "using" no Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Adicione um campo de configuração e adicione um construtor que inicializa o campo de configuração no `Startup` classe para ativar a configuração em seu programa.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Na pasta wwwroot no seu projeto, adicione uma pasta de imagens e adicione um ficheiro de imagem à sua pasta wwwroot. Por exemplo, pode utilizar uma das imagens sobre isso [página de API de imagem digitalizada](https://azure.microsoft.com/services/cognitive-services/computer-vision/). Com o botão direito em uma das imagens, guardar para o disco rígido local, em seguida, no Solution Explorer, clique com botão direito na pasta de imagens e choosee **Add** > **Item existente** para adicioná-lo ao seu projeto. Seu projeto deve ter um aspeto semelhante ao seguinte no Explorador de soluções: 
  
   ![pasta de imagens com o arquivo de imagem](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-3.PNG) 

1. Com o botão direito no arquivo de imagem, escolha Propriedades e, em seguida, escolha **copiar se for mais recente**. 

   ![Copiar se for mais recente](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-5.PNG) 
 
1. Substitua o método de configurar o seguinte código para acessar a API de imagem digitalizada e testar uma imagem.

   ```csharp
        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // TODO: Change this to your image's path on your site. 
            string imagePath = @"images/subway.png";

            // Enable static files such as image files. 
            app.UseStaticFiles();

            string visionApiKey = this.configuration["ComputerVisionAPI_ServiceKey"];
            string visionApiEndPoint = this.configuration["ComputerVisionAPI_ServiceEndPoint"];

            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", visionApiKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "visualFeatures=Categories,Description,Color&language=en";

            // Assemble the URI for the REST API Call.
            string uri = visionApiEndPoint + "/analyze" + "?" + requestParameters;

            HttpResponseMessage response;

            // Request body. Posts an image you've added to your site's images folder. 
            var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
            byte[] byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

            string contentString = string.Empty;
            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                response = client.PostAsync(uri, content).Result;

                // Get the JSON response.
                contentString = response.Content.ReadAsStringAsync().Result;
            }

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync("<h1>Cognitive Services Demo</h1>");
                await context.Response.WriteAsync($"<p><b>Test Image:</b></p>");
                await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
                await context.Response.WriteAsync($"<p><b>Computer Vision API results:</b></p>");
                await context.Response.WriteAsync("<p>");
                await context.Response.WriteAsync(JsonPrettyPrint(contentString));
                await context.Response.WriteAsync("<p>");
            });
        }

   ```
    O código aqui constrói um pedido HTTP com o URI e a imagem como conteúdo binário para uma chamada à API de REST de visão do computador.

1. Adicione as funções de programa auxiliar GetImageAsByteArray e JsonPrettyPrint.

   ```csharp
        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            string INDENT_STRING = "    ";
            var indent = 0;
            var quoted = false;
            var sb = new StringBuilder();
            for (var i = 0; i < json.Length; i++)
            {
                var ch = json[i];
                switch (ch)
                {
                    case '{':
                    case '[':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case '}':
                    case ']':
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        sb.Append(ch);
                        break;
                    case '"':
                        sb.Append(ch);
                        bool escaped = false;
                        var index = i;
                        while (index > 0 && json[--index] == '\\')
                            escaped = !escaped;
                        if (!escaped)
                            quoted = !quoted;
                        break;
                    case ',':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case ':':
                        sb.Append(ch);
                        if (!quoted)
                            sb.Append(" ");
                        break;
                    default:
                        sb.Append(ch);
                        break;
                }
            }
            return sb.ToString();
        }
   ```

1. Executar o aplicativo web e ver o que API de imagem digitalizada encontrado em sua imagem.

   ![Imagem do computador de API de imagem digitalizada e resultados formatados](media/vs-computer-vision-connected-service/Cog-Vision-Connected-Service-4.PNG)  

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos. Esta ação elimina o serviço cognitivo e recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a API de imagem digitalizada lendo o [documentação de API de visão do computador](Home.md).
