---
title: Guia de introdução do Python para os serviços do Azure cognitivos, Bing notícias pesquisa API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de pesquisa do Bing notícias nos serviços cognitivos Microsoft no Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 0fde478b650513aa1527c1d47f5b453ba094506c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352352"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Início rápido para o Bing notícias pesquisa API com o Python
Esta explicação passo a passo demonstra um exemplo simples da chamada para a API de pesquisa do Bing notícias e processamento pós-cópia o objeto JSON resultante. Para obter mais informações, consulte [documentação nova pesquisa do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: 

[![Gestor de enlaces](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="running-the-walkthrough"></a>Executar as instruções
Em primeiro lugar, defina `subscription_key` a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o `search_url` ponto final está correta. Desta redação, apenas um ponto final é utilizado para a pesquisa do Bing APIs. Se ocorrerem erros de autorização, verificar este valor contra o ponto final da pesquisa Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Definir `search_term` para procurar artigos de notícias sobre o Microsoft.


```python
search_term = "Microsoft"
```

O seguinte bloquear utiliza o `requests` biblioteca do Python para a chamada para a pesquisa do Bing APIs e devolvem os resultados como um objeto JSON. Observe que podemos passa a chave de API através de `headers` dicionário e a pesquisa termo através o `params` dicionário. Para obter uma lista completa das opções que podem ser utilizadas para filtrar os resultados da pesquisa, consulte o [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) documentação.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O `search_results` objeto contém os artigos novos relevantes, juntamente com metadados avançado. Por exemplo, a seguinte linha de código extrai as descrições dos artigos.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Estas descrições, em seguida, podem ser compostas como uma tabela com a palavra-chave de pesquisa realçada na **negrito**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Notícias de paginação](paging-news.md)
> [utilizando marcadores decoration para realçar texto](hit-highlighting.md)

## <a name="see-also"></a>Consulte também 

 [Pesquisa na web notícias de última hora](search-the-web.md)  
 [Experimente](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
