---
title: Codificar com serviços de multimédia do Azure v3 de transformação personalizados | Documentos da Microsoft
description: Este tópico mostra como utilizar serviços de multimédia do Azure v3 para codificar uma transformação personalizada.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: c3318020b54beb121ac48d993a8ce794ac8e377f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376640"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Como codificar com uma transformação personalizados

Ao codificar com os serviços de multimédia do Azure, pode começar a utilizar rapidamente com um dos pré-visualizando incorporada recomendado com base nas práticas recomendadas do setor, como demonstrado na [ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md) tutorial, ou pode optar por criar um personalizado configuração predefinida para os seus requisitos específicos de cenário ou dispositivo de destino. 

> [!Note]
> Em serviços de multimédia do Azure v3, todas as taxas de bits de codificação são em bits por segundo. Isto é diferente da v2 REST que predefine Media Encoder Standard. Por exemplo, a velocidade de transmissão no v2 teria de ser especificada como 128, mas em v3 seria 128000.

## <a name="download-the-sample"></a>Transferir o exemplo

Clone um repositório do GitHub que contém o exemplo completo do .NET Core para o seu computador com o seguinte comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
O exemplo predefinido personalizado está localizado no [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/) pasta.

## <a name="create-a-transform-with-a-custom-preset"></a>Criar uma transformação com uma configuração predefinida personalizada 

Ao criar um novo [transformar](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar o que deseja produzir como saída. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O seguinte procedimento **TransformOutput** cria definições personalizadas de codec e a camada da saída.

Ao criar uma [Transformação](https://docs.microsoft.com/rest/api/media/transforms), primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Em serviços de multimédia v3, **Obtenha** métodos em entidades retornar **nulo** se a entidade não existe (uma verificação de maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Passos Seguintes

[Ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md) 
