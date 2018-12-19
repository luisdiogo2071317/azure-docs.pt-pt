---
title: Exemplo do Script da CLI do Azure – Copiar (mover) o instantâneo de um disco gerido para uma subscrição idêntica ou diferente com a CLI | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Copiar (mover) o instantâneo de um disco gerido para uma subscrição idêntica ou diferente com a CLI
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
ms.openlocfilehash: c8c9f2ab2bed2e05f540422e47fdc634cb011187
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583277"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Copiar o instantâneo de um disco gerido para uma subscrição idêntica ou diferente com a CLI

Este script copia um instantâneo de um disco gerido para uma subscrição idêntica ou diferente. Utilize este script para mover um instantâneo para uma subscrição diferente na mesma região do instantâneo principal.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um instantâneo na subscrição de destino com o ID do instantâneo de origem. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Obtém todas as propriedades de um instantâneo através do nome e das propriedades do grupo de recursos do instantâneo. A propriedade do ID é utilizada para copiar o instantâneo para uma subscrição diferente.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_create) | Copia um instantâneo através da criação de um instantâneo numa subscrição diferente com o ID e o nome do instantâneo principal.  |

## <a name="next-steps"></a>Passos Seguintes

[Criar uma máquina virtual a partir de um instantâneo](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI da máquina virtual e discos geridos adicionais na [Documentação da VM Linux do Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
