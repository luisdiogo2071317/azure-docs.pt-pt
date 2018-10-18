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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: dccbc6e57e970ec7089f81fccb33b741b9c00e74
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376725"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Serviços de multimédia do Azure v3 perguntas mais frequentes

Este artigo responde às perguntas mais frequentes de v3 de serviços de multimédia do Azure (AMS).

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Pode utilizar o portal do Azure para gerir os recursos de v3?

Ainda não. Pode utilizar um dos SDKs suportados. Veja tutoriais e amostras neste conjunto de documentos.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Existe uma API para a configuração de unidades reservadas de multimédia?

A equipe de serviços de multimédia é eliminando o RUs na v3. No entanto o trabalho de serviço necessário não está concluído. Até lá, os clientes têm de utilizar o portal do Azure ou APIs do AMS v2 para definir o RUs (conforme descrito em [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

Ao usar **VideoAnalyzerPreset** e/ou **AudioAnalyzerPreset**, defina a sua conta de serviços de multimédia para 10 unidades reservadas de multimédia S3.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>Não o recurso de V3 tem nenhum conceito de AssetFile?

Os AssetFiles foram removidas da API do AMS para separar os serviços de multimédia da dependência do SDK de armazenamento. Agora o armazenamento, não os serviços de multimédia, mantém as informações que pertence no armazenamento. 

## <a name="where-did-client-side-storage-encryption-go"></a>Em que passei a encriptação do armazenamento do lado do cliente?

Recomendamos que agora, a encriptação de armazenamento do lado do servidor (o que está ativada por predefinição). Para obter mais informações, consulte [encriptação do serviço de armazenamento do Azure para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>O que é o método de carregamento recomendada?

Recomendamos que a utilização de HTTP (s) ingere. Para obter mais informações, consulte [HTTP (s) de ingestão](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Como funciona a paginação?

Serviços de multimédia suportam $top para recursos que dão suporte a OData, mas o valor transmitido para $top tem de ser inferior a 1000 (por exemplo, o tamanho da página para a paginação).

Isto permite-lhe optar por colocar uma pequena amostra dos itens com $top (por exemplo, os itens mais recentes 100) ou página, no entanto, todos os itens com a paginação. 

Serviços de multimédia não suporta paginação por meio dos dados com um utilizador especificado tamanho da página.

Para obter mais informações, consulte [filtragem, ordenação, paginação](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Como recuperar uma entidade em serviços de multimédia v3?

v3 baseia-se numa superfície de API unificada, que expõe a funcionalidade de gerenciamento e operações incorporada **do Azure Resource Manager**. De acordo com **do Azure Resource Manager**, os nomes de recursos são sempre de ser exclusivos. Desta forma, pode utilizar todas as cadeias de identificador exclusivo (por exemplo, GUIDs) para os nomes de recursos. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral do serviços de multimédia v3](media-services-overview.md)
