---
title: 'Início rápido: Efetua uma pesquisa com Python - API de pesquisa Web Bing'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para enviar pedidos para a API de pesquisa Web do Bing REST com o Python e receber uma resposta JSON
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 4c42461c31e821128c8aa583b5620c1274a9e955
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53260060"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Início rápido: Utilizar o Python para chamar a API de pesquisa Web Bing  
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em **Pesquisa**.  Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Utilize este início rápido para fazer a primeira chamada à API de Pesquisa na Web do Bing e receber uma resposta JSON em menos de 10 minutos.  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Este exemplo é executado como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Clique no destaque do Binder de arranque:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

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
