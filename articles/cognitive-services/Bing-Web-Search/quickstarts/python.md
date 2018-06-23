---
title: Chamada e resposta - guia de introdução do Python para serviços do Azure cognitivos, API de pesquisa do Bing Web | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de pesquisa do Bing Web no Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 9/18/2017
ms.author: v-jerkin
ms.openlocfilehash: 8d4df9db60c7a74a5b9e53d4622528c0054b4f19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352436"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-python"></a>Chamada e resposta: a primeira consulta de pesquisa na Web Bing no Python

A API de pesquisa do Bing Web fornece uma experiência semelhante ao Bing.com/Search devolvendo resultados de pesquisa que o Bing determina são relevantes para a consulta do utilizador. Os resultados podem incluir páginas Web, imagens, vídeos, notícias e entidades, juntamente com consultas de pesquisa relacionados, as correções ortografia, fusos horários, a conversão de unidade, traduções e cálculos. Os tipos de resultados, obter baseiam-se no respetiva relevância e a camada de APIs de pesquisa do Bing que subscrever.

Consulte o [referência da API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) para detalhes técnicos sobre as APIs.

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: 

[![Gestor de enlaces](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)


## <a name="prerequisites"></a>Pré-requisitos
Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="running-the-walkthrough"></a>Executar as instruções

Definir `subscription_key` a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o `search_url` ponto final está correta. Desta redação, apenas um ponto final é utilizado para a pesquisa do Bing APIs. Se ocorrerem erros de autorização, verificar este valor contra o ponto final da pesquisa Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

Definir `search_term` para consulta Bing cognitivos nos serviços da Microsoft.


```python
search_term = "Microsoft Cognitive Services"
```

O seguinte bloquear utiliza o `requests` biblioteca do Python para a chamada para a pesquisa do Bing APIs e devolvem os resultados como um objeto JSON. Observe que podemos passa a chave de API através de `headers` dicionário e a pesquisa termo através o `params` dicionário. Para obter uma lista completa das opções que podem ser utilizadas para filtrar os resultados da pesquisa, consulte o [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) documentação.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O `search_results` objeto contém os resultados da pesquisa, juntamente com metadados avançado, como as páginas e consultas relacionadas. As seguintes linhas de código formatar as páginas principais devolvidas pela consulta.


```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa do Bing Web](../tutorial-bing-web-search-single-page-app.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de pesquisa na Web Bing](../overview.md)  
[Experimente](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[referência da API de pesquisa do Bing Web](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
