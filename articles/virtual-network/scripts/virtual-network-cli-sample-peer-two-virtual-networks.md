---
title: Exemplo de script CLI do Azure - redes virtuais ponto a ponto dois | Microsoft Docs
description: Azure CLI script de exemplo - redes virtuais ponto a ponto dois.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 244d7f6ff64643386c417d708f7fb1e9bbc34209
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Elemento duas redes virtuais

Este script cria e liga duas redes virtuais na mesma região através da rede do Azure. Depois de executar o script, tem um peering entre duas redes virtuais.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/bash), ou a partir de uma instalação local da CLI do Azure. Se utilizar a CLI localmente, este script requer que está a executar a versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar o CLI localmente, terá também de executar `az login` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, VM e todos os recursos relacionados:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando nas seguintes ligações de tabela para a documentação específica do comando:

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Cria uma rede e sub-rede virtual do Azure. |
| [Criar AZ rede vnet peering](/cli/azure/network/vnet/peering#az_network_vnet_peering_create) | Cria um peering entre duas redes virtuais.  |
| [az group delete](/cli/azure/vm/extension#az_vm_extension_set) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Exemplos de script do CLI de rede virtual adicionais podem ser encontrados na [amostras CLI de rede Virtual](../cli-samples.md).
