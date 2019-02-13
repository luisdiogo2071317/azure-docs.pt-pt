---
title: Contas de armazenamento nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este artigo discute como os serviços de multimédia utiliza contas de armazenamento.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 90e01f39fa6b31095d76d0dfae2f700b4fa2ca3f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182872"
---
# <a name="storage-accounts"></a>Contas de armazenamento

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundárias** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas para **Fins gerais v2** (GPv2) ou **Fins gerais v1** (GPv1). 

>[!NOTE]
> As contas apenas de blobs não são permitidas como **Principais**. 

Recomendamos que utilize GPv2, para que possa tirar partido de optar entre frequente e esporádico camadas de armazenamento. Para saber mais sobre as contas de armazenamento, veja [descrição geral da conta de armazenamento do Azure](../../storage/common/storage-account-overview.md). 

A conta de Media Services e todas as contas de armazenamento associado tem de ser na mesma subscrição do Azure. É vivamente recomendado para utilizar contas de armazenamento na mesma localização que a conta de serviços de multimédia para evitar custos de saída de latência e dados adicionais

## <a name="assets-in-a-storage-account"></a>Recursos numa conta de armazenamento

Em serviços de multimédia v3, as APIs de armazenamento são utilizadas para carregar ficheiros. Para ver como utilizar as APIs de armazenamento com os serviços de multimédia para carregar os ficheiros de entrada, confira [criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md). 

> [!Note]
> Não deve tentar alterar os conteúdos de contentores de BLOBs que foram gerados pelo SDK de serviços de multimédia sem utilizar as APIs dos serviços de multimédia.

## <a name="next-steps"></a>Passos Seguintes

Para saber como anexar uma conta de armazenamento à sua conta de Media Services, veja [criar uma conta](create-account-cli-quickstart.md).
