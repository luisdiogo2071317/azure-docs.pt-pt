---
title: Criar um tutorial de Python de serviço de visão personalizada - serviços cognitivos do Azure | Documentos da Microsoft
description: Explore uma aplicação Python básica que utiliza a API de imagem digitalizada personalizado nos serviços cognitivos da Microsoft. Criar um projeto, adicionar etiquetas, carregar imagens, preparar seu projeto e fazer uma predição ao utilizar o ponto final predefinido.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: df0bdc0bbd2768566336323851f366c9ae280a88
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301604"
---
# <a name="custom-vision-api-python-tutorial"></a>Tutorial de Python de API de visão personalizada

Saiba como criar um projeto de classificação de imagem com o serviço de visão personalizada e um script de Python básico. Depois de criado, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição de padrão do projeto e utilizá-lo para uma imagem de teste por meio de programação. Utilize este exemplo de código-fonte aberto como um modelo para criar sua própria aplicação com a API de visão personalizada.



## <a name="prerequisites"></a>Pré-requisitos

- Python 2.7 + ou Python 3.5 +.
- A ferramenta de pip.

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de formação e predição

Para obter as chaves utilizadas neste exemplo, visite o [página da web de visão personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na __contas__ secção, copie os valores da __chave de treinamento__ e __predição chave__ campos.

![Imagem das chaves da interface do Usuário](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalar o SDK do serviço de visão personalizada

Para instalar o SDK de serviço de visão personalizada, utilize o seguinte comando:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Obter imagens de exemplo

Este exemplo utiliza as imagens a partir da `Samples/Images` diretório da [ https://github.com/Microsoft/Cognitive-CustomVision-Windows ](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) projeto. Clonar ou transferir e extrair o projeto para o ambiente de desenvolvimento.

## <a name="create-a-custom-vision-service-project"></a>Criar um projeto de serviço de visão personalizada

Para criar um novo projeto de serviço de visão personalizada, crie o novo ficheiro designado `sample.py`. Utilize o seguinte código como o conteúdo do ficheiro:

> [!IMPORTANT]
> Definir o `training_key` para o valor da chave de treinamento que obteve anteriormente.
>
> Definir o `prediction_key` para o valor da chave de predição obtido anteriormente.

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

Para adicionar etiquetas ao seu projeto, adicione o seguinte código ao final do `sample.py` ficheiro:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Carregar imagens para o projeto

Para adicionar imagens de exemplo para o projeto, insira o seguinte código após a criação de marca. Esse código carrega a imagem com a marca correspondente:

> [!IMPORTANT]
>
> Altere o caminho para as imagens com base em onde transferiu o projeto de cognitivos-CustomVision-Windows anteriormente.

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

Para treinar o classificador, adicione o seguinte código ao final do `sample.py` ficheiro:

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obter e utilizar o ponto final de predição de predefinido

Para enviar uma imagem para o ponto final de predição e obter a previsão, adicione o seguinte código ao final do `sample.py` ficheiro:

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

Execute a solução. Os resultados de predição são apresentados na consola.

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