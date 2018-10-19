---
title: 'Início Rápido: API de Pesquisa de Entidades do Bing, Python'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Pesquisa de Entidades do Bing.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 11/28/2017
ms.author: v-jaswel
ms.openlocfilehash: 80265c883214812acbd0af3bd186a0fbd3703132
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814343"
---
# <a name="quickstart-for-bing-entity-search-api-with-python"></a>Início Rápido da API de Pesquisa de Entidades do Bing com o Python

Este artigo mostra-lhe como utilizar a API de [Pesquisa de Entidades do Bing](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web) com o Python.

## <a name="prerequisites"></a>Pré-requisitos

Vai precisar do [Python 3.x](https://www.python.org/downloads/) para executar este código.

Tem de ter uma [conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com a **API de Pesquisa de Entidades do Bing**. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-entity-search-api) é suficiente para este guia de início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="search-entities"></a>Pesquisar entidades

Para executar esta aplicação, siga os passos abaixo.

1. Crie um novo projeto de Python com o seu IDE favorito.
2. Adicione o código indicado abaixo.
3. Substitua o valor `key` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse
import json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/entities'

mkt = 'en-US'
query = 'italian restaurants near me'

params = '?mkt=' + mkt + '&q=' + urllib.parse.quote (query)

def get_suggestions ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_suggestions ()
print (json.dumps(json.loads(result), indent=4))
```

**Resposta**

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "http://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

[Voltar ao início](#HOLTop)

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Tutorial da Pesquisa de Entidades do Bing](../tutorial-bing-entities-search-single-page-app.md)
> [Descrição geral da Pesquisa de Entidades do Bing](../search-the-web.md )
> [Referência da API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
