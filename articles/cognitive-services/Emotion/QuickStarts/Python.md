---
title: Início rápido do emoções API Python | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de emoções com o Python nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/05/2018
ms.author: anroth
ms.openlocfilehash: ff1f6b2ddc872d0ee63d9885b04b1f007bc86e33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351631"
---
# <a name="emotion-api-python-quickstart"></a>Guia de introdução do emoções Python de API

> [!IMPORTANT]
> Vídeo de API de pré-visualização terminou no 30 de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Esta explicação passo a passo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o [emoções API reconhece o método](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com o Python para reconhecer os emotions expressados por um ou mais pessoas numa imagem. 

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: [ ![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Pré-requisito
Obter a chave de subscrição gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>Executar as instruções
Para continuar com estas instruções, substitua `subscription_key` com a chave de API que obteve anteriormente.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o URL do serviço corresponde à região utilizada ao definir a chave de API. Se estiver a utilizar uma chave de avaliação, não terá de efetuar quaisquer alterações.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Estas instruções utilizam imagens que são armazenadas no disco. Também pode utilizar as imagens que estão disponíveis através de um URL acessível publicamente. Para obter mais informações, consulte o [documentação da REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Uma vez que os dados de imagem são transmitidos como parte do corpo do pedido, tenha em atenção que tem de definir o `Content-Type` cabeçalho para `application/octet-stream`. Se está a transmitir numa imagem através de um URL, não se esqueça de definir o cabeçalho:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
Crie um dicionário que contém o URL:
```python
data = {'url': image_url}
```
e passar que para o `requests` biblioteca a utilizar:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Algumas imagens de exemplo do primeiro de transferir o [emoções API](https://azure.microsoft.com/services/cognitive-services/emotion/) site.


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



O objeto devolvido do JSON contém as caixas de delimitador de faces que foram reconhecidos, juntamente com os emotions detetados. Cada emoções está associada uma pontuação entre 0 e 1 onde uma pontuação superior é mais indicativa de uma emoções do que uma pontuação inferior. 

As seguintes linhas de código as emotions detetados no faces na imagem com o `matplotlib` biblioteca. Para reduzir clutter, apenas os três emotions principais são apresentados.


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

O `annotate_image` função mostrada seguinte pode ser utilizada para sobreposição emotions por cima de um ficheiro de imagem fornecidos o caminho no sistema de ficheiros. Se baseia-se no código para a chamada à API emoções apresentada anteriormente.


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

Por fim, o `annotate_image` função pode ser chamada num ficheiro de imagem, conforme mostrado na linha seguinte:


```python
annotate_image("images/emotion_2.jpg")
```
