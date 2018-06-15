---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 3009948c72b0cad6ba603a3ccbe35db8d93fe2c7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664735"
---
## <a name="storage"></a>Armazenamento

|  |  |
|---------|---------|
| [SKUs Permitidos para Contas de Armazenamento e Máquinas Virtuais](../articles/azure-policy/scripts/allowed-skus-storage.md) | Requer que as contas de armazenamento e as máquinas virtuais utilizem SKUs aprovados. Utiliza políticas incorporadas para garantir SKUs aprovados. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de contas de armazenamento aprovadas. |
| [SKUs de contas de armazenamento permitidos](../articles/azure-policy/scripts/allowed-stor-acct-skus.md) | Requer que as contas de armazenamento utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Negar camadas de acesso esporádico para contas de armazenamento](../articles/azure-policy/scripts/deny-cool-access-tiering.md) | Proíbe a utilização de camadas de acesso esporádico para contas de armazenamento de blobs.  |
| [Assegurar o tráfego https apenas para contas de armazenamento](../articles/azure-policy/scripts/ensure-https-stor-acct.md) | Requer que as contas de armazenamento utilizem o tráfego HTTPS.  |
| [Assegurar a encriptação de ficheiros de armazenamento](../articles/azure-policy/scripts/ensure-store-file-enc.md) | Requer que a encriptação de ficheiros esteja ativada para contas de armazenamento.  |
| [Requerer a encriptação de contas de armazenamento](../articles/azure-policy/scripts/req-store-acct-enc.md) | Requer que a conta de armazenamento utilize a encriptação de blobs.  |