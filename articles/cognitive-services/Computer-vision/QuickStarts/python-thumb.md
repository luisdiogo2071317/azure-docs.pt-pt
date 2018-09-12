---
title: Início rápido criar miniatura através da Imagem Digitalizada com Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá gerar uma miniatura a partir de uma imagem através da Imagem Digitalizada com Python nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: bc1d01cd4b8f15ba627d917825ee5205c34f5cdf
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772117"
---
# <a name="quickstart-generate-a-thumbnail---rest-python"></a>Início Rápido: Gerar uma miniatura – REST, Python

Neste início rápido, irá gerar uma miniatura de uma imagem através da Imagem Digitalizada.

Pode executar este início rápido passo a passo com um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Para iniciar o Binder, selecione o botão seguinte:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="intelligently-generate-a-thumbnail"></a>Gerar uma miniatura de forma inteligente

Com o [método Obter Miniatura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), pode gerar uma miniatura de uma imagem. Especifica a altura e a largura, que podem ser diferentes da proporção da imagem de entrada. A Imagem Digitalizada utiliza o recorte inteligente para identificar de forma inteligente a região de interesse e gerar coordenadas de recorte com base nessa região.

Para executar o exemplo, siga os passos seguintes:

1. Copie o código seguinte para um novo ficheiro de script do Python.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `vision_base_url` para a localização onde obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere o valor `image_url` para outra imagem.
1. Execute o script.

O código seguinte utiliza a biblioteca `requests` do Python para chamar a API de Análise de Imagem de Imagem Digitalizada. A chave de API é passada pelo dicionário `headers`. O tamanho da miniatura é passado pelo `params` dicionário. A miniatura é devolvida como uma matriz de bytes na resposta.

## <a name="get-thumbnail-request"></a>Pedido Obter Miniatura

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

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data    = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

## <a name="next-steps"></a>Passos seguintes

Explore uma aplicação do Python que utilize a Imagem Digitalizada para realizar o reconhecimento ótico de carateres (OCR); criar miniaturas com recorte inteligente; além de detetar, categorizar, etiquetar e descrever funcionalidades visuais, incluindo rostos, numa imagem. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial do Python de API de Imagem Digitalizada](../Tutorials/PythonTutorial.md)
