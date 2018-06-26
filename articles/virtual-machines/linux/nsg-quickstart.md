---
title: Abrir portas para uma VM com Linux com o Azure CLI 2.0 | Microsoft Docs
description: Saiba como abrir uma porta / criar um ponto final para a VM com Linux utilizando o modelo de implementação do Gestor de recursos do Azure e o 2.0 CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: 77e4744d43aa4e71e6cdbbe2364149465a5aeae1
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936620"
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Abrir portas e os pontos finais para uma VM com Linux com a CLI do Azure
Abrir uma porta ou criar um ponto final, para uma máquina virtual (VM) no Azure através da criação de um filtro de rede numa sub-rede ou interface de rede VM. Colocar estes filtros que controlam o tráfego de entrada e saído, num grupo de segurança de rede ligado para o recurso que recebe o tráfego. Vamos utilizar um exemplo comum de tráfego da web na porta 80. Este artigo mostra como abrir uma porta para uma VM com o 2.0 CLI do Azure. 

Para criar um grupo de segurança de rede e regras que tem a versão mais recente [Azure CLI 2.0](/cli/azure/install-az-cli2) instalado e registado para uma conta do Azure utilizando [início de sessão az](/cli/azure/reference-index#az_login).

Nos exemplos a seguir, substitua os nomes dos parâmetros de exemplo com os seus próprios valores. Os nomes dos parâmetros de exemplo incluem *myResourceGroup*, *myNetworkSecurityGroup*, e *myVnet*.


## <a name="quickly-open-a-port-for-a-vm"></a>Rapidamente abrir uma porta para uma VM
Se precisar de rapidamente abrir uma porta para uma VM num cenário de programador/teste, pode utilizar o [az vm open-porta](/cli/azure/vm#az_vm_open_port) comando. Este comando cria um grupo de segurança de rede, adiciona uma regra e aplica-se-la a uma VM ou sub-rede. O exemplo seguinte abre porta *80* na VM com o nome *myVM* no grupo de recursos denominado *myResourceGroup*.

```azure-cli
az vm open-port --resource-group myResourceGroup --name myVM --port 80
```

Para obter mais controlo sobre as regras, tal como definir um intervalo de endereços IP de origem, continue com os passos adicionais neste artigo.


## <a name="create-a-network-security-group-and-rules"></a>Criar um grupo de segurança de rede e regras
Criar o grupo de segurança de rede com [az rede nsg criar](/cli/azure/network/nsg#az_network_nsg_create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup* no *eastus* localização:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Adicionar uma regra com [criar regras de nsg de rede az](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) para permitir tráfego HTTP para o seu servidor Web (ou ajustar o seus próprios cenário, tais como a conectividade de acesso ou a base de dados SSH). O exemplo seguinte cria uma regra com o nome *myNetworkSecurityGroupRule* para permitir tráfego TCP na porta 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```


## <a name="apply-network-security-group-to-vm"></a>Aplicar o grupo de segurança de rede VM
Associar o grupo de segurança de rede com a interface de rede da VM (NIC) [atualização de nic de rede az](/cli/azure/network/nic#az_network_nic_update). O exemplo seguinte associa uma NIC existente com o nome *myNic* com o grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Em alternativa, pode associar o seu grupo de segurança de rede com uma sub-rede de rede virtual com [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update) , em vez de apenas para a interface de rede numa VM única. O exemplo seguinte associa uma sub-rede existente com o nome *mySubnet* no *myVnet* rede virtual com o grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Obter mais informações sobre grupos de segurança de rede
Os comandos rápidos aqui permitem-lhe começar a trabalhar com tráfego que flui para a VM. Grupos de segurança de rede fornecem várias funcionalidades excelentes e granularidade para controlar o acesso aos seus recursos. Pode ler mais sobre [criar um grupo de segurança de rede e a ACL regras aqui](tutorial-virtual-network.md#secure-network-traffic).

Para aplicações web de elevada disponibilidade, deve colocar as VMs por trás de um balanceador de carga do Azure. O Balanceador de carga distribui o tráfego para VMs, com um grupo de segurança de rede que fornece a filtragem de tráfego. Para obter mais informações, consulte [como carregar equilibrar os computadores virtuais Linux no Azure para criar uma aplicação altamente disponível](tutorial-load-balancer.md).

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma regra para permitir tráfego HTTP simples. Pode encontrar informações sobre como criar ambientes mais detalhadas nos seguintes artigos:

* [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [O que é um Grupo de Segurança de Rede (NSG)? (What is a Network Security Group (NSG)?)](../../virtual-network/security-overview.md)
