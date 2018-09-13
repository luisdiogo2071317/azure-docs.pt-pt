---
title: Utilizar a Tradução de Texto para identificar o idioma do texto com Ruby | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai identificar o idioma do texto de origem com a API de Texto do Microsoft Translator com Ruby nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: b692b66454cc86e6d81aec9c3139b39a905d0d66
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "43771057"
---
# <a name="quickstart-identify-language-from-text-with-ruby"></a>Início Rápido: identificar o idioma do texto com Ruby

Neste início rápido, vai identificar o idioma do texto de origem com a API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) ou posterior para executar este código.

Para utilizar a API de Texto do Microsoft Translator também precisa de uma chave de subscrição; veja [How to sign up for the Translator Text API (Como inscrever-se na API de Texto do Microsoft Translator)](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Pedido de deteção

O código seguinte identifica o idioma do texto de origem com o método [Detetar](./reference/v3-0-detect.md).

1. Crie um novo projeto do Ruby no seu editor de código preferido.
2. Adicione o código indicado abaixo.
3. Substitua o valor `key` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/detect?api-version=3.0'

uri = URI (host + path)

text = 'Salve, mondo!'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="detect-response"></a>Resposta da deteção

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

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

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros exemplos de projetos de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos de Ruby no GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
