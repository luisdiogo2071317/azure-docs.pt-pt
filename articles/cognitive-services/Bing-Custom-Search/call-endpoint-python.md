---
title: 'Início rápido: Chamar o ponto final de pesquisa personalizada do Bing com Python | Documentos da Microsoft'
titlesuffix: Azure Cognitive Services
description: Utilizar este início rápido para começar a pedir os resultados da pesquisa da sua instância de pesquisa personalizada do Bing com Python
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: 2399cdfcfbc6be30dc013d52760ec4c355d106e4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868315"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Início rápido: Chamar o ponto final de pesquisa personalizada do Bing com Python

Utilize este guia de introdução para começar a pedir os resultados da pesquisa da sua instância de pesquisa personalizada do Bing. Embora esse aplicativo é escrito em Python, a API de pesquisa personalizada do Bing é um serviço RESTful web compatível com a maioria das linguagens de programação. O código fonte deste exemplo está disponível no [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Pré-requisitos

- Uma instância de pesquisa personalizada do Bing. Consulte [início rápido: Criar a primeira instância de pesquisa personalizada do Bing](quick-start.md) para obter mais informações.
- [Python](https://www.python.org/) 2.x ou 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Criar e inicializar a aplicação

1. Criar um novo ficheiro de Python no seu IDE ou editor favorito e adicione as seguintes declarações de importação. Crie variáveis para a chave de subscrição, ID de configuração personalizada e um termo de pesquisa. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Enviar e receber um pedido de pesquisa 

1. Construir o URL do pedido ao acrescentar o termo de pesquisa para o `q=` parâmetro de consulta e o ID de configuração da sua instância de pesquisa personalizada para `customconfig=`. Separe-os com um `&` caráter. 

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Enviar o pedido à sua instância de pesquisa personalizada do Bing e imprimir os resultados da pesquisa.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma aplicação web de pesquisa personalizada](./tutorials/custom-search-web-page.md)
