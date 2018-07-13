---
title: Tutorial de c# do API face | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Crie uma aplicação simple do Windows que utiliza a API Face dos serviços cognitivos para detetar recursos de faces numa imagem.
services: cognitive-services
author: ghogen
manager: douge
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ghogen
ms.openlocfilehash: b51760f889db27aa25e54582070ee7d3adcf66f8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38665242"
---
# <a name="connecting-to-cognitive-services-face-api-by-using-connected-services-in-visual-studio"></a>Ligar a API Face dos serviços cognitivos com os serviços ligados no Visual Studio

Ao utilizar a API Face de serviços cognitivos, pode detetar, analisar, organize e assinale rostos em fotos.

Este artigo e os respetivos artigos complementar fornecem detalhes para utilizar a funcionalidade de serviço ligado do Visual Studio para a API Face dos serviços cognitivos. A capacidade está disponível em ambos os 15.7 de 2017 Visual Studio ou posterior, com a extensão de serviços cognitivos instalada.

## <a name="prerequisites"></a>Pré-requisitos

- **Uma subscrição do Azure**. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versão 15.7** com o **desenvolvimento Web** instalada da carga de trabalho. [Baixe agora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="create-a-project-and-add-support-for-cognitive-services-face-api"></a>Criar um projeto e adicionar suporte para a API Face dos serviços cognitivos

1. Crie um novo projeto de web do ASP.NET Core. Utilize o modelo de projeto vazio. 

1. Na **Explorador de soluções**, escolha **Add** > **serviço ligado**.
   Será exibida a página de serviço ligado com os serviços que pode adicionar ao seu projeto.

   ![Adicionar item de menu do serviço ligado](./media/vs-face-connected-service/Connected-Service-Menu.PNG)

1. No menu de serviços disponíveis, escolha **API Face dos serviços cognitivos**.

   ![Escolher o serviço para ligar a](./media/vs-face-connected-service/Cog-Face-Connected-Service-0.PNG)

   Se já tem sessão iniciada no Visual Studio e uma subscrição do Azure associado à sua conta, é apresentada uma página com uma lista suspensa com as suas subscrições.

   ![Selecione a sua subscrição](media/vs-face-connected-service/Cog-Face-Connected-Service-1.PNG)

1. Selecione a subscrição que pretende utilizar e, em seguida, escolha um nome para a API Face, ou escolha a ligação de edição para modificar o nome gerado automaticamente, selecione o grupo de recursos e o escalão de preço.

   ![Editar detalhes do serviço ligado](media/vs-face-connected-service/Cog-Face-Connected-Service-2.PNG)

   Siga a ligação para obter detalhes sobre os escalões de preço.

1. Escolha adicionar para adicionar suporte para o serviço ligado.
   Visual Studio modifica seu projeto para adicionar os pacotes NuGet, entradas de ficheiro de configuração e outras alterações para dar suporte a uma ligação a API Face.

## <a name="use-the-face-api-to-detect-attributes-of-faces-in-an-image"></a>Utilizar a API de rostos para detetar os atributos de rostos numa imagem

1. Adicione as seguintes instruções "using" no Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Adicione um campo de configuração e adicione um construtor que inicializa o campo de configuração na classe de inicialização para ativar a configuração em seu programa.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Na pasta wwwroot no seu projeto, adicione uma pasta de imagens e adicione um ficheiro de imagem à sua pasta wwwroot. Por exemplo, pode utilizar uma das imagens sobre isso [página de API de rostos](https://azure.microsoft.com/services/cognitive-services/face/). Clique com o botão direito do rato em uma das imagens, guardar para o disco rígido local, em seguida, no Solution Explorer, clique com botão direito na pasta de imagens e choosee **Add** > **Item existente** para adicioná-lo ao seu projeto. Seu projeto deve ter um aspeto semelhante ao seguinte no Explorador de soluções:
 
   ![pasta de imagens com o arquivo de imagem](media/vs-face-connected-service/Cog-Face-Connected-Service-6.PNG)

1. Com o botão direito no arquivo de imagem, escolha Propriedades e, em seguida, escolha **copiar se for mais recente**.

   ![Copiar se for mais recente](media/vs-face-connected-service/Cog-Face-Connected-Service-5.PNG)
 
1. Substitua o método de configurar o seguinte código para acessar a API Face e testar uma imagem. Altere a cadeia de caracteres imagePath para o caminho correto e o nome de ficheiro para a sua imagem de rostos.

   ```csharp
    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // TODO: Change this to your image's path on your site.
            string imagePath = @"images/face1.png";

            // Enable static files such as image files.
            app.UseStaticFiles();

            string faceApiKey = this.configuration["FaceAPI_ServiceKey"];
            string faceApiEndPoint = this.configuration["FaceAPI_ServiceEndPoint"];

            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", faceApiKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

            // Assemble the URI for the REST API Call.
            string uri = faceApiEndPoint + "/detect?" + requestParameters;

            // Request body. Posts an image you've added to your site's images folder.
            var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
            var byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

            string contentStringFace = string.Empty;
            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                var response = client.PostAsync(uri, content).Result;

                // Get the JSON response.
                contentStringFace = response.Content.ReadAsStringAsync().Result;
            }

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync($"<p><b>Face Image:</b></p>");
                await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
                await context.Response.WriteAsync($"<p><b>Face detection API results:</b></p>");
                await context.Response.WriteAsync("<p>");
                await context.Response.WriteAsync(JsonPrettyPrint(contentStringFace));
                await context.Response.WriteAsync("<p>");
            });
        }
   ```
    O código neste passo constrói um pedido HTTP com uma chamada à API de REST do rosto, utilizando a chave adicionado quando adicionou o serviço ligado.

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

1. Executar o aplicativo web e ver o que a API Face encontrados na imagem.
 
   ![Imagem da face API e resultados formatados](media/vs-face-connected-service/Cog-Face-Connected-Service-4.PNG)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos. Esta ação elimina o serviço cognitivo e recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
1. Selecione **Eliminar grupo de recursos**.
1. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a API Face ao ler o [documentação da API Face](Overview.md).
