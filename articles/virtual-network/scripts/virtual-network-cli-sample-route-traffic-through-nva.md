---
title: Exemplo de script CLI do Azure - encaminhar o tráfego através de uma aplicação virtual de rede | Microsoft Docs
description: Azure CLI script de exemplo - encaminhar o tráfego através de uma aplicação virtual de rede de firewall.
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
ms.openlocfilehash: 8506d67152a21586b2854891674d50f89d1a643c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Encaminhar o tráfego através de uma aplicação virtual de rede

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. Também cria uma VM com o reencaminhamento IP está ativado para encaminhar o tráfego entre as duas sub-redes. Depois de executar o script pode implementar o software de rede, como uma aplicação de firewall, para a VM.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/bash), ou a partir de uma instalação local da CLI do Azure. Se utilizar a CLI localmente, este script requer que está a executar a versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar o CLI localmente, terá também de executar `az login` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

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
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Cria o back-end e sub-redes da rede de Perímetro. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Cria um endereço IP público para aceder a VM a partir da internet. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Cria uma interface de rede virtual e ativar o reencaminhamento de IP para o mesmo. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Cria um grupo de segurança de rede (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Cria regras do NSG que permitem que as portas HTTP e HTTPS de entrada para a VM. |
| [atualização de sub-rede da vnet de rede de AZ](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| Associa os NSGs e tabelas de rotas para sub-redes. |
| [az network route-table create](/cli/azure/network/route-table#az_network_route_table_create)| Cria uma tabela de rota para todas as rotas. |
| [Criar rota de tabela de rotas de rede AZ](/cli/azure/network/route-table/route#az_network_route_table_route_create)| Cria rotas para encaminhar o tráfego entre sub-redes e a internet através da VM. |
| [az vm create](/cli/azure/vm#az_vm_create) | Cria uma máquina virtual e anexa a NIC ao mesmo. Este comando também especifica a imagem de máquina virtual para utilizar e credenciais administrativas. |
| [az group delete](/cli/azure/group#az_group_delete) | Elimina um grupo de recursos e todos os recursos que nele contidos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Exemplos de script do CLI de rede virtual adicionais podem ser encontrados na [amostras CLI de rede Virtual](../cli-samples.md).
