---
title: Criar uma entrada da tarefa de serviços de multimédia do Azure a partir de um ficheiro local | Documentos da Microsoft
description: Este tópico mostra como criar uma entrada da tarefa a partir de um ficheiro local.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 3b4c11c359c15f1275a16774b490c08b543572c3
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378718"
---
# <a name="create-a-job-input-from-a-local-file"></a>Criar uma entrada da tarefa a partir de um ficheiro local

Em serviços de multimédia v3, quando submete tarefas para processar os seus vídeos, terá de informar os serviços de multimédia onde encontrar o vídeo de entrada. O vídeo de entrada pode ser armazenado como um recurso de serviço de suporte de dados, caso em que crie um elemento de entrada com base num arquivo (armazenado localmente ou no armazenamento de Blobs do Azure). Este tópico mostra como criar uma entrada da tarefa a partir de um ficheiro local. Para obter um exemplo completo, consulte [exemplo de github](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .NET

O código a seguir mostra como criar um elemento de entrada e usá-lo como entrada para a tarefa. A função de CreateInputAsset executa as seguintes ações:

* Cria o elemento 
* Obtém um [URL de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) gravável para o [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) do Elemento
* Carrega o ficheiro para o contentor de armazenamento através do URL de SAS

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada da tarefa a partir de um URL HTTPS](job-input-from-http-how-to.md).
