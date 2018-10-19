---
title: 'Tutorial: Criar um projeto de classificação de imagens – Serviço de Visão Personalizada, Java'
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
ms.openlocfilehash: 9a7f50e0eb33016d6a2d8f28be047b327135c51f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367360"
---
# <a name="tutorial-build-an-image-classification-project-with-java"></a>Tutorial: Criar um projeto de classificação de imagens com Java

Saiba como criar um projeto de classificação de imagens com o Serviço de Visão Personalizada ao utilizar Java. Depois de criar o projeto, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição predefinido do projeto e utilizá-lo para testar uma imagem de forma programática. Utilize este exemplo de open source como um modelo para criar a sua própria aplicação com a API de Visão Personalizada.

## <a name="prerequisites"></a>Pré-requisitos

- JDK 7 ou 8 instalado.
- Maven instalado.

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de preparação e de predição

Para obter as chaves utilizadas neste exemplo, visite a [página Web da Visão Personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na secção __Accounts__ (Contas), copie os valores dos campos __Training Key__ (Chave de Preparação) e __Prediction Key__ (Chave de Predição).

![Imagem da IU de chaves](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalar o SDK do Serviço de Visão Personalizada

Pode instalar o SDK de Visão Personalizada a partir do repositório central maven:
* [SDK de Preparação](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [SDK de Predição](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Compreender o código

O projeto completo, incluindo imagens, está disponível a partir do [repositório de exemplos de Visão Personalizada do Azure para Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

Utilize o seu IDE Java favorito para abrir o projeto `Vision/CustomVision`. 

Esta aplicação utiliza a chave de preparação que obteve anteriormente para criar um novo projeto com o nome __Projeto Java de Exemplo__. Em seguida, a aplicação carrega imagens para preparar e testar um classificador. O classificador identifica se se trata de uma árvore __Abioto__ ou __Cerejeira Japonesa__.

Os seguintes fragmentos de código implementam a funcionalidade principal deste exemplo:

## <a name="create-a-custom-vision-service-project"></a>Criar um projeto do Serviço de Visão Personalizada

> [!IMPORTANT]
> Defina a `trainingApiKey` para o valor da chave de preparação que obteve anteriormente.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Adicionar etiquetas ao seu projeto

```java
// create hemlock tag
Tag hemlockTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Hemlock")
    .execute();

// create cherry tag
Tag cherryTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Japanese Cherry")
    .execute();
```

## <a name="upload-images-to-the-project"></a>Carregar imagens para o projeto

O exemplo está configurado para incluir as imagens no pacote final. As imagens são lidas da secção de recursos do JAR e carregadas para o serviço.

```java
System.out.println("Adding images...");
for (int i = 1; i <= 10; i++) {
    String fileName = "hemlock_" + i + ".jpg";
    byte[] contents = GetImage("/Hemlock", fileName);
    AddImageToProject(trainer, project, fileName, contents, hemlockTag.id(), null);
}

for (int i = 1; i <= 10; i++) {
    String fileName = "japanese_cherry_" + i + ".jpg";
    byte[] contents = GetImage("/Japanese Cherry", fileName);
    AddImageToProject(trainer, project, fileName, contents, cherryTag.id(), null);
}
```

O código de fragmento anterior utiliza duas funções de programa auxiliar que obtêm as imagens como fluxos de recursos e carrega-as para o serviço.

```java
private static void AddImageToProject(Trainings trainer, Project project, String fileName, byte[] contents, UUID tag)
{
    System.out.println("Adding image: " + fileName);

    ImageFileCreateEntry file = new ImageFileCreateEntry()
        .withName(fileName)
        .withContents(contents);

    ImageFileCreateBatch batch = new ImageFileCreateBatch()
        .withImages(Collections.singletonList(file));

    batch = batch.withTagIds(Collections.singletonList(tag));

    trainer.createImagesFromFiles(project.id(), batch);
}

private static byte[] GetImage(String folder, String fileName)
{
    try {
        return ByteStreams.toByteArray(CustomVisionSamples.class.getResourceAsStream(folder + "/" + fileName));
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    return null;
}
```

## <a name="train-the-project"></a>Preparar o projeto

Esta ação cria a primeira iteração no projeto e marca esta iteração como a iteração predefinida. 

```java
System.out.println("Training...");
Iteration iteration = trainer.trainProject(project.id());

while (iteration.status().equals("Training"))
{
    System.out.println("Training Status: "+ iteration.status());
    Thread.sleep(1000);
    iteration = trainer.getIteration(project.id(), iteration.id());
}

System.out.println("Training Status: "+ iteration.status());
trainer.updateIteration(project.id(), iteration.id(), iteration.withIsDefault(true));
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obter e utilizar o ponto final de predição predefinido

> [!IMPORTANT]
> Defina a `predictionApiKey` para o valor da chave de predição que obteve anteriormente.

```java
final String predictionApiKey = "insert your prediction key here";
PredictionEndpoint predictClient = CustomVisionPredictionManager.authenticate(predictionApiKey);

// Use below for predictions from a url
// String url = "some url";
// ImagePrediction results = predictor.predictions().predictImage()
//                         .withProjectId(project.id())
//                         .withUrl(url)
//                         .execute();

// load test image
byte[] testImage = GetImage("/Test", "test_image.jpg");

// predict
ImagePrediction results = predictor.predictions().predictImage()
    .withProjectId(project.id())
    .withImageData(testImage)
    .execute();

for (Prediction prediction: results.predictions())
{
    System.out.println(String.format("\t%s: %.2f%%", prediction.tagName(), prediction.probability() * 100.0f));
}
```

## <a name="run-the-example"></a>Executar o exemplo

Para compilar e executar a solução com o maven:

```
mvn compile exec:java
```

O resultado da aplicação é semelhante ao seguinte texto:

```
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```