---
title: Codificar transformação personalizada utilizando v3 de Media Services do Azure | Microsoft Docs
description: Este tópico mostra como utilizar Media Services do Azure v3 para codificar uma transformação personalizada.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/17/2018
ms.author: juliako
ms.openlocfilehash: d298070877a366d04b2df1ef8ac63b08f8771de0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655674"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Como codificar com uma transformação personalizada

Quando encoding com Media Services do Azure, pode começar rapidamente com um das predefinições de incorporada recomendadas com base nas melhores práticas industriais como é demonstrado no [ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md) tutorial, ou pode optar por criar um personalizado predefinição para os seus requisitos específicos de cenário ou dispositivo de destino. 

> [!Note]
> V3 de Media Services do Azure, todas as taxas de bits codificação estão no bits por segundo. Isto é diferente de v2 as restantes predefinições codificador de multimédia Standard. Por exemplo, velocidade de transmissão na v2 teria de ser especificada como 128, mas v3 seria possível 128000.

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo .NET Core completo para o seu computador utilizando o seguinte comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
O exemplo predefinido personalizado está localizado no [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) pasta.

## <a name="create-a-transform-with-a-custom-preset"></a>Criar uma transformação com uma predefinição personalizada 

Quando criar um novo [transformar](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar que pretende que produzem como resultado. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O seguinte **TransformOutput** cria definições personalizadas de codecs e a camada da saída.

Ao criar uma [Transformação](https://docs.microsoft.com/rest/api/media/transforms), primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Nos Media Services v3, **obter** métodos entidades devolvem **nulo** se a entidade não existir (Verifique sensível no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Passos Seguintes

[Ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md) 
