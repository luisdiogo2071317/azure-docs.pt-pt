---
title: 'Início Rápido: API de Pesquisa de Notícias do Bing, Python'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Pesquisa de Notícias do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.openlocfilehash: 2696401b15ceca86c7e7b7d33e7e7f7887f4b7dc
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164342"
---
# <a name="quickstart-for-bing-news-search-api-with-python"></a>Início Rápido da API de Pesquisa de Notícias do Bing com Python
Estas instruções demonstram um exemplo simples de chamar a API de Pesquisa de Notícias do Bing e o pós-processamento do objeto JSON resultante. Para obter mais informações, veja a [documentação da Pesquisa de Notícias do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference).  

Pode executar este exemplo como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org), ao clicar no destaque de lançamento do Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma [conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de Pesquisa do Bing**. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="running-the-walkthrough"></a>Executar as instruções
Em primeiro lugar, defina `subscription_key` para a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, verifique se que o ponto final `search_url` está correto. Até ao momento, só é utilizado um ponto final para as APIs de Pesquisa do Bing. Caso se depare com erros de autorização, verifique novamente este valor relativamente ao ponto final da pesquisa do Bing no seu dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

Defina `search_term` para procurar artigos de notícias sobre a Microsoft.


```python
search_term = "Microsoft"
```

O seguinte bloco utiliza a biblioteca `requests` em Python para chamar as APIs de Pesquisa do Bing e devolver os resultados como um objeto JSON. Tenha em atenção que passamos a chave de API através do dicionário `headers` e o termo de pesquisa através do dicionário `params`. Para ver a lista completa de opções que podem ser utilizadas para filtrar os resultados da pesquisa, veja a documentação da [API REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O objeto `search_results` contém os artigos novos relevantes, juntamente com metadados detalhados. Por exemplo, a seguinte linha de código extrai as descrições dos artigos.


```python
descriptions = [article["description"] for article in search_results["value"]]
```

Estas descrições, em seguida, podem ser compostas numa tabela com a palavra-chave de pesquisa realçada a **negrito**.


```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc) for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Paginação de notícias](paging-news.md)
> [Utilizar marcadores de decoração para realçar o texto](hit-highlighting.md)

## <a name="see-also"></a>Consulte também 

 [Pesquisar notícias na Web](search-the-web.md)  
 [Experimente](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/)
