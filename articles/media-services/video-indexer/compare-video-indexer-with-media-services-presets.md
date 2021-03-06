---
title: Comparação das configurações predefinidas de v3 do Video Indexer e dos serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico compara o Video Indexer e configurações predefinidas de v3 de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: b4c74a67c66b12e3a7333f3e679e7c61a750cf7e
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002396"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Comparar configurações predefinidas de v3 de serviços de multimédia do Azure e o Video Indexer 

Este artigo compara as capacidades do **as APIs do indexador de vídeo** e **dos serviços de multimédia v3 APIs**. 

Atualmente, existe uma sobreposição entre recursos oferecidos pelos [APIs de v2 do Video Indexer](https://api-portal.videoindexer.ai/) e o [dos serviços de multimédia v3 APIs](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). A tabela a seguir oferece a diretriz atual para compreender as diferenças e semelhanças. 

## <a name="compare"></a>Comparar

|Funcionalidade|APIs de indexador de vídeos |Analisador de vídeo e áudio Analyzer predefinições<br/>nas APIs de v3 de serviços de multimédia|
|---|---|---|
|Informações de suporte de dados|[Avançada](video-indexer-output-json-v2.md) |[Noções Básicas](../latest/intelligence-concept.md)|
|Experiências|Ver a lista completa das funcionalidades suportadas: <br/> [Descrição geral](video-indexer-overview.md)|Devolve apenas informações de vídeo|
|Faturação|[Preços dos serviços de multimédia](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Preços dos serviços de multimédia](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Conformidade|[Conformidade do Azure](https://aka.ms/AzureCompliance)|Serviços de multimédia estão em conformidade com várias certificações. Confira [Offerings.pdf de conformidade do Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) e procure "Serviços de multimédia" ver se ele está em conformidade com um certificado de interesse.|
|Avaliação gratuita|EUA Leste|Não disponível|
|Disponibilidade |E.U.A. oeste, Ásia Oriental, Europa do Norte|Ver [estado do Azure](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Passos Seguintes

[Descrição geral do Video Indexer](video-indexer-overview.md)

[Descrição geral do serviços de multimédia v3](../latest/media-services-overview.md)
