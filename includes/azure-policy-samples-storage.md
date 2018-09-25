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
ms.openlocfilehash: b967558828c42331b3702fe90ce842fd1c0032bd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003955"
---
## <a name="storage"></a>Armazenamento

|  |  |
|---------|---------|
| [SKUs Permitidos para Contas de Armazenamento e Máquinas Virtuais](../articles/governance/policy/samples/allowed-skus-storage.md) | Requer que as contas de armazenamento e as máquinas virtuais utilizem SKUs aprovados. Utiliza políticas incorporadas para garantir SKUs aprovados. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de contas de armazenamento aprovadas. |
| [SKUs de contas de armazenamento permitidos](../articles/governance/policy/samples/allowed-stor-acct-skus.md) | Requer que as contas de armazenamento utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Negar camadas de acesso esporádico para contas de armazenamento](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Proíbe a utilização de camadas de acesso esporádico para contas de armazenamento de blobs.  |
| [Assegurar o tráfego https apenas para contas de armazenamento](../articles/governance/policy/samples/ensure-https-stor-acct.md) | Requer que as contas de armazenamento utilizem o tráfego HTTPS.  |
| [Assegurar a encriptação de ficheiros de armazenamento](../articles/governance/policy/samples/ensure-store-file-enc.md) | Requer que a encriptação de ficheiros esteja ativada para contas de armazenamento.  |
| [Requerer a encriptação de contas de armazenamento](../articles/governance/policy/samples/req-store-acct-enc.md) | Requer que a conta de armazenamento utilize a encriptação de blobs.  |