---
title: As contas de armazenamento nos serviços de suporte de dados do Azure | Microsoft Docs
description: Este artigo descreve como os Media Services utilizam contas de armazenamento.
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
ms.openlocfilehash: 6d4c21867b0b46508f348300ae2b9553a75d23b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="storage-accounts"></a>Contas de armazenamento

Quando criar uma conta de Media Services, terá de fornecer o nome de um recurso de conta do Storage do Azure. A conta de armazenamento especificado está ligada à sua conta de Media Services. 

Tem de ter um **primário** conta de armazenamento e pode ter qualquer número de **secundário** contas de armazenamento associadas à conta dos Media Services. Os Media Services suportam **v2 para fins gerais** (GPv2) ou **v1 para fins gerais** contas (GPv1). 

>[!NOTE]
> As contas do blob únicas não são permitidas como **primário**. 

Recomendamos que utilize GPv2, para que possa tirar partido de escolher entre hot e esporádico camadas de armazenamento. Para saber mais sobre contas de armazenamento, consulte o artigo [opções de conta do Storage do Azure](../../storage/common/storage-account-options.md). 

## <a name="assets-in-a-storage-account"></a>Recursos numa conta do storage

Os Media Services v3, as APIs de armazenamento são utilizadas para carregar ficheiros. Para ver como utilizar os APIs de armazenamento com os Media Services para carregar os ficheiros de entrada, consulte [criar uma entrada de tarefa a partir de um ficheiro local](job-input-from-local-file-how-to.md). 

> [!Note]
> Não deve tentar alterar os conteúdos de contentores do blob que foram gerados pelo SDK de Media Services sem utilizar as APIs de serviços de suporte de dados.

## <a name="next-steps"></a>Passos Seguintes

Para saber como ligar uma conta de armazenamento à conta de Media Services, consulte [criar uma conta](create-account-cli-quickstart.md).
