---
title: Tutorial do Python de API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Saiba como utilizar a API de rostos em com o SDK Python para detetar faces humanos numa imagem nos serviços cognitivos.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 408f9759262d6ae2193df3193ee1c306d384afe6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308793"
---
# <a name="getting-started-with-face-api-in-python-tutorial"></a>Introdução à API de letra tutorial do Python

Neste tutorial, ficará a saber invocar a API de rostos em através do SDK Python para detetar faces humanos numa imagem.

## <a name="prerequisites"></a> Pré-requisitos

Para utilizar o tutorial, terá de fazer o seguinte:

- Instale o Python 2.7 + ou Python 3.5 +.
- Instale o pip.
- Instale o SDK Python para a API de rosto da seguinte forma:

```bash
pip install cognitive_face
```

- Obter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/) cognitivos nos serviços da Microsoft. Pode utilizar o servidor primário ou a chave secundária neste tutorial. (Tenha em atenção que para utilizar qualquer API de letra, tem de ter uma chave de subscrição válido.)

## <a name="sdk-example"></a> Detetar um rosto numa imagem

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'  # Replace with your regional Base URL
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

Segue-se um resultado de exemplo. É um `list` de faces detetados. Cada item na lista é uma `dict` instância onde `faceId` é um ID exclusivo para a letra detetado e `faceRectangle` descreve a posição de rosto detetado. Um ID de rostos em expira dentro de 24 horas.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Desenhar retângulos à volta as faces

As coordenadas de json que recebeu o comando anterior pode desenhar retângulos na imagem para representar visualmente cada tipo de letra. Terá `pip install Pillow` para utilizar o `PIL` módulo de processamento de imagens.  Na parte superior do ficheiro, adicione o seguinte:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Em seguida, após `print(faces)`, incluem o seguinte no script:

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name='further'></a> Exploração adicional

Para ajudar a explorar ainda mais a API de letra, este tutorial fornece um exemplo de GUI. Para executá-la, instale primeiro [wxPython](https://wxpython.org/) , em seguida, execute os comandos abaixo.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a> Resumo

Neste tutorial, aprendeu o processo básico para utilizar a API de rostos em através de invocar o SDK Python. Para obter mais informações sobre os detalhes de API, consulte os procedimentos e [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> Tópicos relacionados

- [Introdução à API de rostos em CSharp](FaceAPIinCSharpTutorial.md)
- [Introdução à API em Java de letra para Android](FaceAPIinJavaForAndroidTutorial.md)
