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
ms.openlocfilehash: 94e7192e13397ad8ec973d92f4c538f430c9cd60
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-a-local-file"></a>Criar uma entrada de tarefa a partir de um ficheiro local

V3 de Media Services, ao submeter tarefas para processar os seus vídeos, é necessário saber onde encontrar o vídeo de entrada de Media Services. O vídeo de entrada pode ser armazenado como um recurso de serviço de suporte de dados, caso em que cria um recurso de entrada com base num ficheiro (armazenado localmente ou no Blob storage do Azure). Este tópico mostra como criar uma entrada de tarefa a partir de um ficheiro local. Para obter um exemplo completo, consulte este [github exemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .NET

O código seguinte mostra como criar um recurso de entrada e utilizá-lo como entrada para a tarefa. A função de CreateInputAsset efetua as seguintes ações:

* Cria o elemento 
* Obtém um gravável [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) para o elemento [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container)
* Carrega o ficheiro para o contentor de armazenamento utilizando o URL de SAS

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada de tarefa a partir de um URL de HTTP (s)](job-input-from-http-how-to.md).
