---
title: Criar um tutorial de Java de serviço de visão personalizada - serviços cognitivos do Azure | Documentos da Microsoft
description: Explore uma aplicação Java básica que utiliza a API de imagem digitalizada personalizado nos serviços cognitivos da Microsoft. Criar um projeto, adicionar etiquetas, carregar imagens, preparar seu projeto e fazer uma predição ao utilizar o ponto final predefinido.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: a83a2f5cac9281a4cd79c1a0cead0f2af82d73df
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305710"
---
# <a name="use-custom-vision-api-to-build-an-image-classification-project-with-java"></a>Utilize a API de visão personalizada para criar um projeto de classificação de imagem com o Java

Saiba como criar um projeto de classificação de imagem com o serviço de visão personalizada com Java. Depois de criado, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição de padrão do projeto e utilizá-lo para uma imagem de teste por meio de programação. Utilize este exemplo de código-fonte aberto como um modelo para criar sua própria aplicação com a API de visão personalizada.

## <a name="prerequisites"></a>Pré-requisitos

- JDK 7 ou 8 instalado.
- Maven instalado.

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de formação e predição

Para obter as chaves utilizadas neste exemplo, visite o [página da web de visão personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na __contas__ secção, copie os valores da __chave de treinamento__ e __predição chave__ campos.

![Imagem das chaves da interface do Usuário](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalar o SDK do serviço de visão personalizada

Pode instalar o SDK de visão personalizada do repositório central maven:
* [SDK de treinamento](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Previsão de SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Compreender o código

O projeto completo, incluindo imagens, está disponível a partir da [exemplos do Azure de visão personalizada para o repositório de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

Utilize o seu IDE favorito de Java para abrir o `Vision/CustomVision` projeto. 

Esta aplicação utiliza a chave de treinamento que obteve anteriormente para criar um novo projeto chamado __projeto de Java de exemplo__. Em seguida, carrega imagens para treinar e testar um classificador. O classificador identifica se uma árvore é um __Hemlock__ ou uma __cereja japonês__.

Os seguintes fragmentos de código implementam a funcionalidade principal deste exemplo:

## <a name="create-a-custom-vision-service-project"></a>Criar um projeto de serviço de visão personalizada

> [!IMPORTANT]
> Definir o `trainingApiKey` para o valor da chave de treinamento que obteve anteriormente.

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

O exemplo está configurado para incluir as imagens no pacote final. Imagens são lidos na seção de recursos de jar e carregadas para o serviço.

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

Os trechos de código anterior faz uso de duas funções de programa auxiliar que obtêm as imagens como fluxos de recursos e carregá-los para o serviço.

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

Esta ação cria a primeira iteração no projeto e marca essa iteração como a iteração de predefinição. 

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obter e utilizar o ponto final de predição de predefinido

> [!IMPORTANT]
> Definir o `predictionApiKey` para o valor da chave de predição obtido anteriormente.

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