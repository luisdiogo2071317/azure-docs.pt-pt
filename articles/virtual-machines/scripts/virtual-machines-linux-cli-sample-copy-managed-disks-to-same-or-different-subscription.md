---
title: Exemplo do Script da CLI do Azure – Copiar (mover) discos geridos para uma subscrição idêntica ou diferente | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Copiar (mover) discos geridos para uma subscrição idêntica ou diferente
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: aea21d09aa37eb2f0902df6245e9baebfcbb37fd
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55698570"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Copiar discos geridos para uma subscrição idêntica ou diferente com a CLI

Este script copia um disco gerido para uma subscrição idêntica ou diferente na mesma região. A cópia funciona apenas quando as subscrições são parte do mesmo inquilino do AAD.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um novo disco gerido na subscrição de destino com o ID do disco gerido de origem. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk) | Obtém todas as propriedades de um disco gerido através do nome e das propriedades do grupo de recursos do disco gerido. A propriedade do ID é utilizada para copiar o disco gerido para uma subscrição diferente.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk) | Copia um disco gerido ao criar um novo disco gerido numa subscrição diferente com o ID e o nome do disco gerido principal.  |

## <a name="next-steps"></a>Passos Seguintes

[Criar uma máquina virtual a partir de um disco gerido](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI da máquina virtual e discos geridos adicionais na [Documentação da VM Linux do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
