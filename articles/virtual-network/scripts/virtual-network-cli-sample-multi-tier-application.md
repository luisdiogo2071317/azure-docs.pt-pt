---
title: Exemplo de script da CLI do Azure - Criar uma rede para aplicações de várias camadas | Microsoft Docs
description: Exemplo de script da CLI do Azure - Criar uma rede virtual para aplicações de várias camadas.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 2db43d842ec5656f6f059c240eb9b9cc2376459e
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697483"
---
# <a name="create-a-network-for-multi-tier-applications-script-sample"></a>Criar uma rede para o exemplo de script de aplicações de várias camadas

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. O tráfego para a sub-rede do front-end está limitado a HTTP e SSH, enquanto o tráfego para a sub-rede de back-end está limitado a MySQL, porta 3306. Depois de executar o script, tem duas máquinas virtuais, uma em cada sub-rede nas quais pode implementar software MySQL e o servidor Web.

Pode executar o script a partir do [Azure Cloud Shell](https://shell.azure.com/bash) ou a partir de uma instalação local da CLI do Azure. Se utilizar a CLI localmente, este script requer que esteja a executar a versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI, também terá de executar o `az login` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Script de exemplo


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e grupos de segurança de rede. Cada comando na tabela seguinte liga à documentação específica do comando:

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](/cli/azure/network/vnet) | Cria uma rede e sub-rede virtual de front-end do Azure. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Cria uma sub-rede de back-end. |
| [az network public-ip create](/cli/azure/network/public-ip) | Cria um endereço IP público para aceder à VM a partir da Internet. |
| [az network nic create](/cli/azure/network/nic) | Cria interfaces de rede virtual e anexa-as a sub-redes de front-end e back-end da rede virtual. |
| [az network nsg create](/cli/azure/network/nsg) | Cria grupos de segurança (NSG) que estão associados às sub-redes de front-end e back-end. |
| [az network nsg rule create](/cli/azure/network/nsg/rule) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [az vm create](/cli/azure/vm) | Cria máquinas virtuais e anexa um NIC para cada VM. Este comando também especifica a imagem da máquina virtual a utilizar e as credenciais administrativas. |
| [az group delete](/cli/azure/group) | Elimina um grupo de recursos e todos os recursos contidos no mesmo. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Pode ver exemplos adicionais de scripts da CLI de rede virtual em [Exemplos da CLI de rede virtual](../cli-samples.md).
