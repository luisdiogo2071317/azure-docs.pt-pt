---
title: 'Exemplo: Criar um skill personalizado no pipeline de pesquisa cognitivos (Azure Search) | Microsoft Docs'
description: Demonstra a utilizar a API de traduzir texto no skill personalizado mapeado para uma pesquisa cognitivos indexação pipeline na Azure Search.
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a295bf741862bb58a86234b5c85f48d7a1b52be7
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Exemplo: Criar um skill personalizado utilizando a API de traduzir texto

Neste exemplo, saiba como criar uma web skill personalizada de API que aceita texto em qualquer idioma e converte-o para inglês. O exemplo utiliza um [função do Azure](https://azure.microsoft.com/services/functions/) moldar o [traduzir API de texto](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) para que implementa a interface skill personalizado.

## <a name="prerequisites"></a>Pré-requisitos

+ Leia sobre [interface skill personalizado](cognitive-search-custom-skill-interface.md) artigo se não estiver familiarizado com a interface de entrada/saída deve implementar um skill personalizado.

+ [Inscrever-se para a API de texto tradutor](../cognitive-services/translator/translator-text-how-to-signup.md)e obter uma chave de API para consumi-lo.

+ Instalar [Visual Studio 2017 versão 15.5](https://www.visualstudio.com/vs/) ou posterior, incluindo a carga de trabalho de desenvolvimento do Azure.

## <a name="create-an-azure-function"></a>Criar uma Função do Azure

Embora este exemplo utiliza uma função do Azure para alojar uma API web, não é necessária.  Desde que cumpra o [interface requisitos para um skill cognitivos](cognitive-search-custom-skill-interface.md), a abordagem que adotar é immaterial. As funções do Azure, no entanto, tornam mais fácil criar um skill personalizado.

### <a name="create-a-function-app"></a>Criar uma aplicação de função

1. No Visual Studio, selecione **novo** > **projeto** no menu de ficheiro.

1. Na caixa de diálogo novo projeto, selecione **instalada**, expanda **Visual c#** > **nuvem**, selecione **das funções do Azure**, escreva um Atribua um nome para o seu projeto e selecione **OK**. O nome da aplicação de funções deve ser válido como um espaço de nomes C#. Portanto, não use sublinhados, hífenes ou outros carateres não alfanuméricos.

1. Selecione o tipo a ser **acionador de HTTP**

1. Conta de armazenamento, pode selecionar **nenhum**, uma vez que não terá qualquer armazenamento para esta função.

1. Selecione **OK** para criar a função de projeto e HTTP acionada função.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>Modificar o código para chamar o serviço cognitivos traduzir

O Visual Studio cria um projeto e neste uma classe que contém o código automático de tipo de função de escolhida. O atributo *FunctionName* nos conjuntos define o nome da função. O atributo *HttpTrigger* especifica que a função é acionada por um pedido de HTTP.

Agora, substitua todo o conteúdo do ficheiro *Function1.cs* com o seguinte código:

```csharp
using System.IO;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Text;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
        #region classes used to serialize the response
        private class WebApiResponseError
        {
            public string message { get; set; }
        }

        private class WebApiResponseWarning
        {
            public string message { get; set; }
        }

        private class WebApiResponseRecord
        {
            public string recordId { get; set; }
            public Dictionary<string, object> data { get; set; }
            public List<WebApiResponseError> errors { get; set; }
            public List<WebApiResponseWarning> warnings { get; set; }
        }

        private class WebApiEnricherResponse
        {
            public List<WebApiResponseRecord> values { get; set; }
        }
        #endregion


        /// <summary>
        /// Note that this function can translate up to 1000 characters. If you expect to need to translate more characters, use 
        /// the paginator skill before calling this custom enricher
        /// </summary>
        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req, 
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string originalLanguage = null;
            string translatedText = null;

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Validation
            if (data?.values == null)
            {
                return new BadRequestObjectResult(" Could not find values array");
            }
            if (data?.values.HasValues == false || data?.values.First.HasValues == false)
            {
                // It could not find a record, then return empty values array.
                return new BadRequestObjectResult(" Could not find valid records in values array");
            }

            recordId = data?.values?.First?.recordId?.Value as string;
            originalText = data?.values?.First?.data?.text?.Value as string;
            originalLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            // Only translate records that actually need to be translated. 
            if (!originalLanguage.Contains("en"))
            {
                translatedText = TranslateText(originalText, "en-us").Result;
            }
            else
            {
                // text is already in English.
                translatedText = originalText;
            }

            // Put together response.
            WebApiResponseRecord responseRecord = new WebApiResponseRecord();
            responseRecord.data = new Dictionary<string, object>();
            responseRecord.recordId = recordId;
            responseRecord.data.Add("text", translatedText);

            WebApiEnricherResponse response = new WebApiEnricherResponse();
            response.values = new List<WebApiResponseRecord>();
            response.values.Add(responseRecord);

            return (ActionResult)new OkObjectResult(response); 
        }

        /// <summary>
        /// Use Cognitive Service to translate text from one language to antoher.
        /// </summary>
        /// <param name="myText">The text to translate</param>
        /// <param name="destinationLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string myText, string destinationLanguage)
        {
            string host = "https://api.microsofttranslator.com";
            string path = "/V2/Http.svc/Translate";

            // NOTE: Replace this example key with a valid subscription key.
            string key = "064d8095730d4a99b49f4bcf16ac67f8";

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            List<KeyValuePair<string, string>> list = new List<KeyValuePair<string, string>>() {
                new KeyValuePair<string, string>(myText, "en-us")
            };

            StringBuilder totalResult = new StringBuilder();

            foreach (KeyValuePair<string, string> i in list)
            {
                string uri = host + path + "?to=" + i.Value + "&text=" + System.Net.WebUtility.UrlEncode(i.Key);

                HttpResponseMessage response = await client.GetAsync(uri);

                string result = await response.Content.ReadAsStringAsync();

                // Parse the response XML
                System.Xml.XmlDocument xmlResponse = new System.Xml.XmlDocument();
                xmlResponse.LoadXml(result);
                totalResult.Append(xmlResponse.InnerText); 
            }

            return totalResult.ToString();
        }
    }
}
```

Certifique-se de que introduz as suas próprias *chave* valor o *TranslateText* método com base na chave recebeu quando inscrever-se para a API para traduzir texto.

Neste exemplo é um enricher simple que só funciona num registo a cada vez. Este facto torna-se importante mais tarde, quando definir o tamanho do lote para o skillset.

## <a name="test-the-function-from-visual-studio"></a>Testar a função do Visual Studio

Prima **F5** para executar os comportamentos de função do programa e teste. Utilize o Postman ou Fiddler para emitir uma chamada, como o mostrado abaixo:

```http
POST https://localhost:7071/api/Translate
```
### <a name="request-body"></a>Corpo do pedido
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "es"
            }
        }
   ]
}
```
### <a name="response"></a>Resposta
Deverá ver uma resposta semelhante ao seguinte exemplo:

```json
{
    "values": [
        {
            "recordId": "a1",
            "data": {
                "text": "This is a contract in English"
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>Publicar a função do Azure

Quando estiver satisfeito com o comportamento da função, pode publicá-lo.

1. No **Explorador de Soluções**, clique com o botão direito do rato no projeto e selecione **Publicar**. Escolha **criar novos** > **publicar**.

1. Se já não tiver ligado Visual Studio à sua conta do Azure, selecione **adicionar uma conta...**

1. Siga os avisos no ecrã. É-lhe pedido para especificar a conta do Azure, o grupo de recursos, o plano de alojamento e a conta de armazenamento que pretende utilizar. Pode criar um novo grupo de recursos, um novo plano de alojamento e uma conta de armazenamento se ainda não tiver estes. Quando terminar, selecione **criar**

1. Depois de concluída a implementação, anote o URL do Site. É o endereço da sua aplicação de função no Azure. 

1. No [portal do Azure](https://portal.azure.com), navegue para o grupo de recursos e procure a função traduzir publicou. Sob o **gerir** secção, deverá ver as chaves de anfitrião. Selecione o **cópia** ícone para a *predefinido* chave do anfitrião.  


## <a name="test-the-function-in-azure"></a>Testar a função no Azure

Agora que tem a chave de anfitrião predefinido, testar a função da seguinte forma:

```http
POST https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]
```
### <a name="request-body"></a>Corpo do Pedido
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "es"
            }
        }
   ]
}
```

Isto deve produzir o resultado semelhante ao que vimos anteriormente ao executar a função no ambiente local.

## <a name="connect-to-your-pipeline"></a>Ligar para o pipeline
Agora que tem um skill personalizada novo, pode adicioná-lo à sua skillset. O exemplo abaixo mostra como chamar o skill. Uma vez que o skill não processa lotes, adicione uma instrução para o tamanho máximo do lote ser apenas ```1``` enviar documentos um de cada vez.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "http://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/languageCode"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "translatedText"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Próximos Passos
Parabéns! Criou o seu primeiro enricher personalizado. Agora, pode seguir o mesmo padrão para adicionar a sua própria funcionalidade personalizada. 

+ [Adicionar um skill personalizado para um pipeline de pesquisa cognitivos](cognitive-search-custom-skill-interface.md)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Criar Skillset (REST)](ref-create-skillset.md)
+ [Como mapear os campos avançados](cognitive-search-output-field-mapping.md)