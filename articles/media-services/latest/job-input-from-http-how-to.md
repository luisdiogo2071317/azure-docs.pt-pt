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
ms.date: 09/24/2018
ms.author: juliako
ms.openlocfilehash: 8b8bfb578b9a7190e93da721531041bb93a9a03c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224737"
---
# <a name="create-a-job-input-from-an-https-url"></a>Criar uma entrada da tarefa a partir de um URL HTTPS

Em serviços de multimédia v3, quando submete tarefas para processar os seus vídeos, terá de informar os serviços de multimédia onde encontrar o vídeo de entrada. Uma das opções é especificar um URL de HTTP (s) como uma tarefa de entrada (como mostrado neste exemplo). Tenha em atenção que, AMS v3 não suporta atualmente a codificação através de HTTPS URLs da transferência. Para obter um exemplo completo, consulte [exemplo de github](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .NET

O código a seguir mostra como criar uma tarefa com um URL HTTPS de entrada.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).
