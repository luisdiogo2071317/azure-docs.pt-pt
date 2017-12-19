---
title: "Como obter uma instância de processador de multimédia com REST | Microsoft Docs"
description: "Saiba como criar um componente de processador de suporte de dados para codificar, converter o formato, encriptar ou desencriptar o conteúdo de multimédia de Media Services do Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: 4e673a92a9740b96eac20cdf5673395bacca8b77
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Como obter uma instância de processador de multimédia
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Descrição geral
Processadores de suporte de dados são um componente que processa um vídeo específico ou a tarefa de processamento de áudio, tais como a codificação de conversão do formato, encriptação ou desencriptar os conteúdos de multimédia. Todas as tarefas submetidas para os Media Services requerem um processador de multimédia para codificar, encriptar ou converter o conteúdo de vídeo ou áudio. 

## <a name="azure-media-processors"></a>Processadores de Media Services do Azure 

O tópico seguinte fornece uma lista de processadores de suporte de dados:

* [Codificação processadores de multimédia](scenarios-and-availability.md#encoding-media-processors)
* [Processadores de multimédia de análise](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Ao aceder a entidades nos Media Services, tem de definir campos de cabeçalho específicos e os valores no seus pedidos HTTP. Para obter mais informações, consulte [programa de configuração para o desenvolvimento de API de REST de serviços de suporte de dados](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Ligar aos Media Services

Para obter informações sobre como ligar à API do AMS, consulte [aceder à API de serviços de suporte de dados do Azure com a autenticação do Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 


## <a name="get-a-media-processor"></a>Obter um processador de multimédia

A chamada REST seguinte mostra como obter uma instância de processador de suporte de dados por nome (neste caso, **codificador de multimédia Standard**). 

Pedido:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.17
    Host: media.windows.net

Resposta:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Passos Seguintes
Agora que sabe como obter uma instância de processador de suporte de dados, vá para o [como codificar um elemento](media-services-rest-get-started.md) artigo que demonstra como utilizar o codificador de multimédia Standard para codificar um elemento.

