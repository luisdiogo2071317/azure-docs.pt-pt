---
title: Modelo de domínio de início rápido do Python de Imagem Digitalizada | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, vai utilizar modelos de domínio para identificar celebridades e marcos numa imagem através da Imagem Digitalizada com o Python nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 357cab72c0a6c9a2254350c84cda91c366ac685a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43771935"
---
# <a name="quickstart-use-a-domain-model---rest-python"></a>Início rápido: Utilizar um modelo de domínio - REST, Python

Neste início rápido, vai utilizar modelos de domínio para identificar celebridades e pontos de referência numa imagem através da Imagem Digitalizada.

Pode executar este início rápido passo a passo com um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Para iniciar o Binder, selecione o botão seguinte:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="identify-celebrities-and-landmarks"></a>Identificar celebridades e marcos

Com o [método Reconhecer Conteúdo de Domínio Específico](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200), pode identificar um conjunto específico de objetos numa imagem. Os dois modelos de domínios específicos que estão atualmente disponíveis são _celebridades_ e _marcos_.

Para executar o exemplo, siga os passos seguintes:

1. Copie o código seguinte para um novo ficheiro de script do Python.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `vision_base_url` para a localização onde obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere o valor `image_url` para outra imagem.
1. Execute o script.

O código seguinte utiliza a biblioteca `requests` do Python para chamar a API de Análise de Imagem de Imagem Digitalizada. Devolve os resultados como um objeto JSON. A chave de API é passada pelo dicionário `headers`. O modelo a utilizar é passado pelo dicionário `params`.

## <a name="landmark-identification"></a>Identificação dos marcos

### <a name="recognize-landmark-request"></a>Reconhecer pedido de Marco

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

landmark_analyze_url = vision_base_url + "models/landmarks/analyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/f/f6/" + \
    "Bunker_Hill_Monument_2005.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'model': 'landmarks'}
data    = {'url': image_url}
response = requests.post(
    landmark_analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The
# most relevant landmark for the image is obtained from the 'result' property.
analysis = response.json()
assert analysis["result"]["landmarks"] is not []
print(analysis)
landmark_name = analysis["result"]["landmarks"][0]["name"].capitalize()

# Display the image and overlay it with the landmark name.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(landmark_name, size="x-large", y=-0.1)
```

### <a name="recognize-landmark-response"></a>Reconhecer resposta de Marco

O JSON devolve uma resposta de êxito, por exemplo:

```json
{
  "result": {
    "landmarks": [
      {
        "name": "Bunker Hill Monument",
        "confidence": 0.9768505096435547
      }
    ]
  },
  "requestId": "659a10cd-44bb-44db-9147-a295b853b2b8",
  "metadata": {
    "height": 1600,
    "width": 1200,
    "format": "Jpeg"
  }
}
```

## <a name="celebrity-identification"></a>Identificação de celebridade

### <a name="recognize-celebrity-request"></a>Reconhecer pedido de Celebridade

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

vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

celebrity_analyze_url = vision_base_url + "models/celebrities/analyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/d/d9/" + \
    "Bill_gates_portrait.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'model': 'celebrities'}
data    = {'url': image_url}
response = requests.post(
    celebrity_analyze_url, headers=headers, params=params, json=data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The
# most relevant celebrity for the image is obtained from the 'result' property.
analysis = response.json()
assert analysis["result"]["celebrities"] is not []
print(analysis)
celebrity_name = analysis["result"]["celebrities"][0]["name"].capitalize()

# Display the image and overlay it with the celebrity name.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(celebrity_name, size="x-large", y=-0.1)
```

### <a name="recognize-celebrity-response"></a>Reconhecer resposta de Celebridade

O JSON devolve uma resposta de êxito, por exemplo:

```json
{
  "result": {
    "celebrities": [
      {
        "faceRectangle": {
          "top": 123,
          "left": 156,
          "width": 187,
          "height": 187
        },
        "name": "Bill Gates",
        "confidence": 0.9993845224380493
      }
    ]
  },
  "requestId": "f14ec1d0-62d4-4296-9ceb-6b5776dc2020",
  "metadata": {
    "height": 521,
    "width": 550,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Passos seguintes

Explore uma aplicação do Python que utilize a Imagem Digitalizada para realizar o reconhecimento ótico de carateres (OCR); criar miniaturas com recorte inteligente; além de detetar, categorizar, etiquetar e descrever funcionalidades visuais, incluindo rostos, numa imagem. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial do Python de API de Imagem Digitalizada](../Tutorials/PythonTutorial.md)
