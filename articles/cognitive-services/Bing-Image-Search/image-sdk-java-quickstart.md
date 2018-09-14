---
title: 'Início rápido: Procure imagens usando o SDK de pesquisa de imagens do Bing e o Java'
description: Utilize este guia de introdução para procurar e localizar imagens na web com o SDK de pesquisa de imagens do Bing e a Java.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 12bd6f9a9a0b43b4571a7e0311ffbea54c7b9054
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574066"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-java"></a>Início rápido: Pesquisa de imagens com o SDK de pesquisa de imagens do Bing e o Java

Utilize este guia de introdução para tornar a sua primeira pesquisa de imagens com o SDK de pesquisa de imagens do Bing, que é um wrapper para a API e contém os mesmos recursos. Esta aplicação de Java simple envia uma consulta de pesquisa de imagem, analisa a resposta JSON e apresenta o URL da imagem do primeiro devolvido.

O código-fonte para este exemplo está disponível [no GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart) com tratamento de erros adicionais e anotações. 

## <a name="prerequisites"></a>Pré-requisitos 

A versão mais recente do [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (JDK)

Instale as dependências de SDK de pesquisa de imagens do Bing com o Maven, Gradle ou outro sistema de gestão de dependência. O ficheiro POM do Maven requer a seguinte declaração:

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
 </dependencies> 
```

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Criar um novo projeto de Java no seu IDE ou editor favorito e adicione as seguintes importações à sua implimentation de classe:

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. O método principal criar variáveis para a sua chave de subscrição e termo de pesquisa. Em seguida, crie uma instância do cliente de pesquisa de imagens do Bing.

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## <a name="send-a-search-request-to-the-bing-image-search-api"></a>Enviar um pedido de pesquisa para a API de pesquisa de imagens do Bing

1. Usando `bingImages().search()`, enviar o pedido HTTP que contém a consulta de pesquisa. Guardar a resposta como uma `ImagesModel`.
    ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## <a name="parse-and-view-the-result"></a>Analisar e ver o resultado

Analisar os resultados de imagem devolvidos na resposta.
Se a resposta contém os resultados da pesquisa, armazenar o primeiro resultado e imprimir os detalhes, como uma miniatura devolveu o URL, o URL original, juntamente com o número total de imagens.  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte também 

* [O que é a pesquisa de imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Tente uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso de serviços cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Exemplos de Java para o SDK dos serviços cognitivos do Azure](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
* [Documentação dos serviços cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
