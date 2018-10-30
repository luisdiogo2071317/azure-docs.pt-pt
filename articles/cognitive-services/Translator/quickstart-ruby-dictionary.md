---
title: 'Início Rápido: localizar traduções alternativas, Ruby – API de Texto do Microsoft Translator'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, pode localizar traduções alternativas e exemplos de termos em contexto através da API de Texto do Microsoft Translator com Ruby.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: c9e33fd4be80c8bb95248e8be34f9540d9cd69ea
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649623"
---
# <a name="quickstart-find-alternate-translations-with-the-translator-text-rest-api-ruby"></a>Início Rápido: localizar traduções alternativas com a API REST de Texto do Microsoft Translator (Ruby)

Neste guia de introdução, pode encontrar detalhes de possíveis traduções alternativas para um termo, assim como exemplos de utilizações dessas traduções alternativas com a API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) ou posterior para executar este código.

Para utilizar a API de Texto do Microsoft Translator também precisa de uma chave de subscrição; veja [How to sign up for the Translator Text API (Como inscrever-se na API de Texto do Microsoft Translator)](translator-text-how-to-signup.md).

## <a name="dictionary-lookup-request"></a>Pedido de Pesquisa no Dicionário

Apresentamos uma situação onde obtém traduções alternativas para uma palavra através do método de [Pesquisa no Dicionário](./reference/v3-0-dictionary-lookup.md).

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
path = '/dictionary/lookup?api-version=3.0'

params = '&from=en&to=fr'

uri = URI (host + path + params)

text = 'great'

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

## <a name="dictionary-lookup-response"></a>Resposta da Pesquisa no Dicionário

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

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

Apresentamos uma situação onde obtém exemplos contextuais de como utilizar um termo no dicionário através do método [Exemplos do Dicionário](./reference/v3-0-dictionary-examples.md).

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
path = '/dictionary/examples?api-version=3.0'

params = '&from=en&to=fr'

uri = URI (host + path + params)

text = 'great'
translation = 'formidable'

content = '[{"Text" : "' + text + '", "Translation" : "' + translation + '"}]'

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

## <a name="dictionary-examples-response"></a>Resposta dos Exemplos do Dicionário

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

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

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros exemplos de projetos de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos de Ruby no GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
