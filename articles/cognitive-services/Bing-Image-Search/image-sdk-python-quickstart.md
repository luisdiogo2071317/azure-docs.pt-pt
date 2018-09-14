---
title: 'Início rápido: Procure imagens usando o SDK de pesquisa de imagens do Bing e Python'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para procurar e localizar imagens na web com o SDK de pesquisa de imagens do Bing e Python.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 4a24f1e4e051b627034f1d4664e94e0f47c43014
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578298"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-python"></a>Início rápido: Pesquisa de imagens com o SDK de pesquisa de imagens do Bing e Python

Utilize este guia de introdução para tornar a sua primeira pesquisa de imagens com o SDK de pesquisa de imagens do Bing, que é um wrapper para a API e contém os mesmos recursos. Esta aplicação de Python simple envia uma consulta de pesquisa de imagem, analisa a resposta JSON e apresenta o URL da imagem do primeiro devolvido.

O código-fonte para este exemplo está disponível [no Github](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image-search-quickstart.py) com tratamento de erros adicionais e anotações.

## <a name="prerequisites"></a>Pré-requisitos 

* [Python 2.7 ou 3.4](https://www.python.org/) e superior.

* O [pesquisa de imagens do Azure SDK](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/) para Python
    * Instalar utilizando `pip install azure-cognitiveservices-search-imagesearch`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Crie um novo script de Python no seu IDE favorito ou editor e as seguintes importações:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Crie variáveis para o termo de pesquisa e a chave de subscrição.

    ```python
    subscription_key = "Enter your key here"
    search_term = "canadian rockies"
    ```

## <a name="create-the-image-search-client"></a>Criar o cliente de pesquisa de imagem

3. Criar uma instância do `CognitiveServicesCredentials`e utilizá-lo para criar uma instância do cliente:

    ```python
    client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```
4. Envie uma consulta de pesquisa para a API de pesquisa de imagens do Bing:
    ```python
    image_results = client.images.search(query=search_term)
    ```
## <a name="process-and-view-the-results"></a>Processar e exibir os resultados

Analisar os resultados de imagem devolvidos na resposta.


Se a resposta contém os resultados da pesquisa, armazenar o primeiro resultado e imprimir os detalhes, como uma miniatura devolveu o URL, o URL original, juntamente com o número total de imagens.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte também 

* [O que é a pesquisa de imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Tente uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso de serviços cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Amostras de Python para o SDK dos serviços cognitivos do Azure](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)  
* [Documentação dos serviços cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)