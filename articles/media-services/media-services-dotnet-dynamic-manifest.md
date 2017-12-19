---
title: "Criar Filtros com o SDK .NET dos Serviços de Multimédia do Azure"
description: "Este tópico descreve como criar filtros para que o cliente possa utilizar a secções específicas do fluxo de um fluxo. Os Media Services cria manifestos dinâmicos para alcançar esta seletiva de transmissão em fluxo."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 8ffd310573d0800593bd9d93d74da4bcece61fa4
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Criar Filtros com o SDK .NET dos Serviços de Multimédia do Azure
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

A partir da versão 2.17, os Media Services permite-lhe definir os filtros para os elementos. Estes filtros são regras de lado do servidor que permitem que os seus clientes de optar por efetuar ações como: reprodução apenas uma secção de um vídeo (em vez de reproduzir o vídeo todo), ou especifique apenas um subconjunto de áudio e vídeos renditions que processa (em vez do dispositivo do cliente todos os os renditions que estão associados com o elemento). Esta filtragem dos seus ativos é conseguido através de **manifesto dinâmica**s que são criados após o pedido do cliente para um vídeo de fluxo com base em filtros especificados.

Para obter mais informações relacionadas com filtros e manifesto dinâmico, consulte [dinâmico manifestos descrição geral](media-services-dynamic-manifest-overview.md).

Este artigo mostra como utilizar o SDK .NET dos Media Services para criar, atualizar e eliminar os filtros. 

Tenha em atenção de que o se atualizar um filtro, pode demorar até dois minutos até que ponto final para atualizar as regras de transmissão em fluxo. Se o conteúdo tiver sido servido com este filtro (e colocados em cache no proxies e CDN caches), ao atualizar este filtro pode resultar em falhas de leitor. Sempre limpe a cache depois de atualizar o filtro. Se esta opção não for possível, considere utilizar um filtro diferente. 

## <a name="types-used-to-create-filters"></a>Tipos de utilizados para criar filtros
Quando criar os filtros, são utilizados os seguintes tipos: 

* **IStreamingFilter**.  Este tipo baseia-se na API REST seguintes [filtro](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Este tipo baseia-se na API REST seguintes [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Este tipo baseia-se na API REST seguintes [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** e **IFilterTrackPropertyCondition**. Estes tipos são baseados nas APIs REST seguintes [FilterTrackSelect e FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Criar/atualizar/leitura/eliminar filtros de global
O código seguinte mostra como utilizar .NET para criar, atualizar, ler e eliminar os filtros de recurso.

    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


## <a name="createupdatereaddelete-asset-filters"></a>Filtros de recurso criar/atualizar/leitura/eliminar
O código seguinte mostra como utilizar .NET para criar, atualizar, ler e eliminar os filtros de recurso.

    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();




## <a name="build-streaming-urls-that-use-filters"></a>Criar URLs que utilizam filtros de transmissão em fluxo
Para obter informações sobre como publicar e fornecer os recursos, consulte [entrega de conteúdos para clientes descrição-geral](media-services-deliver-content-overview.md).

Os exemplos seguintes mostram como adicionar filtros para os URLs de transmissão em fluxo.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live transmissão em fluxo (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live transmissão em fluxo (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Transmissão em fluxo uniforme**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Veja Também
[Descrição geral de manifestos dinâmica](media-services-dynamic-manifest-overview.md)

