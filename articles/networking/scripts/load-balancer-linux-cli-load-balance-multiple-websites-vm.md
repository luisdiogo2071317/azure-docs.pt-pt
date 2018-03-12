---
title: "Exemplo de Script do CLI do Azure - balanceamento de carga de vários sites com a CLI do Azure | Microsoft Docs"
description: "Exemplo de Script do CLI do Azure - balanceamento de carga de vários sites na mesma máquina virtual"
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 72a780f1870d05a714aaeec879004b6f06d8bb7f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="load-balance-multiple-websites"></a>Balanceamento de carga de vários sites

Este script de exemplo cria uma rede virtual com duas máquinas virtuais (VM) que são membros de um conjunto de disponibilidade. Um balanceador de carga direciona o tráfego de dois endereços IP separados para as duas VMs. Depois de executar o script, pode implementar o software de servidor web para as VMs e anfitrião vários web sites, cada um com o seu próprio endereço IP.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, rede virtual, o Balanceador de carga e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_network_vnet_create) | Cria uma rede virtual do Azure e a sub-rede. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_create) | Cria um endereço IP público com um endereço IP estático e um nome DNS associado. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_create) | Cria um balanceador de carga do Azure. |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe#az_network_lb_probe_create) | Cria uma sonda do Balanceador de carga. Uma sonda do Balanceador de carga é utilizada para monitorizar cada VM no conjunto de Balanceador de carga. Se qualquer VM torna-se inacessível, o tráfego não é encaminhado para a VM. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Cria uma regra de Balanceador de carga. Neste exemplo, é criada uma regra para a porta 80. Como o tráfego HTTP chega ao balanceador de carga, é encaminhado para a porta 80 uma das VMs no conjunto de Balanceador de carga. |
| [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#az_network_lb_frontend_ip_create) | Crie um endereço IP de front-end de Balanceador de carga. |
| [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool#az_network_lb_address_pool_create) | Cria um conjunto de endereços de back-end. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic#az_network_nic_create) | Cria uma placa de rede virtual e anexa-lo para a rede virtual e a sub-rede. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Cria um conjunto de disponibilidade. Conjuntos de disponibilidade garantir a disponibilidade de aplicações, propagando-se as máquinas virtuais em recursos físicos, de modo a que o se ocorrer uma falha, o conjunto completo não é afetado. |
| [az network nic ip-config create](https://docs.microsoft.com/cli/azure/network/nic/ip-config#az_network_nic_ip_config_create) | Cria um confiuration IP. Tem de ter a funcionalidade de Microsoft.Network/AllowMultipleIpConfigurationsPerNic ativada para a sua subscrição. Apenas uma configuração pode ser designada como a configuração de IP primária por NIC, utilizando o – sinalizador disponibilizar principal. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Cria a máquina virtual e liga-o para a placa de rede, a rede virtual, a sub-rede e o NSG. Este comando também especifica a imagem de máquina virtual a ser credenciais administrativas e utilizadas.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Exemplos de script CLI redes adicionais podem ser encontrados no [documentação de descrição geral de funcionamento em rede do Azure](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
