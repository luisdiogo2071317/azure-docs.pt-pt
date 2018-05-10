---
title: Notas de versão de v3 dos Media Services do Azure | Microsoft Docs
description: Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece as atualizações mais recentes no v3 de Media Services do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: fc6c5ba6cd97c261dd44eade33bf21e8d1b74bf0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure Media Services v3 notas de versão (pré-visualização) 

Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

* As versões mais recentes
* Problemas conhecidos
* Correções de erros
* Funcionalidade preterida
* Planos de alterações

## <a name="may-07-2018"></a>07 de Maio de 2018

### <a name="net-sdk"></a>SDK do .net

As seguintes funcionalidades estão presentes no .net SDK:

1. **Transforma** e **tarefas** para codificar ou analisar o conteúdo de multimédia. Para obter exemplos, consulte [transmitir ficheiros](stream-files-tutorial-with-api.md) e [analisar](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** publicação e conteúdo para os dispositivos de utilizador final de transmissão em fluxo
3. **StreamingPolicies** e **ContentKeyPolicies** para configurar a entrega de chave e a proteção de conteúdos (DRM) quando distribui conteúdo.
4. **LiveEvents** e **LiveOutputs** para configurar a inserção e arquivo de conteúdos de transmissão em fluxo em direto.
5. **Ativos** para armazenar e publicar conteúdo do suporte de dados no armazenamento do Azure. 
6. **Lidam** para configurar e dimensionar o empacotamento dinâmico, encriptação e de transmissão em fluxo em direto e a pedido suportes de dados de conteúdo.

### <a name="known-issues"></a>Problemas conhecidos

Problema conhecido:

Quando submete uma tarefa com um URL HTTPS (JobInputHttp) que aponta para o conteúdo de origem, certifique-se de que o servidor de HTTP suporta o pedido de 'HEAD'. Caso contrário, a tarefa será rejeitada.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Descrição geral](media-services-overview.md)
