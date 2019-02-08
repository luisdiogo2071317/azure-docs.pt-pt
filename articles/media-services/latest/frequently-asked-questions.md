---
title: Serviços de multimédia do Azure v3 perguntas mais frequentes | Documentos da Microsoft
description: Este artigo responde às perguntas mais frequentes de v3 de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/05/2019
ms.author: juliako
ms.openlocfilehash: a447c359c38c2173ea42b6d717067fc8b3a88f9a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875496"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Serviços de multimédia do Azure v3 perguntas mais frequentes

Este artigo responde às perguntas mais frequentes de v3 de serviços de multimédia do Azure (AMS).

## <a name="v3-apis"></a>v3 APIs

### <a name="how-do-i-configure-media-reserved-units"></a>Como posso configurar a unidades reservadas de multimédia?

Para a análise de áudio e tarefas de análise de vídeo que são acionados por serviços de multimédia v3 ou Video Indexer, recomenda-se elevada para aprovisionar a sua conta com 10 MRUs de S3. Se precisar de mais de 10 S3 MRUs, abra um pedido de suporte através do [portal do Azure](https://portal.azure.com/).

Para obter detalhes, consulte [Dimensionar processamento de multimédia com a CLI](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>O que é o método recomendado para vídeos do processo?

Recomenda-se que submeter tarefas através de um URL de HTTP (s) que aponta para o vídeo. Para obter mais informações, [HTTP (s) de ingestão](job-input-from-http-how-to.md). Não é necessário criar um elemento com o vídeo de entrada, antes que ele possa ser processado.

### <a name="how-does-pagination-work"></a>Como funciona a paginação?

Ao utilizar a paginação, deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular. Para obter detalhes e exemplos, consulte [filtragem, ordenação, paginação](entities-overview.md).

## <a name="live-streaming"></a>Transmissão em direto 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Como inserir quebras/vídeos e slates da imagem durante a transmissão em direto?

Codificação em direto do serviços de multimédia v3 ainda não suporta inserção de slates vídeo ou imagem durante a transmissão em direto. 

Pode utilizar um [locais codificador em direto](recommended-on-premises-live-encoders.md) para mudar o vídeo de origem. Muitas aplicações fornecem a capacidade de mudar de origens, incluindo Telestream Wirecast, comutador Studio (no iOS), OBS Studio (aplicação gratuita) e muito mais.

## <a name="media-services-v2-vs-v3"></a>Serviços de multimédia v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Pode utilizar o portal do Azure para gerir os recursos de v3?

Ainda não. Pode utilizar um dos SDKs suportados. Veja tutoriais e amostras neste conjunto de documentos.

### <a name="is-there-an-assetfile-concept-in-v3"></a>Existe um conceito de AssetFile na v3?

Os AssetFiles foram removidas da API do AMS para separar os serviços de multimédia da dependência do SDK de armazenamento. Agora o armazenamento, não os serviços de multimédia, mantém as informações que pertence no armazenamento. 

Para obter mais informações, consulte [migre para o serviços de multimédia v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Em que passei a encriptação do armazenamento do lado do cliente?

Recomenda-se agora para utilizar a encriptação de armazenamento do lado do servidor (o que está ativada por predefinição). Para obter mais informações, consulte [encriptação do serviço de armazenamento do Azure para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral do serviços de multimédia v3](media-services-overview.md)
