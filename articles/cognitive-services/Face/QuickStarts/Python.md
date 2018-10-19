---
title: 'Guia de Início Rápido: Detetar rostos numa imagem – API Face, Python'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, irá detetar rostos numa imagem através da API Face com o Python.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 05/24/2018
ms.author: nolachar
ms.openlocfilehash: 7e1185fed1892dbd2c60434cfceec555bf7b0bc5
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129221"
---
# <a name="quickstart-detect-faces-in-an-image-using-python"></a>Guia de Início Rápido: Detetar rostos numa imagem com Python

Neste guia de início rápido, irá detetar rostos humanos numa imagem remota com o serviço Face. Os rostos detetados são delimitados por retângulos e sobrepostos com o género e a idade de cada pessoa. Para utilizar uma imagem local, veja a sintaxe em [Imagem Digitalizada: analisar uma imagem local com o Python](../../Computer-vision/QuickStarts/python-disk.md).

Pode executar este guia de início rápido sob a forma de um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Para iniciar o Binder, selecione o botão seguinte:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma chave de subscrição para executar o exemplo. Pode obter chaves de subscrição de avaliação gratuita em [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

## <a name="detect-faces-in-an-image"></a>Detetar rostos numa imagem

Utilize o método [Face - Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) (Rosto – Detetar) para detetar rostos numa imagem e devolver atributos, incluindo:

* Face ID: ID exclusivo utilizado em vários cenários da API Face.
* Retângulo de Rostos: a esquerda, o topo, a largura e a altura indicam a localização do rosto na imagem, a largura e a altura indicam a localização do rosto na imagem.
* Marcos: uma matriz de marcos de rosto com 27 pontos aponta para as posições importantes dos componentes do rosto.
* Atributos faciais, como a idade, o sexo, a intensidade do sorriso, a posição da cabeça e os pelos faciais.

Para executar o exemplo, siga os seguintes passos:

1. Copie o código seguinte para um novo ficheiro de script do Python.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `face_api_url` para a localização onde obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere o valor `image_url` para outra imagem.
1. Execute o script.

### <a name="face---detect-request"></a>Rosto – Pedido de deteção

O código seguinte utiliza a biblioteca `requests` do Python para chamar a API Face Detect. Devolve os resultados como um objeto JSON. A chave de API é passada pelo dicionário `headers`. Os tipos de funcionalidades para reconhecer são passadas pelo dicionário `params`.

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from matplotlib import patches
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
face_api_url = 'https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect'

# Set image_url to the URL of an image that you want to analyze.
image_url = 'https://how-old.net/Images/faces2/main007.jpg'

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'false',
    'returnFaceAttributes': 'age,gender,headPose,smile,facialHair,glasses,' +
    'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise'
}
data = {'url': image_url}
response = requests.post(face_api_url, params=params, headers=headers, json=data)
faces = response.json()

# Display the original image and overlay it with the face information.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.figure(figsize=(8, 8))
ax = plt.imshow(image, alpha=0.6)
for face in faces:
    fr = face["faceRectangle"]
    fa = face["faceAttributes"]
    origin = (fr["left"], fr["top"])
    p = patches.Rectangle(
        origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    plt.text(origin[0], origin[1], "%s, %d"%(fa["gender"].capitalize(), fa["age"]),
             fontsize=20, weight="bold", va="bottom")
_ = plt.axis("off")
```

### <a name="face---detect-response"></a>Rosto – Resposta da deteção

É devolvida uma resposta com êxito em JSON, por exemplo:

```json
[
  {
    "faceId": "35102aa8-4263-4139-bfd6-185bb0f52d88",
    "faceRectangle": {
      "top": 208,
      "left": 228,
      "width": 91,
      "height": 91
    },
    "faceAttributes": {
      "smile": 1,
      "headPose": {
        "pitch": 0,
        "roll": 4.3,
        "yaw": -0.3
      },
      "gender": "female",
      "age": 27,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 1,
        "neutral": 0,
        "sadness": 0,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "low",
        "value": 0
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.65
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.06,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "blond",
            "confidence": 0.5
          },
          {
            "color": "black",
            "confidence": 0.34
          },
          {
            "color": "red",
            "confidence": 0.32
          },
          {
            "color": "gray",
            "confidence": 0.14
          },
          {
            "color": "other",
            "confidence": 0.03
          }
        ]
      }
    }
  },
  {
    "faceId": "42502166-31bb-4ac8-81c0-a7adcb3b3e70",
    "faceRectangle": {
      "top": 109,
      "left": 125,
      "width": 79,
      "height": 79
    },
    "faceAttributes": {
      "smile": 1,
      "headPose": {
        "pitch": 0,
        "roll": 1.7,
        "yaw": 2.1
      },
      "gender": "male",
      "age": 32,
      "facialHair": {
        "moustache": 0.4,
        "beard": 0.4,
        "sideburns": 0.4
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 1,
        "neutral": 0,
        "sadness": 0,
        "surprise": 0
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.11
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.74
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": false,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0.02,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "blond",
            "confidence": 0.94
          },
          {
            "color": "red",
            "confidence": 0.76
          },
          {
            "color": "gray",
            "confidence": 0.2
          },
          {
            "color": "other",
            "confidence": 0.03
          },
          {
            "color": "black",
            "confidence": 0.01
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore as API Face utilizadas para detetar rostos humanos numa imagem, demarcar os rostos com retângulos e devolver atributos, como a idade e o sexo.

> [!div class="nextstepaction"]
> [APIs Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
