---
title: Tradução de Texto localizar traduções alternativas com C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, pode localizar traduções alternativas e exemplos de termos em contexto através da API de Texto do Microsoft Translator com C# nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: 3f45e7281456f9ae09912a2ee665cb480dc5052f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "43770642"
---
# <a name="quickstart-find-alternate-translations-and-usage-with-c35"></a>Início rápido: Localizar traduções e utilizações alternativas com C&#35;

Neste início rápido, encontra detalhes de possíveis traduções alternativas para um termo, assim como exemplos de utilizações dessas traduções alternativas com a API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Para utilizar a API de Texto do Microsoft Translator, também necessita de uma chave de subscrição, consulte [Como iniciar sessão na API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="dictionary-lookup-request"></a>Pedido de Pesquisa no Dicionário

Apresentamos uma situação onde obtém traduções alternativas para uma palavra através do método de [Pesquisa no Dicionário](./reference/v3-0-dictionary-lookup.md).

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
        static string path = "/dictionary/lookup?api-version=3.0";
        // Translate from English to French.
        static string params_ = "&from=en&to=fr";

        static string uri = host + path + params_;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "great";

        async static void Lookup()
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
            Lookup();
            Console.ReadLine();
        }
    }
}
```

## <a name="dictionary-lookup-response"></a>Resposta de Pesquisa no Dicionário

O JSON devolve uma resposta de êxito, tal apresentado no exemplo seguinte:

```json
[
  {
    "normalizedSource": "great",
    "displaySource": "great",
    "translations": [
      {
        "normalizedTarget": "grand",
        "displayTarget": "grand",
        "posTag": "ADJ",
        "confidence": 0.2783,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 34358
          },
          {
            "normalizedText": "big",
            "displayText": "big",
            "numExamples": 15,
            "frequencyCount": 21770
          },
...
        ]
      },
      {
        "normalizedTarget": "super",
        "displayTarget": "super",
        "posTag": "ADJ",
        "confidence": 0.1514,
        "prefixWord": "",
        "backTranslations": [
          {
            "normalizedText": "super",
            "displayText": "super",
            "numExamples": 15,
            "frequencyCount": 12023
          },
          {
            "normalizedText": "great",
            "displayText": "great",
            "numExamples": 15,
            "frequencyCount": 10931
          },
...
        ]
      },
...
    ]
  }
]
```

## <a name="dictionary-examples-request"></a>Pedido de Exemplos do Dicionário

Apresentamos uma situação onde obtém exemplos contextuais de como utilizar um termo do dicionário através do método [Exemplos do Dicionário](./reference/v3-0-dictionary-examples.md).

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
        static string path = "/dictionary/examples?api-version=3.0";
        // Translate from English to French.
        static string params_ = "&from=en&to=fr";

        static string uri = host + path + params_;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "great";
        static string translation = "formidable";

        async static void Examples()
        {
            System.Object[] body = new System.Object[] { new { Text = text, Translation = translation } };
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
            Examples();
            Console.ReadLine();
        }
    }
}
```

## <a name="dictionary-examples-response"></a>Resposta de Exemplos do Dicionário

O JSON devolve uma resposta de êxito, tal apresentado no exemplo seguinte:

```json
[
  {
    "normalizedSource": "great",
    "normalizedTarget": "formidable",
    "examples": [
      {
        "sourcePrefix": "You have a ",
        "sourceTerm": "great",
        "sourceSuffix": " expression there.",
        "targetPrefix": "Vous avez une expression ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
      {
        "sourcePrefix": "You played a ",
        "sourceTerm": "great",
        "sourceSuffix": " game today.",
        "targetPrefix": "Vous avez été ",
        "targetTerm": "formidable",
        "targetSuffix": "."
      },
...
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros projetos exemplo de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do C# no GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
