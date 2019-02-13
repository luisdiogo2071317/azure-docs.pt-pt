---
title: 'Início rápido: Efetua uma pesquisa com Python - API de pesquisa Web Bing'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para enviar pedidos para a API de pesquisa Web do Bing REST com o Python e receber uma resposta JSON
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 02/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: e191821c8e2c6e189e8d7f8befcb11009f2ed7d8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172370"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Início rápido: Utilizar o Python para chamar a API de pesquisa Web Bing  

Utilize este guia de introdução para efetuar a primeira chamada à API de pesquisa Web do Bing e receber a resposta JSON. Esta aplicação de Python envia um pedido de pesquisa para a API e apresenta a resposta. Embora esta aplicação esteja escrita em Python, a API é um serviço Web RESTful compatível com a maioria das linguagens de programação.

Este exemplo é executado como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Clique no destaque do Binder de arranque:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

* [Python 2.x ou 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Definir variáveis

Substitua o valor `subscription_key` por uma chave de subscrição válida da sua conta do Azure.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Declarar o ponto final da API de Pesquisa na Web do Bing. Caso se depare com quaisquer erros de autorização, verifique novamente este valor relativamente ao ponto final da pesquisa do Bing no dashboard do Azure.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Esteja à vontade para personalizar a consulta de pesquisa, ao substituir o valor por `search_term`.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Fazer um pedido

Este bloco utiliza a biblioteca `requests` para chamar a API de Pesquisa na Web do Bing e devolver os resultados como um objeto JSON. A chave de API é passada no dicionário `headers` e os parâmetros de consulta e o termo de pesquisa são passados no dicionário `params`. Veja a documentação [API de Pesquisa na Web do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) para obter uma lista completa das opções e parâmetros.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formatar e apresentar a resposta

O objeto `search_results` inclui os resultados da pesquisa e metadados, como as consultas e páginas relacionadas. Este código utiliza a biblioteca `IPython.display` para formatar e apresentar a resposta no seu browser.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Código de exemplo no GitHub

Se deseja executar este código localmente, o [exemplo completo está disponível no GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa Web do Bing](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
