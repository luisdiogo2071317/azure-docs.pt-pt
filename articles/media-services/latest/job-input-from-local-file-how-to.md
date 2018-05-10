---
title: Criar uma entrada da tarefa de serviços de suporte de dados do Azure a partir de um ficheiro local | Microsoft Docs
description: Este tópico mostra como criar uma entrada de tarefa a partir de um ficheiro local.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 6ca2da7fc43e509bec357ae47ada3a2f57e79e78
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-job-input-from-a-local-file"></a>Criar uma entrada de tarefa a partir de um ficheiro local

V3 de Media Services, ao submeter tarefas para processar os seus vídeos, é necessário saber onde encontrar o vídeo de entrada de Media Services. O vídeo de entrada pode ser armazenado como um recurso de serviço de suporte de dados, caso em que cria um recurso de entrada com base num ficheiro (armazenado localmente ou no Blob storage do Azure). Este tópico mostra como criar uma entrada de tarefa a partir de um ficheiro local. Para obter um exemplo completo, consulte este [github exemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .NET

O código seguinte mostra como criar um recurso de entrada e utilizá-lo como entrada para a tarefa. A função de CreateInputAsset efetua as seguintes ações:

* Cria o elemento 
* Obtém um gravável [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) para o elemento [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container)
* Carrega o ficheiro para o contentor de armazenamento utilizando o URL de SAS

```csharp
private static Asset CreateInputAsset(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string assetName, string fileToUpload)
{
    // Call Media Services API to create an Asset.
    // This method creates a container in storage for the Asset.
    // The files (blobs) associated with the asset will be stored in this container.
    Asset asset = client.Assets.CreateOrUpdate(resourceGroupName, accountName, assetName, new Asset());

    // Use Media Services API to get back a response that contains
    // SAS URL for the Asset container into which to upload blobs.
    // That is where you would specify read-write permissions 
    // and the exparation time for the SAS URL.
    var response = client.Assets.ListContainerSas(resourceGroupName, accountName, assetName, new ListContainerSasInput()
    {
        Permissions = AssetContainerPermission.ReadWrite,
        ExpiryTime = DateTimeOffset.Now.AddHours(4)
    });

    var sasUri = new Uri(response.AssetContainerSasUrls.First());
    
    // Use Storage API to get a reference to the Asset container
    // that was created by calling Asset's CreateOrUpdate method.  
    CloudBlobContainer container = new CloudBlobContainer(sasUri);
    var blob = container.GetBlockBlobReference(Path.GetFileName(fileToUpload));
    
    // Use Strorage API to upload the file into the container in storage.
    blob.UploadFromFile(fileToUpload);

    return asset;
}

private static Job SubmitJob(IAzureMediaServicesClient client, string transformName, string jobName, string outputAssetName)
{
    string inputAssetName = Guid.NewGuid().ToString() + "-input";
    JobInput jobInput = new JobInputAsset(assetName: inputAssetName);

    CreateInputAsset(client, inputAssetName, "ignite.mp4")

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAssetName),
    };

    Job job = client.Jobs.CreateOrUpdate(
        transformName,
        jobName,
        new Job
        {
            Input = jobInput,
            Outputs = jobOutputs,
        });

    return job;
}
```

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada de tarefa a partir de um URL de HTTP (s)](job-input-from-http-how-to.md).
