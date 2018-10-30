---
title: 'Tutorial: criar um projeto de classificação de imagens com o SDK de Visão Personalizada para C#'
titlesuffix: Azure Cognitive Services
description: Crie um projeto, adicione etiquetas, carregue imagens, prepare o seu projeto e faça uma predição com o ponto final predefinido.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: e046fe452a13384ae7929be805c6252d6ad2fbf9
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953048"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-c"></a>Tutorial: criar um projeto de classificação de imagens com o SDK de Visão Personalizada para C#

Saiba como utilizar o SDK de serviço de Visão Personalizada numa aplicação do C#. Depois de criado, poderá adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição predefinido do projeto e utilizar o ponto final para testar uma imagem de forma programática. Utilize este exemplo de open source como um modelo para criar a sua própria aplicação para Windows com a API do Serviço de Visão Personalizada.

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição do Visual Studio 2017 para Windows.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Obter o SDK de Visão Personalizada e exemplos
Para criar este exemplo, precisará de Pacotes NuGet do SDK de Visão Personalizada:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Pode transferir as imagens com os [Exemplos de C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision).

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de preparação e de predição

Para obter as chaves utilizadas neste exemplo, visite a [página Web da Visão Personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na secção __Accounts__ (Contas), copie os valores dos campos __Training Key__ (Chave de Preparação) e __Prediction Key__ (Chave de Predição).

![Imagem da IU de chaves](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Compreender o código

No Visual Studio, abra o projeto localizado no diretório `Samples/CustomVision.Sample/` do projeto do SDK.

Esta aplicação utiliza a chave de preparação que obteve anteriormente para criar um novo projeto com o nome __O Meu Novo Projeto__. Em seguida, a aplicação carrega imagens para preparar e testar um classificador. O classificador identifica se se trata de uma árvore __Abioto__ ou __Cerejeira Japonesa__.

Os seguintes fragmentos de código implementam a funcionalidade principal deste exemplo:

* __Criar um novo projeto do Serviço de Visão Personalizada__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Criar etiquetas num projeto__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Carregar e etiquetar imagens__:

    ```csharp
    // Add some images to the tags
    Console.WriteLine("\tUploading images");
    LoadImagesFromDisk();

    // Images can be uploaded one at a time
    foreach (var image in hemlockImages)
    {
        using (var stream = new MemoryStream(File.ReadAllBytes(image)))
        {
            trainingApi.CreateImagesFromData(project.Id, stream, new List<string>() { hemlockTag.Id.ToString() });
        }
    }

    // Or uploaded in a single batch 
    var imageFiles = japaneseCherryImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
    trainingApi.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(imageFiles, new List<Guid>() { japaneseCherryTag.Id }));
    ```

* __Preparar o classificador__:

    ```csharp
    // Now there are images with tags start training the project
    Console.WriteLine("\tTraining");
    var iteration = trainingApi.TrainProject(project.Id);

    // The returned iteration will be in progress, and can be queried periodically to see when it has completed
    while (iteration.Status == "Completed")
    {
        Thread.Sleep(1000);

        // Re-query the iteration to get it's updated status
        iteration = trainingApi.GetIteration(project.Id, iteration.Id);
    }
    ```

* __Definir uma iteração predefinida para o ponto final de predição__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Criar um ponto final de predição__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Enviar uma imagem para o ponto final de predição__:

    ```csharp
    // Make a prediction against the new project
    Console.WriteLine("Making a prediction:");
    var result = endpoint.PredictImage(project.Id, testImage);

    // Loop over each prediction and write out the results
    foreach (var c in result.Predictions)
    {
        Console.WriteLine($"\t{c.TagName}: {c.Probability:P1}");
    }
    ```

## <a name="run-the-application"></a>Executar a aplicação

1. Faça as seguintes alterações para adicionar as chaves de preparação e predição à aplicação:

    * Adicione a sua __chave de preparação__ à seguinte linha:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Adicione a sua __chave de predição__ à seguinte linha:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Execute a aplicação. Como a aplicação é executada, o seguinte resultado é escrito na consola:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Prima qualquer tecla para sair da aplicação.
