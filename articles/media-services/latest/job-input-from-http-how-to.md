---
title: Criar uma entrada da tarefa de serviços de multimédia do Azure a partir de um URL HTTPS | Documentos da Microsoft
description: Este tópico mostra como criar uma entrada da tarefa a partir de um URL de HTTP (s).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: 1760c63bee6bc1b0a631499a8875bedb34e71afd
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308847"
---
# <a name="create-a-job-input-from-an-https-url"></a>Criar uma entrada da tarefa a partir de um URL HTTPS

Em serviços de multimédia v3, quando submete tarefas para processar os seus vídeos, terá de informar os serviços de multimédia onde encontrar o vídeo de entrada. Uma das opções é especificar um URL HTTPS como uma tarefa de entrada (como mostrado neste exemplo). Note que, atualmente, o AMS v3 não suporta a codificação de transferência segmentada através de URLs HTTPS. Para obter um exemplo completo, consulte [exemplo de GitHub](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .NET

O código a seguir mostra como criar uma tarefa com um URL HTTPS de entrada.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).
