---
title: Utilizar o serviço de visão de personalizada a partir de uma aplicação c# - serviços cognitivos do Azure | Microsoft Docs
description: Explore uma básica c# aplicação que utiliza a API de visão personalizado nos serviços cognitivos da Microsoft. Criar um projeto, adicione etiquetas, carregar imagens, preparar o seu projeto e efetuar uma predição utilizando o ponto final predefinido.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 80cb022808748ed2c60dff7c363d6020cb4043a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352868"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Utilizar o serviço de visão personalizada de um C&#35; aplicação

Saiba como utilizar o serviço de visão personalizada de uma aplicação c#. Depois de criado, pode adicionar etiquetas, carregar imagens, preparar o projeto, obter o URL de ponto final de predição de predefinição do projeto e utilizar o ponto final para testar através de programação de uma imagem. Utilize este exemplo de open source como um modelo para criar a sua própria aplicação para Windows utilizando a API de serviço de visão personalizada.

## <a name="prerequisites"></a>Pré-requisitos

* Qualquer edição do Visual Studio 2015 ou 2017 para Windows.

* O [SDK do serviço de visão personalizada](http://github.com/Microsoft/Cognitive-CustomVision-Windows/). Isto inclui o exemplo e imagens utilizadas neste documento.

## <a name="get-the-training-and-prediction-keys"></a>Obter as chaves de formação e previsão

Para obter as chaves utilizadas neste exemplo, visite o [página web de visão personalizada](https://customvision.ai) e selecione o __engrenagem ícone__ no canto superior direito. No __contas__ secção, copie os valores do __formação chave__ e __predição chave__ campos.

![Imagem das chaves da IU](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>Compreender o código

No Visual Studio, abra o projeto localizado no `Samples/CustomVision.Sample/` diretório do projeto do SDK.

Esta aplicação utiliza a chave de formação que obteve anteriormente para criar um novo projeto com o nome __novo projeto do meu__. Em seguida, carrega imagens para dar formação e testar um classificador. O classificador identifica se uma árvore é um __Hemlock__ ou um __Cherry japonês__.

Os fragmentos de código seguinte implementam a funcionalidade principal deste exemplo:

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

* __Definir uma iteração predefinido para o ponto final de predição__:

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

1. Efetue as alterações seguintes para adicionar as chaves de formação e predição à aplicação:

    * Adicionar o __chave formação__ para a seguinte linha:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Adicionar o __chave predição__ para a seguinte linha:

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
