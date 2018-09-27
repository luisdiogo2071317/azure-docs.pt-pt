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
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: ed2550c1df4645933fb968c54ee536995c810136
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219334"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Serviços de multimédia do Azure v3 notas de versão (pré-visualização) 

Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

* Versões mais recentes
* Problemas conhecidos
* Correções de erros
* Funcionalidades preteridas
* Planos para que as alterações

## <a name="may-07-2018"></a>07 de Maio de 2018

### <a name="net-sdk"></a>.NET SDK

As seguintes funcionalidades estão presentes no .net SDK:

1. **Transforma** e **tarefas** codificar ou analisar o conteúdo de mídia. Para obter exemplos, consulte [Stream arquivos](stream-files-tutorial-with-api.md) e [Analyze](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** para publicar e conteúdo para os dispositivos de utilizador final de transmissão em fluxo
3. **StreamingPolicies** e **ContentKeyPolicies** para configurar a entrega de chave e a proteção de conteúdo (DRM) quando a entrega de conteúdos.
4. **LiveEvents** e **LiveOutputs** para configurar a ingestão e o arquivamento de conteúdo de transmissão em fluxo em direto.
5. **Ativos** para armazenar e publicar conteúdo de mídia no armazenamento do Azure. 
6. **Pontos finais** a configurar e dimensionar o empacotamento dinâmico, encriptação e transmissão em fluxo em direto e a pedido conteúdos de multimédia.

### <a name="known-issues"></a>Problemas conhecidos

* Quando submeter uma tarefa, pode especificar para ingerir o vídeo de origem utilizar HTTPS URLs, SAS URLs ou caminhos para ficheiros localizados no armazenamento de Blobs do Azure. Atualmente, o AMS v3 não suporta a codificação através de HTTPS URLs da transferência.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral](media-services-overview.md)
