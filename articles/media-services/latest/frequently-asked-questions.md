---
title: Azure Media Services v3 perguntas mais frequentes | Microsoft Docs
description: Este artigo fornece respostas às perguntas mais frequentes de v3 de Media Services do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/29/2018
ms.author: juliako
ms.openlocfilehash: 098a34aba8e5ce23f64d4bb07e3b9622aa2adb8e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110425"
---
# <a name="azure-media-services-v3-preview-frequently-asked-questions"></a>V3 de Media Services do Azure (pré-visualização) perguntas mais frequentes

Este artigo fornece respostas às perguntas mais frequentes de v3 de serviços de suporte de dados do Azure (AMS).

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Pode utilizar o portal do Azure para gerir os recursos de v3?

Ainda não. Pode utilizar uma dos SDKs suportados. Consulte os tutoriais e exemplos deste conjunto de documentação.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Existe uma API para configurar unidades reservadas de multimédia?

A equipa de serviços de suporte de dados é eliminando RUs na v3. No entanto o trabalho de serviço necessárias não está concluído. Até lá, os clientes têm de utilizar o portal do Azure ou APIs do AMS v2 para definir RUs (conforme descrito em [dimensionamento de processamento de suporte de dados](../previous/media-services-scale-media-processing-overview.md). 

Quando utilizar **VideoAnalyzerPreset** e/ou **AudioAnalyzerPreset**, defina a sua conta de Media Services para 10 unidades reservadas de multimédia S3.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>A V3 Asset tem nenhum conceito AssetFile?

Os AssetFiles foram removidos da API do AMS para separar os serviços de suporte de dados de dependência do SDK de armazenamento. Agora o armazenamento, não os Media Services, mantém as informações que pertencem no armazenamento. 

## <a name="where-did-client-side-storage-encryption-go"></a>Em que aceda a encriptação do lado do cliente armazenamento?

Recomendamos agora a encriptação de armazenamento do lado do servidor (o que está ativada por predefinição). Para obter mais informações, consulte [encriptação do serviço de armazenamento do Azure para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>O que é o método de carregamento recomendada?

Recomendamos que a utilização de HTTP (s) ingere. Para obter mais informações, consulte [HTTP (s) de inserção](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Como funciona a paginação?

Os Media Services suportam $top para recursos que suportam OData, mas o valor transmitido ao $top tem de ser inferior a 1000 (por exemplo, o tamanho da página para paginação).

Isto permite-lhe optar por obter um exemplo pequeno de itens utilizando $top (por exemplo, os itens mais recentes 100) ou para a página apesar de todos os itens com a paginação. 

Os Media Services não suporta paginação através de dados com um utilizador especificado tamanho da página.

Para obter mais informações, consulte [filtragem, ordenação, paginação](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Como obter uma entidade em Media Services v3?

v3 baseia-se numa superfície de API unificada que expõe uma funcionalidade de gestão e operações incorporada no **do Azure Resource Manager**. Em conformidade com **do Azure Resource Manager**, os nomes de recursos são sempre de ser exclusivos. Desta forma, pode utilizar todas as cadeias de identificador exclusivo (por exemplo, GUIDs) para os nomes de recursos. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral de v3 de Media Services](media-services-overview.md)
