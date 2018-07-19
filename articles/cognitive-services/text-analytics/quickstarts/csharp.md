---
title: Início rápido em C#, para os serviços cognitivos do Azure, API de análise de texto | Documentos da Microsoft
description: Exemplos de código e informações de GET para ajudá-lo a rapidamente começar a utilizar a API de análise de texto nos serviços cognitivos da Microsoft no Azure.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/20/2017
ms.author: ashmaka
ms.openlocfilehash: 94847adf761652a25fd3e2d594c7169776fefc89
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125130"
---
# <a name="quickstart-for-text-analytics-api-with-c"></a>Início rápido para API com o C # de análise de texto 
<a name="HOLTop"></a>

Este artigo mostra-lhe como detetar idioma, analisar sentimentos e extrair expressões-chave a utilizar o [APIs de análise de texto](//go.microsoft.com/fwlink/?LinkID=759711) com c#. O código foi escrito para trabalhar num .net, aplicativo principal, com um mínimo referências a bibliotecas externas, portanto, também pode executá-lo no Linux ou MacOS.

Consulte a [as definições da API](//go.microsoft.com/fwlink/?LinkID=759346) para documentação técnica para as APIs.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de análise de texto**. Pode utilizar o **escalão gratuito para 5000 transações/mês** para concluir este início rápido.

Também tem de ter o [chave de acesso e de ponto final](../How-tos/text-analytics-how-to-access-key.md) que foi gerado para durante o início de sessão cópia de segurança. 


## <a name="install-the-nuget-sdk-package"></a>Instalar o pacote de Nuget SDK
1. Crie uma nova solução de consola no Visual Studio.
1. Clique com o botão direito do rato na solução e clique em **gerir pacotes NuGet para solução**
1. Mark a **Include Prerelease** caixa de verificação.
1. Selecione o **navegue** separador e procure **Microsoft.Azure.CognitiveServices.Language**
1. Selecione o pacote Nuget e instalá-lo.

> [!Tip]
>  Enquanto poderia chamar o [pontos de extremidade HTTP](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) diretamente da linguagem c#, o SDK de Microsoft.Azure.CognitiveServices.Language torna muito mais fácil chamar o serviço sem ter de se preocupar sobre serializando e desserializando o JSON.
>
> Alguns links úteis:
> - [Página de SDK Nuget](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [Código do SDK ](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-using-the-sdk"></a>Chamar a API de análise de texto com o SDK
1. Substitua Program.cs com o código fornecido abaixo. Este programa demonstra as capacidades de API de análise de texto em 3 secções (extração de idioma, extração de frases-chave e análise de sentimentos).
1. Substitua o `Ocp-Apim-Subscription-Key` valor de cabeçalho com uma chave de acesso válido para a sua subscrição.
1. Substituir a localização na `client.BaseUri` para o ponto final que inscreveu. Pode encontrar o ponto final no recurso do Portal do Azure. O ponto de extremidade normalmente é semelhante a "https://[region].api.cognitive.microsoft.com/text/analytics/v2.0".
1. Execute o programa.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
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
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials());
            client.BaseUri = new Uri("https://westus.api.cognitive.microsoft.com/text/analytics/v2.0");

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language
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

            // Getting key-phrases
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing keyphrases
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }

            // Extracting sentiment
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


            // Printing sentiment results
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)

