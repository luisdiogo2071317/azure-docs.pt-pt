---
title: 'Início rápido: Reconhecer emoções no rostos numa imagem - API de emoções, Python'
description: Obtenha informações e exemplos de código para o ajudar a começar a utilizar rapidamente a API de Emoções com Python.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 02/05/2018
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: bdc42233d027134ca42e52398eeefd83ae45669f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211258"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Início rápido: Crie uma aplicação para reconhecer emoções no rostos numa imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Estas instruções disponibilizam informações e exemplos de código para ajudá-lo a começar a utilizar rapidamente o [método de Reconhecimento da API de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com Python para reconhecer emoções expressas por uma ou mais pessoas numa imagem.

Pode executar este exemplo como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org), ao clicar no destaque de lançamento do Binder: ![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Pré-requisito
Obtenha a sua Chave de Subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>Executar as instruções
Para continuar este tutorial, substitua `subscription_key` pela chave de API que obteve anteriormente.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o URL de serviço corresponde à região que utilizou ao configurar a chave de API. Se estiver a utilizar uma chave de avaliação, não tem de fazer quaisquer alterações.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Estas instruções utilizam imagens que estão armazenadas no disco. Também pode utilizar as imagens que estão disponíveis através de um URL acessível publicamente. Para obter mais informações, veja a [Documentação da API REST](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Uma vez que os dados de imagem são transmitidos como parte do corpo do pedido, tenha em atenção que tem de definir o cabeçalho `Content-Type` como `application/octet-stream`. Se estiver a transmitir uma imagem através de um URL, não se esqueça de definir o cabeçalho como:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
crie um dicionário que contenha o URL:
```python
data = {'url': image_url}
```
e transmita-o para a biblioteca `requests` com:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Em primeiro lugar, transfira algumas imagens de exemplo do site [API de Emoções](https://azure.microsoft.com/services/cognitive-services/emotion/).


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



O Objeto JSON devolvido contém as caixas delimitadoras dos rostos que foram reconhecidos, juntamente com as emoções detetadas. Cada emoção tem associada uma pontuação de 0 a 1, em que uma pontuação mais elevada é mais significativa do que uma emoção com uma pontuação inferior.

As linhas de código seguintes detetaram emoções nos rostos na imagem com a biblioteca `matplotlib`. Para reduzir a desorganização, apenas as três emoções principais são apresentadas.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

A função `annotate_image` mostrada a seguir pode ser utilizada para sobrepor emoções num ficheiro de imagem, de acordo com o caminho no sistema de ficheiros. Baseia-se no código para chamar a API de Emoções mostrado anteriormente.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Por fim, a função `annotate_image` pode ser chamada num ficheiro de imagem, conforme mostrado na linha seguinte:


```python
annotate_image("images/emotion_2.jpg")
```
