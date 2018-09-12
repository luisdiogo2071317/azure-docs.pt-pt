---
title: Início rápido de análise de uma imagem local da Imagem Digitalizada com Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai analisar uma imagem local através da Imagem Digitalizada com o Python nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: a1f3fce5a547f143f7c4884c6642e78f53d160e9
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43771914"
---
# <a name="quickstart-analyze-a-local-image---rest-python"></a>Início Rápido: Analisar uma imagem local – REST, Python

Neste início rápido, vai analisar uma imagem local através da Imagem Digitalizada. Para analisar uma imagem remota, veja [Analisar uma imagem remota com o Python](python-analyze.md).

Pode executar este início rápido passo a passo com um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Para iniciar o Binder, selecione o botão seguinte:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, precisa de uma chave de subscrição; veja [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-a-local-image"></a>Analisar uma imagem local

Este exemplo é semelhante a [Analisar uma imagem remota com o Python](python-analyze.md), exceto pelo facto de a imagem a analisar ser lida localmente a partir do disco. São precisas duas alterações:

- Adicione um cabeçalho `{"Content-Type": "application/octet-stream"}` ao pedido.
- Adicione os dados de imagem (matriz de bytes) ao corpo do pedido.

Para executar o exemplo, siga os seguintes passos:

1. Copie o seguinte código para um novo ficheiro de script do Python.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `vision_base_url` para a localização onde obteve as suas chaves de subscrição, se assim for preciso.
1. Altere o valor `image_path` para o caminho de uma imagem local.
1. Execute o script.

O código seguinte utiliza a biblioteca `requests` do Python para chamar a API de Imagem de Análise de Imagem Digitalizada. Devolve os resultados como um objeto JSON. A chave de API é passada pelo dicionário `headers`. Os tipos de funcionalidades para reconhecer são passadas pelo dicionário `params`. Os dados da imagem binária são passados pelo parâmetro `data` para `requests.post`.

## <a name="analyze-image-request"></a>Pedido de Análise de Imagem

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the westcentralus region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

analyze_url = vision_base_url + "analyze"

# Set image_path to the local path of an image that you want to analyze.
image_path = "C:/Documents/ImageToAnalyze.jpg"

# Read the image into a byte array
image_data = open(image_path, "rb").read()
headers    = {'Ocp-Apim-Subscription-Key': subscription_key,
              'Content-Type': 'application/octet-stream'}
params     = {'visualFeatures': 'Categories,Description,Color'}
response = requests.post(
    analyze_url, headers=headers, params=params, data=image_data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(analysis)
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(image_data))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
```

## <a name="analyze-image-response"></a>Resposta de Análise de Imagem

Uma resposta bem-sucedida é devolvida no JSON, por exemplo:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Passos seguintes

Explore uma aplicação do Python que utiliza a Imagem Digitalizada para realizar o reconhecimento ótico de carateres (OCR); criar miniaturas com recorte inteligente; além de detetar, categorizar, etiquetar e descrever funcionalidades visuais, incluindo rostos, numa imagem. Para experimentar rapidamente as APIs de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial do Python de API de Imagem Digitalizada](../Tutorials/PythonTutorial.md)
