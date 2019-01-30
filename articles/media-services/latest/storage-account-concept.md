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
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 4668ff23710121e5495395d71902b9ffa4c01238
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220353"
---
# <a name="storage-accounts"></a>Contas de armazenamento

Quando criar uma conta dos Serviços de Multimédia, terá de fornecer o nome de um recurso de conta de Armazenamento do Azure. A conta de armazenamento especificada está ligada à sua conta dos Serviços de Multimédia. 

Tem de ter uma conta de armazenamento **Principal** e pode ter qualquer número de contas de armazenamento **Secundárias** associadas à conta dos Serviços de Multimédia. Os Serviços de Multimédia suportam contas para **Fins gerais v2** (GPv2) ou **Fins gerais v1** (GPv1). 

>[!NOTE]
> As contas apenas de blobs não são permitidas como **Principais**. 

Recomendamos que utilize GPv2, para que possa tirar partido de optar entre frequente e esporádico camadas de armazenamento. Para saber mais sobre as contas de armazenamento, veja [descrição geral da conta de armazenamento do Azure](../../storage/common/storage-account-overview.md). 

A conta de Media Services e todas as contas de armazenamento associado tem de ser na mesma subscrição do Azure. Recomenda-se para utilizar contas de armazenamento na mesma localização que a conta de Media Services.

## <a name="assets-in-a-storage-account"></a>Recursos numa conta de armazenamento

Em serviços de multimédia v3, as APIs de armazenamento são utilizadas para carregar ficheiros. Para ver como utilizar as APIs de armazenamento com os serviços de multimédia para carregar os ficheiros de entrada, confira [criar uma entrada da tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md). 

> [!Note]
> Não deve tentar alterar os conteúdos de contentores de BLOBs que foram gerados pelo SDK de serviços de multimédia sem utilizar as APIs dos serviços de multimédia.

## <a name="next-steps"></a>Passos Seguintes

Para saber como anexar uma conta de armazenamento à sua conta de Media Services, veja [criar uma conta](create-account-cli-quickstart.md).
