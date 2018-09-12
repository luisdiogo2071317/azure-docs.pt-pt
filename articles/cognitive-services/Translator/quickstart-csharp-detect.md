---
title: Tradução de Texto identificar o idioma do texto com C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai identificar o idioma do texto de origem com a API de Tradução de Texto com C# nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: 381478f3125371fbcb5f6466af68f63e4e04bb3f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "43770637"
---
# <a name="quickstart-identify-language-from-text-with-c35"></a>Início rápido: Identificar o idioma do texto com C&#35;

Neste início rápido, vai identificar o idioma do texto de origem através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Para utilizar a API de Texto do Microsoft Translator, também necessita de uma chave de subscrição, consulte [Como iniciar sessão na API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Detetar o pedido

O código seguinte identifica o idioma do texto de origem através do método [Detetar](./reference/v3-0-detect.md).

1. Crie um novo projeto em C# no seu IDE favorito.
2. Adicione o código indicado abaixo.
3. Substitua o valor `key` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/detect?api-version=3.0";

        static string uri = host + path;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "Salve mondo!";

        async static void Detect()
        {
            System.Object[] body = new System.Object[] { new { Text = text } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                var result = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(responseBody), Formatting.Indented);

                Console.OutputEncoding = UnicodeEncoding.UTF8;
                Console.WriteLine(result);
            }
        }

        static void Main(string[] args)
        {
            Detect();
            Console.ReadLine();
        }
    }
}
```

## <a name="detect-response"></a>Detetar a resposta

O JSON devolve uma resposta de êxito, tal apresentado no exemplo seguinte:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros projetos exemplo de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do C# no GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
