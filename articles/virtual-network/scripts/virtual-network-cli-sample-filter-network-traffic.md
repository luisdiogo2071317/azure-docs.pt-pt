---
title: Exemplo de script CLI do Azure - tráfego de rede de VM de filtro | Microsoft Docs
description: Script CLI do Azure de exemplo - filtrar o tráfego de rede VM de entrada e saído.
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
ms.openlocfilehash: bfc894ea718205ac77be48552f6cb5a076572395
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrar o tráfego de rede VM de entrada e saído

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. Tráfego de rede de entrada para a sub-rede do front-end está limitado a HTTP, HTTPS e SSH, enquanto o tráfego de saída à internet da sub-rede de back-end não é permitido. Depois de executar o script, terá de uma máquina virtual com dois NICs. Cada NIC está ligado a outra sub-rede.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/bash), ou a partir de uma instalação local da CLI do Azure. Se utilizar a CLI localmente, este script requer que está a executar a versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar o CLI localmente, terá também de executar `az login` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, VM e todos os recursos relacionados:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e os grupos de segurança de rede. Cada comando nas seguintes ligações de tabela para a documentação específica do comando:

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Cria uma rede virtual do Azure e a sub-rede do front-end. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Cria uma sub-rede de back-end. |
| [atualização de sub-rede da vnet de rede de AZ](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) | Associa os NSGs a sub-redes. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Cria um endereço IP público para aceder a VM a partir da internet. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Cria interfaces de rede virtual e anexa-las a sub-redes de front-end e back-end à rede virtual. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Cria grupos de segurança (NSG) que estão associados às sub-redes de front-end e back-end de rede. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [az vm create](/cli/azure/vm#az_vm_create) | Cria máquinas virtuais e anexa um NIC para cada VM. Este comando também especifica a imagem de máquina virtual para utilizar e credenciais administrativas. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos e todos os recursos que nele contidos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Exemplos de script do CLI de rede virtual adicionais podem ser encontrados na [amostras CLI de rede Virtual](../cli-samples.md).
