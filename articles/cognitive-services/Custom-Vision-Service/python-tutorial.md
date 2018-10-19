---
title: 'Tutorial: Criar um projeto de classificação de imagens – Serviço de Visão Personalizada, Python'
titlesuffix: Azure Cognitive Services
description: Crie um projeto, adicione etiquetas, carregue imagens, prepare o seu projeto e faça uma predição com um ponto final predefinido.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 14b805a60637a889698132e169d5a41670a8bce0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363382"
---
# <a name="tutorial-create-an-image-classification-project-using-the-custom-vision-service-with-python"></a>Tutorial: Criar um projeto de classificação de imagens com o Serviço de Visão Personalizada com Python

Saiba como criar um projeto de classificação de imagens com o Serviço de Visão Personalizada e um script de Python básico. Depois de criar o projeto, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição predefinido do projeto e utilizá-lo para testar uma imagem de forma programática. Utilize este exemplo de open source como um modelo para criar a sua própria aplicação com a API de Visão Personalizada.



## <a name="prerequisites"></a>Pré-requisitos

- Python 2.7 ou versões posteriores ou Python 3.5. ou versões posteriores
- A ferramenta PIP.

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de preparação e de predição

Para obter as chaves utilizadas neste exemplo, visite a [página Web da Visão Personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na secção __Accounts__ (Contas), copie os valores dos campos __Training Key__ (Chave de Preparação) e __Prediction Key__ (Chave de Predição).

![Imagem da IU de chaves](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalar o SDK do Serviço de Visão Personalizada

Para instalar o SDK do Serviço de Visão Personalizada, utilize o seguinte comando:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Obter imagens de exemplo

Este exemplo utiliza as imagens a partir do diretório `Samples/Images` do projeto [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images). Clone ou transfira e extrai o projeto para o seu ambiente de desenvolvimento.

## <a name="create-a-custom-vision-service-project"></a>Criar um projeto do Serviço de Visão Personalizada

Para criar um novo projeto do Serviço de Visão Personalizada, crie um novo ficheiro com o nome `sample.py`. Utilize o seguinte código como conteúdo do ficheiro:

> [!IMPORTANT]
> Defina a `training_key` para o valor da chave de preparação que obteve anteriormente.
>
> Defina a `prediction_key` para o valor da chave de predição que obteve anteriormente.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Adicionar etiquetas ao seu projeto

Para adicionar etiquetas ao seu projeto, adicione o seguinte código no fim do ficheiro `sample.py`:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Carregar imagens para o projeto

Para adicionar as imagens de exemplo ao projeto, insira o seguinte código após a criação da etiqueta. Este código carrega a imagem com a etiqueta correspondente:

> [!IMPORTANT]
>
> Altere o caminho para as imagens com base no local para onde transferiu o projeto Cognitive-CustomVision-Windows anteriormente.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Preparar o projeto

Para preparar o classificador, adicione o seguinte código no fim do ficheiro `sample.py`:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obter e utilizar o ponto final de predição predefinido

Para enviar uma imagem para o ponto final de predição e obter a predição, adicione o seguinte código no fim do ficheiro `sample.py`:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Executar o exemplo

Execute a solução. Os resultados da predição aparecem na consola.

```
python sample.py
```

O resultado da aplicação é semelhante ao seguinte texto:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```