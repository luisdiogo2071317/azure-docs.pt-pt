---
title: Como utilizar a API de LCA anomalias com Ruby - serviços cognitivos Microsoft | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar Ruby e a API de LCA anomalias nos serviços cognitivos.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: ca4754514ba5012f7e9e28981d0869d174561fb3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353324"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>Utilizar a API de LCA anomalias com Ruby

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar a API de LCA anomalias com Ruby para realizar tarefas de obter o resultado de deteção de anomalias de dados de séries de tempo.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Ao obter pontos de anomalias com anomalias LCA API utilizando Ruby 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Exemplo de dados de séries de tempo
O exemplo de tempo de pontos de dados de séries é o seguinte, [!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Analisar dados e obter os pontos de anomalias exemplo Ruby

Os passos da utilização de exemplo são os seguintes.

1. Instalar [cliente rest](https://github.com/rest-client/rest-client) executando 'gem de instalação-cliente de rest'.
2. Guarde abaixo código como um ficheiro de .rb.
3. Substitua o `[YOUR_SUBSCRIPTION_KEY]` valor com a sua chave de subscrição válido.
4. Substitua o `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` com o exemplo ou os seus pontos de dados.
5. Executar e verificar a resposta.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Resposta de exemplo

Uma resposta com êxito é devolvida em JSON. Segue-se a resposta de amostra.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
