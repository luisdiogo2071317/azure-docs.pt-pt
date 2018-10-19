---
title: 'Tutorial: Detetar e enquadrar rostos numa imagem – API Face, Python'
titleSuffix: Azure Cognitive Services
description: Saiba como utilizar a API Face com o SDK Python para detetar rostos numa imagem.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6cc3ac25d2196c0275b445503b79b9ac06a791d3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127742"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>Tutorial: Detetar e enquadrar rostos com API Face e Python 

Neste tutorial, irá aprender a invocar a API Face através do SDK Python para detetar rostos numa imagem.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o tutorial, terá de fazer o seguinte:

- Instale o Python 2.7+ ou Python 3.5+.
- Instale o pip.
- Instale o SDK Python para a API Face da seguinte forma:

```bash
pip install cognitive_face
```

- Obtenha uma [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/) dos Serviços Cognitivos da Microsoft. Pode utilizar a sua chave principal ou secundária neste tutorial. (Tenha em atenção que, para utilizar uma API Face, tem de ter uma chave de subscrição válida.)

## <a name="detect-a-face-in-an-image"></a>Detetar um Rosto numa Imagem

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

Segue-se um resultado de exemplo. É uma `list` de rostos detetados. Cada item na lista é uma instância `dict` em que `faceId` é um ID exclusivo do rosto detetado e `faceRectangle` descreve a posição do rosto detetado. Um Face ID expira dentro de 24 horas.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Desenhar retângulos em torno dos rostos

Utilizando as coordenadas do JSON que recebeu do comando anterior, pode desenhar retângulos na imagem para representar visualmente cada rosto. Precisará de `pip install Pillow` para utilizar o módulo de imagem `PIL`.  Na parte superior do ficheiro, adicione o seguinte:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Em seguida, após `print(faces)`, inclua o seguinte no seu script:

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

## <a name="further-exploration"></a>Mais Exploração

Para ajudá-lo a explorar a API Face, este tutorial fornece uma GUI de exemplo. Para executar, instale o [wxPython](https://wxpython.org/pages/downloads/) e, em seguida, execute os comandos abaixo.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>Resumo

Neste tutorial, aprendeu o processo básico para utilizar a API Face ao invocar o SDK Python. Para obter mais informações sobre os detalhes da API, veja o Guia de Procedimentos e a [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Tópicos Relacionados

- [Introdução à API Face no CSharp](FaceAPIinCSharpTutorial.md)
- [Introdução à API Face em Java para Android](FaceAPIinJavaForAndroidTutorial.md)
