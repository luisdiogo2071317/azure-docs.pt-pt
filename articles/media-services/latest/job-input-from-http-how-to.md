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
ms.openlocfilehash: d429665de64dacc5818d1d26c2a9029531cd39b3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Criar uma entrada de tarefa a partir de um URL de HTTP (s)

V3 de Media Services, ao submeter tarefas para processar os seus vídeos, é necessário saber onde encontrar o vídeo de entrada de Media Services. Uma das opções é especificar um URL de HTTP (s) como uma tarefa de entrada (como o mostrado neste exemplo). Para obter um exemplo completo, consulte este [github exemplo](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Exemplo de .net

O código seguinte mostra como criar uma tarefa com um URL de HTTPS de entrada.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Passos Seguintes

[Criar uma entrada de tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md).
