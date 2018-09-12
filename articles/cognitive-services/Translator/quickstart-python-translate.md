---
title: Tradução de Texto traduzir texto com o Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, pode traduzir texto de um idioma para o outro através da API de Texto do Microsoft Translator com Python nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 8f70ffb77e21131990d6b77a1cb13c9d5c054d06
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "43770860"
---
# <a name="quickstart-translate-text-with-python"></a>Início rápido: Traduzir texto com Python

Neste início rápido, pode traduzir texto de um idioma para outro através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Necessita do [Python 3.x](https://www.python.org/downloads/) para executar este código.

Para utilizar a API de Texto do Microsoft Translator, também necessita de uma chave de subscrição, consulte [Como iniciar sessão na API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Pedido de tradução

O código seguinte traduz o texto de origem de um idioma para o outro através do método [Traduzir](./reference/v3-0-translate.md).

1. Crie um novo projeto do Python no seu editor de código preferido.
2. Adicione o código indicado abaixo.
3. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, uuid, json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsofttranslator.com'
path = '/translate?api-version=3.0'

# Translate to German and Italian.
params = "&to=de&to=it";

text = 'Hello, world!'

def translate (content):

    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path + params, content, headers)
    response = conn.getresponse ()
    return response.read ()

requestBody = [{
    'Text' : text,
}]
content = json.dumps(requestBody, ensure_ascii=False).encode('utf-8')
result = translate (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
```

## <a name="translate-response"></a>Resposta da tradução

O JSON devolve uma resposta com êxito, tal como é apresentado no exemplo seguinte:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo para este início rápido e outros, incluindo a transliteração e a identificação de idioma, assim como os projetos exemplo da Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do Python no GitHub](https://aka.ms/TranslatorGitHub?type=&language=python)
