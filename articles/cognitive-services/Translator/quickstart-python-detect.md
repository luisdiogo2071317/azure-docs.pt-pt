---
title: Tradução de Texto identificar o idioma do texto com Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai identificar o idioma do texto de origem com a API de Texto do Microsoft Translator com Python nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 07a16e419bfdd4d73108fcdaa12695e99fecabee
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "43770855"
---
# <a name="quickstart-identify-language-from-text-with-python"></a>Início rápido: Identificar o idioma do texto com Python

Neste início rápido, vai identificar o idioma do texto de origem através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Necessita do [Python 3.x](https://www.python.org/downloads/) para executar este código.

Para utilizar a API de Texto do Microsoft Translator, também necessita de uma chave de subscrição, consulte [Como iniciar sessão na API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Detetar o pedido

O código seguinte identifica o idioma do texto de origem através do método [Detetar](./reference/v3-0-detect.md).

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
path = '/detect?api-version=3.0'

params = ''

text = 'Salve, mondo!'

def detect (content):

    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path, content, headers)
    response = conn.getresponse ()
    return response.read ()

requestBody = [{
    'Text' : text,
}]
content = json.dumps(requestBody, ensure_ascii=False).encode('utf-8')
result = detect (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
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
> [Explorar exemplos do Python no GitHub](https://aka.ms/TranslatorGitHub?type=&language=python)
