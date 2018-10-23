---
title: Notas de versão da v3 de serviços de multimédia do Azure | Documentos da Microsoft
description: Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece as atualizações mais recentes nos serviços de multimédia do Azure v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: 10bd2101839e1b6d1dbdc1e53c31693ab0be98fd
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647770"
---
# <a name="azure-media-services-v3-release-notes"></a>Notas de versão da v3 de serviços de multimédia do Azure 

Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

* Versões mais recentes
* Problemas conhecidos
* Correções de erros
* Funcionalidades preteridas
* Planos para que as alterações

## <a name="october-2018---ga"></a>Outubro de 2018 - GA

Esta secção descreve as atualizações de Outubro de serviços (AMS) de multimédia do Azure.

### <a name="rest-v3-ga-release"></a>Versão de disponibilidade geral do REST v3

O [versão de disponibilidade geral do REST v3](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) inclui mais APIs do Live, filtros de manifestos de nível de conta/ativo e o suporte DRM.

#### <a name="azure-cli-20"></a>CLI 2.0 do Azure
 
O módulo de CLI 2.0 do Azure inclui atualmente operações para políticas de transmissão em fluxo em direto, diretivas de chave de conteúdo. Para obter mais informações, consulte [planos para alterações](#plans-for-changes).

#### <a name="azure-resource-management"></a>Gestão de Recursos do Azure 

Suporte do Azure Resource Management permite gerenciamento unificado e operações de API (agora tudo num único local).

Começando com esta versão, pode utilizar modelos do Resource Manager para criar eventos em direto.

#### <a name="improvement-of-asset-operations"></a>Melhoria das operações de recurso 

Foram introduzidas as seguintes melhorias:

- Ingestão de URLs de HTTP (s) ou URLs SAS do armazenamento de Blobs do Azure.
- Especifique a os nomes dos contentores próprios para ativos. 
- Suporte de saída mais fácil para criar fluxos de trabalho personalizados com as funções do Azure.

#### <a name="new-transform-object"></a>Novo objeto de transformação

A nova **transformar** objeto simplifica o modelo de codificação. O novo objeto torna mais fácil criar e partilhar modelos de codificação do Resource Manager e suas configurações predefinidas. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Autenticação do Active Directory e RBAC do Azure

Autenticação do Azure AD e o controlo de acesso baseado em funções (RBAC) permitem seguro transformações, LiveEvents, diretivas de chave de conteúdo ou recursos por função ou utilizadores no Azure AD.

#### <a name="client-sdks"></a>SDKs do Cliente  

Idiomas com suporte no Serviços de multimédia v3: .NET Core, Java, node. js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Atualizações de codificação do Live

Foram introduzidas as seguintes atualizações de codificação em direto:

- Novo modo de baixa latência para live (10 segundos ponto-a-ponto).
- Suporte RTMP aprimorado (maior estabilidade e mais suporte do codificador de origem).
- Ingerir RTMPS seguro.

    Quando cria um LiveEvent, agora obter 4 URLs de inserção. O 4 ingerir URLs são quase idênticos, ter o mesmo token de transmissão em fluxo (AppId), apenas a parte do número de porta é diferente. Dois dos URLs são principais e cópia de segurança para RTMPS. 
- suporte de transcodificação de 24 horas. 
- Suporte aprimorado a sinalização do ad no RTMP via SCTE35.

#### <a name="improved-event-grid-support"></a>Suporte aprimorado do Event Grid

Pode ver o Event Grid seguintes melhorias de suporte:

- Integração de EventGrid do Azure para o desenvolvimento mais fácil com o Logic Apps e as funções do Azure. 
- Inscreva-se para eventos no Encoding, canais em direto e muito mais.

### <a name="cmaf-support"></a>Suporte CMAF

Suporte de encriptação CMAF e 'cbcs' para (iOS 11 +) do Apple HLS e MPEG-DASH jogadores que suportam CMAF.

### <a name="video-indexer"></a>Indexador de Vídeos

Versão de disponibilidade geral do indexador vídeo foi anunciado em Agosto. Para obter novas informações sobre as funcionalidades atualmente suportadas, consulte [o que é o Video Indexer](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Planos para que as alterações

A versão da CLI do Azure estará disponível em 11/06/2018.

### <a name="known-issues"></a>Problemas conhecidos

Apenas os clientes que utilizada a API de pré-visualização para ativo ou AccountFilters são afetados pelo problema seguinte.

Se criou o ativos ou filtros de conta entre 09/28 e 10/12 com serviços de multimédia v3 CLI ou APIs, terá de remover todos os ativos e AccountFilters e voltar a criá-los devido a um conflito de versão. 

## <a name="may-2018---preview"></a>Maio de 2018 - pré-visualização

### <a name="net-sdk"></a>.NET SDK

As seguintes funcionalidades estão presentes no .net SDK:

* **Transforma** e **tarefas** codificar ou analisar o conteúdo de mídia. Para obter exemplos, consulte [Stream arquivos](stream-files-tutorial-with-api.md) e [Analyze](analyze-videos-tutorial-with-api.md).
* **StreamingLocators** para publicar e conteúdo para os dispositivos de utilizador final de transmissão em fluxo
* **StreamingPolicies** e **ContentKeyPolicies** para configurar a entrega de chave e a proteção de conteúdo (DRM) quando a entrega de conteúdos.
* **LiveEvents** e **LiveOutputs** para configurar a ingestão e o arquivamento de conteúdo de transmissão em fluxo em direto.
* **Ativos** para armazenar e publicar conteúdo de mídia no armazenamento do Azure. 
* **Pontos finais** a configurar e dimensionar o empacotamento dinâmico, encriptação e transmissão em fluxo em direto e a pedido conteúdos de multimédia.

### <a name="known-issues"></a>Problemas conhecidos

* Quando submeter uma tarefa, pode especificar para ingerir o vídeo de origem utilizar HTTPS URLs, SAS URLs ou caminhos para ficheiros localizados no armazenamento de Blobs do Azure. Atualmente, o AMS v3 não suporta a codificação de transferência segmentada através de URLs HTTPS.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral](media-services-overview.md)
