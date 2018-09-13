---
title: Utilizar a Tradução de Texto para calcular o comprimento de frases com o Ruby | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, pode calcular o comprimento de frases no texto através da API de Texto do Microsoft Translator com Ruby nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: nolachar
ms.openlocfilehash: 3a4fc999961e06b084a0d7da42ed5576962e5722
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "43771043"
---
# <a name="quickstart-get-sentence-lengths-with-ruby"></a>Início rápido: obter o comprimento de frases com Ruby

Neste guia de introdução, encontrará o comprimento de frases no texto através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) ou posterior para executar este código.

Para utilizar a API de Texto do Microsoft Translator também precisa de uma chave de subscrição; veja [How to sign up for the Translator Text API (Como inscrever-se na API de Texto do Microsoft Translator)](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>Pedido de BreakSentence

O seguinte código separa o texto de origem em frases através do método [BreakSentence](./reference/v3-0-break-sentence.md) (Separar Frase).

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
path = '/breaksentence?api-version=3.0'

uri = URI (host + path)

text = 'How are you? I am fine. What did you do today?'

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

## <a name="breaksentence-response"></a>Resposta do BreakSentence

É devolvida uma resposta com êxito em JSON, tal como apresentado no exemplo seguinte:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros exemplos de projetos de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos de Ruby no GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
