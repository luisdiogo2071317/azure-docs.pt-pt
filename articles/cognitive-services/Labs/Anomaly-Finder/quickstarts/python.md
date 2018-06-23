---
title: Como utilizar a API de LCA anomalias com o Python - serviços cognitivos Microsoft | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar anomalias LCA com o Python nos serviços cognitivos.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c14916b0644edab613b298d6e71f8bbb9a6bb804
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353311"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Utilizar a API de LCA anomalias com o Python

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar a API de LCA anomalias com o Python para realizar tarefas de obter o resultado de anomalias para dados de séries de tempo.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Ao obter pontos de anomalias com anomalias LCA API com o Python 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Exemplo de dados de séries de tempo

Segue-se ao exemplo de tempo de pontos de dados de série.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Analisar dados e obter o exemplo de Python de pontos de anomalias

Certifique-se de que instalou python3, em seguida, crie um ficheiro executável do python com o nome detect.py. No detect.py, deve incluir o código abaixo. Antes de executar o código, não se esqueça de substituir o `[YOUR_SUBSCRIPTION_KEY]` valor com a sua chave de subscrição válido.
Substitua o `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` com os pontos de dados.

```python
import requests
import json


def detect(url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(url, data=json.dumps(request_data), headers=headers)
    if response.status_code == 200:
        return json.loads(response.content.decode("utf-8"))
    else:
        print(response.status_code)
        raise Exception(response.text)


sample_data = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]"
endpont = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
subscription_key = "[YOUR_SUBSCRIPTION_KEY]"

result = detect(endpont, subscription_key, sample_data)
print(result)

```

### <a name="example-response"></a>Resposta de exemplo

Uma resposta com êxito é devolvida em JSON. Segue-se a resposta de amostra.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aplicação de Python](../tutorials/python-tutorial.md)
