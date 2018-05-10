---
title: Criar uma entrada da tarefa de serviços de suporte de dados do Azure a partir de um URL de HTTP (s) | Microsoft Docs
description: Este tópico mostra como criar uma entrada de tarefa a partir de um URL de HTTP (s).
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
ms.openlocfilehash: a47dc3e4939dd27d7ac11be823a09b8a6cba1f60
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Criar uma entrada de tarefa a partir de um URL de HTTP (s)

V3 de Media Services, ao submeter tarefas para processar os seus vídeos, é necessário saber onde encontrar o vídeo de entrada de Media Services. Uma das opções é especificar um URL de HTTP (s) como uma tarefa de entrada (como o mostrado neste exemplo). Para obter um exemplo completo, consulte este [github exemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .net

O código seguinte mostra como criar uma tarefa com um URL de HTTPS de entrada.

```csharp
private static Job SubmitJob(IAzureMediaServicesClient client, string transformName, string jobName)
{
    Asset outputAsset = client.Assets.CreateOrUpdate(Guid.NewGuid().ToString() + "-output", new Asset());

    JobInputHttp jobInput = 
        new JobInputHttp(files: new[] { "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4" });

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAsset.Name),
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

[Criar uma entrada de tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).
