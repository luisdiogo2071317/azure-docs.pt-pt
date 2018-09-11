---
title: 'Início rápido: Utilizar c# para chamar a API de análise de texto | Documentos da Microsoft'
titleSuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar rapidamente a utilizar a API de análise de texto nos serviços cognitivos da Microsoft no Azure.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 08/30/2018
ms.author: ashmaka
ms.openlocfilehash: b4d945b7495897caf1f4edd1e909581614798a23
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303026"
---
# <a name="quickstart-using-c-to-call-the-text-analytics-cognitive-service"></a>Início rápido: Utilizar c# para chamar o serviço cognitivos de análise de texto
<a name="HOLTop"></a>

Este artigo mostra-lhe como detetar idioma, analisar sentimentos e extrair expressões-chave, utilizando o [APIs de análise de texto](//go.microsoft.com/fwlink/?LinkID=759711) com c#. O código foi escrito para trabalhar num aplicativo do .NET Core, com um mínimo referências a bibliotecas externas, portanto, também pode executá-lo no Linux ou MacOS.

Consulte a [as definições da API](//go.microsoft.com/fwlink/?LinkID=759346) para documentação técnica para as APIs.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com a API de análise de texto. Pode utilizar o *escalão gratuito para 5000 transações/mês* para concluir este início rápido.

Também tem de ter o [chave de acesso e de ponto final](../How-tos/text-analytics-how-to-access-key.md) que foram gerados para durante a inscrição. 


## <a name="install-the-nuget-sdk-package"></a>Instalar o pacote NuGet SDK
1. Crie uma nova solução de consola no Visual Studio.
1. A solução com o botão direito e selecione **gerir pacotes NuGet para solução**.
1. Selecione o **Include Prerelease** caixa de verificação.
1. Selecione o **navegue** separador e procure **Microsoft.Azure.CognitiveServices.Language**.
1. Selecione o **Microsoft.Azure.CognitiveServices.Language.TextAnalytics** NuGet empacotar e instalá-lo.

> [!Tip]
> Embora pode chamar o [pontos de extremidade HTTP](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) diretamente da linguagem c#, o SDK de Microsoft.Azure.CognitiveServices.Language torna muito mais fácil chamar o serviço sem ter de se preocupar sobre serializando e desserializando o JSON.
>
> Seguem-se links úteis:
> - [Página de SDK NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [Código do SDK](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-by-using-the-sdk"></a>Chamar a API de análise de texto com o SDK
1. Substitua Program.cs pelo código seguinte. Este programa demonstra as capacidades de API de análise de texto em três seções (extração de idioma, extração de frases-chave e análise de sentimentos).
1. Substitua o `Ocp-Apim-Subscription-Key` valor de cabeçalho com uma chave de acesso que é válido para a sua subscrição.
1. Substituir a localização na `Endpoint` ao ponto final do qual inscreveu. Pode encontrar o ponto final sobre o recurso de portal do Azure. O ponto de extremidade normalmente começa com "https://[region].api.cognitive.microsoft.com." Inclua apenas o nome de protocolo e o anfitrião.
1. Execute o programa.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Linq;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", "ENTER KEY HERE");
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            };

            Console.OutputEncoding = System.Text.Encoding.UTF8;
```

## <a name="detect-language"></a>Detetar idioma

A API de deteção de idioma Deteta o idioma de um texto de documento, utilizando o [método detetar idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

```csharp
            // Extracting language.
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result =  client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine("Document ID: {0} , Language: {1}", document.Id, document.DetectedLanguages[0].Name);
            }
```

## <a name="extract-key-phrases"></a>Extrair expressões-chave

A API de extração de frase chave extrai frases-chave de uma mensagem de texto de documento, utilizando o [método de expressões-chave](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6).

```csharp
            // Getting key phrases.
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing key phrases.
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }
```

## <a name="analyze-sentiment"></a>Analisar sentimento

A API de análise de sentimentos Deteta o sentimento de um conjunto de registos de texto, a utilizar o [método de sentimento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9).

```csharp
            // Analyzing sentiment.
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results.
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
```

## <a name="identify-linked-entities"></a>Identificar as entidades associadas

A API do Entity Linking identifica as entidades conhecidas num texto de documento, utilizando o [método de ligação de entidades](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634).

```csharp
            // Linking entities
            Console.WriteLine("\n\n===== ENTITY LINKING ======");

            EntitiesBatchResult result4 = client.EntitiesAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                            new MultiLanguageInput("en", "0", "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."),
                            new MultiLanguageInput("en", "1", "The Seattle Seahawks won the Super Bowl in 2014."),
                        })).Result;

            // Printing entity results.
            foreach (var document in result4.Documents)
            {
                Console.WriteLine("Document ID: {0} , Entities: {1}", document.Id, String.Join(", ", document.Entities.Select(entity => entity.Name)));
            }
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de Texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas Mais Frequentes (FAQ)](../text-analytics-resource-faq.md)
