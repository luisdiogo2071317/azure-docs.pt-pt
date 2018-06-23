---
title: Como utilizar a API de LCA anomalias com cURL - serviços cognitivos Microsoft | Microsoft Docs
description: Obter informações para ajudá-lo rapidamente começar a utilizar cURL e a API de LCA anomalias nos serviços cognitivos.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 3c1d791b8c0478715b4ffa93cd7dfa43f9be4586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353317"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>Utilize a API de LCA anomalias com cURL

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar a API de LCA anomalias com cURL para realizar tarefas de obter o resultado de anomalias de dados de séries de tempo.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Obter os pontos de anomalias com a API de LCA anomalias utilizando cURL 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Exemplo de dados de séries de tempo

Segue-se ao exemplo de tempo de pontos de dados de série.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Analisar dados e obter os pontos de anomalias cURL exemplo

Os passos da utilização de exemplo são os seguintes.

1. Substitua o `[YOUR_SUBSCRIPTION_KEY]` valor com a sua chave de subscrição válido.
2. Substitua o `[YOUR_REGION]` para utilizar a localização onde obteve as chaves de subscrição.
3. Substitua o `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` com o exemplo ou os seus pontos de dados.
4. Executar e verificar a resposta.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Resposta de exemplo
Uma resposta com êxito é devolvida em JSON. Exemplo de resposta é o seguinte: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
