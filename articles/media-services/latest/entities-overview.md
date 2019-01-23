---
title: Serviços de multimédia entidades descrição geral do Azure - Azure | Documentos da Microsoft
description: Este artigo fornece uma descrição geral de entidades de serviços de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 902014ff3c242a18b3872ba490845eb0923f39a4
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451192"
---
# <a name="azure-media-services-entities-overview"></a>Descrição geral de entidades de serviços de multimédia do Azure

Este artigo fornece uma breve descrição geral das entidades de serviços de multimédia do Azure e dos pontos para um artigo para obter mais informações sobre cada entidade. 

| Tópico | Descrição |
|---|---|
| [Filtros de conta e filtros de elemento](filters-dynamic-manifest-overview.md)|Quando a entrega de conteúdo aos clientes (transmissão em fluxo eventos em direto ou de vídeo a pedido) o cliente poderá ter mais flexibilidade do que o que é descrito no arquivo de manifesto do recurso padrão. Serviços de multimédia do Azure permite-lhe definir [filtros de conta](https://docs.microsoft.com/rest/api/media/accountfilters) e [Asset filtros](https://docs.microsoft.com/rest/api/media/assetfilters). Em seguida, utilize **manifestos dinâmica** com base nos filtros predefinidos. |
| [Ativos](assets-concept.md)|Uma [Asset](https://docs.microsoft.com/rest/api/media/assets) entidade contém ficheiros digitais (incluindo o vídeo, áudio, imagens, coleções de miniaturas, pistas de texto e ficheiros de legendas) e os metadados relativos a esses ficheiros. Depois dos ficheiros digitais são carregados para um elemento, eles podem ser usados nos serviços de multimédia de codificação, transmissão em fluxo, analisar fluxos de trabalho de conteúdo.|
| [Diretivas de chave de conteúdo](content-key-policy-concept.md)|Pode utilizar os serviços de multimédia para proteger os seus suportes de dados a partir do momento em que deixa seu computador por meio de armazenamento, processamento e entrega. Com os serviços de multimédia, pode fornecer seu conteúdo ao vivo e sob demanda dinamicamente encriptado com o Advanced Encryption Standard (AES-128) ou qualquer um dos três sistemas de gestão (DRM) de direitos digitais principais: Microsoft PlayReady, Widevine da Google e Apple FairPlay. Serviços de multimédia também fornecem um serviço para entrega de chaves AES e o DRM (PlayReady, Widevine e FairPlay) licenças para os clientes autorizados.|
| [LiveEvents e LiveOutputs](live-events-outputs-concept.md)|Serviços de multimédia permite-lhe fornecer eventos em direto aos seus clientes na cloud do Azure. Para configurar seus eventos de transmissão em fluxo em direto em serviços de multimédia v3, precisa saber mais sobre [eventos em direto](https://docs.microsoft.com/rest/api/media/liveevents) e [Live saídas](https://docs.microsoft.com/rest/api/media/liveoutputs).|
| [Pontos finais de transmissão em fluxo](streaming-endpoint-concept.md)|R [pontos finais de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streamingendpoints) entidade representa um serviço de transmissão em fluxo que pode entregar conteúdo diretamente a uma aplicação de leitor cliente ou para uma rede de entrega de conteúdos (CDN) para uma maior distribuição. O fluxo de saída de um serviço de ponto final de transmissão em fluxo pode ser uma transmissão em direto ou um elemento de vídeo a pedido na sua conta de Media Services. Quando cria uma conta de Media Services, um **predefinição** ponto final de transmissão em fluxo é criado por si no estado parado. Não é possível eliminar a **predefinição** ponto final de transmissão em fluxo. Pontos finais de transmissão em fluxo adicionais podem ser criados sob a conta. Para começar a transmissão em fluxo de vídeos, terá de iniciar o ponto de final de transmissão em fluxo do qual pretende transmitir o vídeo. |
| [Localizadores de transmissão em fluxo](streaming-locators-concept.md)|Tem de fornecer a seus clientes com um URL que eles podem usar para reproduzir codificado em ficheiros de vídeos ou áudio, terá de criar uma [localizador de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streaminglocators) e criar os URLs de transmissão em fluxo.|
| [Políticas de transmissão em fluxo](streaming-policy-concept.md)| [Políticas de transmissão em fluxo](https://docs.microsoft.com/rest/api/media/streamingpolicies) permitem-lhe definir opções de encriptação para sua StreamingLocators e protocolos de transmissão em fluxo. Pode especificar o nome de uma política personalizada de transmissão em fluxo que criou ou utilizar uma das políticas de transmissão em fluxo predefinido oferecidas pelos serviços de multimédia. <br/><br/>Quando utilizar uma política personalizada de transmissão em fluxo, deve criar um conjunto limitado dessas políticas para a sua conta de serviço de multimédia e reutilizá-los para os localizadores de transmissão em fluxo sempre que as mesmas opções de encriptação e os protocolos são necessários. Devem não ser a criar uma nova política de transmissão em fluxo para cada localizador de transmissão em fluxo.|
| [Transformações e tarefas](transforms-jobs-concept.md)|Uso [transforma](https://docs.microsoft.com/rest/api/media/transforms) para configurar as tarefas comuns de codificação ou analisar vídeos. Cada **transformar** descreve uma recipe ou um fluxo de trabalho de tarefas para processar os ficheiros de vídeos ou áudio.<br/><br/>R [tarefa](https://docs.microsoft.com/rest/api/media/jobs) é o pedido real para os serviços de multimédia do Azure para aplicar a **transformar** para um determinado conteúdo vídeo ou áudio de entrada. O **tarefa** Especifica informações como a localização do vídeo de entrada e a localização para a saída. Pode especificar a localização da sua utilização vídeo entrada: URLs HTTPS, URLs SAS ou um recurso.|

## <a name="next-steps"></a>Passos Seguintes

[Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
