---
title: Utilizar o serviço de visão personalizada a partir de um aplicativo c# - serviços cognitivos do Azure | Documentos da Microsoft
description: Explore uma c# aplicação básica que utiliza a API de imagem digitalizada personalizado nos serviços cognitivos da Microsoft. Criar um projeto, adicionar etiquetas, carregar imagens, preparar seu projeto e efetuar uma predição ao utilizar o ponto final predefinido.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d3c2ffb0fd9578458bd07241eed4a87cf70d3c3c
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617439"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Utilizar o serviço de visão personalizada de um C&#35; aplicação

Saiba como utilizar o serviço de visão personalizada de um aplicativo c#. Depois de criado, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição de padrão do projeto e utilizar o ponto final para uma imagem de teste por meio de programação. Utilize este exemplo de código-fonte aberto como um modelo para criar sua própria aplicação para o Windows com a API de serviço de visão personalizada.

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição do Visual Studio 2017 para Windows.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Obtenha o SDK de visão personalizada e exemplos
Para criar este exemplo, terá dos pacotes de NuGet do SDK de visão personalizada:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Pode baixar as imagens juntamente com o [amostras de c#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision).

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de formação e predição

Para obter as chaves utilizadas neste exemplo, visite o [página da web de visão personalizada](https://customvision.ai) e selecione o __ícone de engrenagem__ no canto superior direito. Na __contas__ secção, copie os valores da __chave de treinamento__ e __predição chave__ campos.

![Imagem das chaves da interface do Usuário](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Compreender o código

No Visual Studio, abra o projeto localizado no `Samples/CustomVision.Sample/` diretório do projeto do SDK.

Esta aplicação utiliza a chave de treinamento que obteve anteriormente para criar um novo projeto chamado __meu novo projeto__. Em seguida, carrega imagens para treinar e testar um classificador. O classificador identifica se uma árvore é um __Hemlock__ ou uma __cereja japonês__.

Os seguintes fragmentos de código implementam a funcionalidade principal deste exemplo:

* __Criar um novo projeto de serviço de visão personalizada__:

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

* __Carregar e marque as imagens__:

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

* __Treinar o classificador__:

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

* __Definir uma iteração predefinido para o ponto final de predição__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Criar um ponto de final de predição__:
 
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

1. Efetue as seguintes alterações para adicionar as chaves de formação e predição à aplicação:

    * Adicionar seu __chave de treinamento__ para a seguinte linha:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Adicionar seu __chave de predição__ para a seguinte linha:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Execute a aplicação. Como a aplicação é executada, o resultado seguinte é escrito no Console:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Prima qualquer tecla para sair do aplicativo.
