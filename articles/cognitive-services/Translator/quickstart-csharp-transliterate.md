---
title: Tradução de Texto converter script de texto com C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido pode converter texto num idioma de um script para outro através da API de Texto do Microsoft Translator com C# nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: 66d649c0015be8c6a74e9925af68297334bfdb30
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "43770614"
---
# <a name="quickstart-transliterate-text-with-c35"></a>Início rápido: Transliterar texto com C&#35;

Neste início rápido, pode converter texto num idioma de um script para outro através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Para utilizar a API de Texto do Microsoft Translator, também necessita de uma chave de subscrição, consulte [Como iniciar sessão na API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Pedido de transliteração

Os passos seguintes convertem texto num idioma de um script para outro com o método de [Transliteração](./reference/v3-0-transliterate.md).

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
        static string path = "/transliterate?api-version=3.0";
        // Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
        static string params_ = "&language=ja&fromScript=jpan&toScript=latn";

        static string uri = host + path + params_;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // Transliterate "good afternoon".
        static string text = "こんにちは";

        async static void Transliterate()
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
            Transliterate();
            Console.ReadLine();
        }
    }
}
```

## <a name="transliterate-response"></a>Resposta de transliteração

O JSON devolve uma resposta de êxito, tal apresentado no exemplo seguinte:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo para este início rápido e outros, incluindo a tradução e a identificação do idioma, assim como outros projetos de exemplo de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do C# no GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
