---
title: Encaminhar o tráfego de rede - CLI do Azure | Microsoft Docs
description: Neste artigo, saiba como encaminhar o tráfego de rede com uma tabela de rota utilizando a CLI do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: eb4a28b5a57d7e301e800cd4ad87c56b7c5df6d2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841950"
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Encaminhar o tráfego de rede com uma tabela de rota utilizando a CLI do Azure

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede (NVA). Neste artigo, saiba como:

* Criar uma tabela de rota
* Criar uma rota
* Criar uma rede virtual com várias sub-redes
* Associar uma tabela de rota para uma sub-rede
* Criar uma NVA que encaminha o tráfego
* Implementar máquinas virtuais (VM) em sub-redes diferentes
* Encaminhar o tráfego de uma sub-rede para outra através de uma NVA

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia de introdução requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Criar uma tabela de rota

Antes de poder criar uma tabela de rota, crie um grupo de recursos com [criar grupo az](/cli/azure/group#az_group_create) para todos os recursos criados neste artigo. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Criar uma tabela de rotas com [criar tabela de rotas do rede az](/cli/azure/network/route#az_network_route_table_create). O exemplo seguinte cria uma tabela de rota chamada *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Criar uma rota

Criar uma rota na tabela de rotas com [criar rota de tabela de rotas de rede az](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rota para uma sub-rede

Antes de poder associar uma tabela de rota para uma sub-rede, terá de criar uma rede virtual e a sub-rede. Criar uma rede virtual com uma sub-rede com [az rede vnet criar](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Criar duas sub-redes adicionais com [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

Associar o *myRouteTablePublic* tabela de rotas para o *pública* sub-rede com [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>Criar uma NVA

Uma NVA, é uma VM que executa uma função de rede, tais como o encaminhamento de mensagens em fila, firewalling ou otimização de WAN.

Criar uma NVA no *DMZ* sub-rede com [az vm criar](/cli/azure/vm#az_vm_create). Quando cria uma VM, o Azure cria e atribui um endereço IP público à VM, por predefinição. O `--public-ip-address ""` parâmetro dá instruções ao Azure não para criar e atribuir um endereço IP público à VM, uma vez que a VM não tem de estar ligada a partir da internet. Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

A VM demora alguns minutos a criar. Continue para o passo seguinte até que o Azure acaba de criar a VM e devolve um resultado sobre a VM. 

Para uma interface de rede conseguir encaminhar o tráfego de rede enviado, que não é destinado a entidades para o seu próprio endereço IP, o reencaminhamento IP tem de estar ativado para a interface de rede. Ativar o reencaminhamento IP para a interface de rede com [atualização de nic de rede az](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Dentro da VM, o sistema operativo ou uma aplicação em execução dentro da VM, deve também conseguir encaminhar o tráfego de rede. Ativar o reencaminhamento IP no sistema de operativo de uma VM com [conjunto de extensão de vm az](/cli/azure/vm/extension#az_vm_extension_set):

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
O comando pode demorar um minuto para executar.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas VMs na rede virtual para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da NVA num passo posterior. 

Criar uma VM com o *pública* sub-rede com [az vm criar](/cli/azure/vm#az_vm_create). O `--no-wait` parâmetro ativa do Azure ao executar o comando em segundo plano, para poder continuar para o comando seguinte. Para simplificar este artigo, é utilizada uma palavra-passe. As chaves são normalmente utilizadas nas implementações de produção. Se utilizar chaves, também tem de configurar o reencaminhamento de agente SSH. Para obter mais informações, consulte a documentação para o cliente SSH. Substitua `<replace-with-your-password>` no seguinte comando com uma palavra-passe da sua escolha.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Criar uma VM com o *privada* sub-rede.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

A VM demora alguns minutos a criar. Depois de criada a VM, a CLI do Azure mostra as informações semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPrivate",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a VM a partir da internet num passo posterior.

## <a name="route-traffic-through-an-nva"></a>Encaminhar o tráfego através de uma NVA

Utilize o seguinte comando para criar uma sessão SSH com o *myVmPrivate* VM. Substitua *<publicIpAddress>* com o endereço IP público da sua VM. No exemplo acima, o endereço IP é *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Quando lhe for pedido para uma palavra-passe, introduza a palavra-passe que selecionou no [criar máquinas virtuais](#create-virtual-machines).

Utilize o seguinte comando para instalar a rota de rastreio no *myVmPrivate* VM:

```bash 
sudo apt-get install traceroute
```

Utilize o seguinte comando para testar o encaminhamento de tráfego de rede para o *myVmPublic* VM a partir de *myVmPrivate* VM.

```bash
traceroute myVmPublic
```

A resposta é semelhante ao seguinte exemplo:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Pode ver que o tráfego é encaminhado diretamente a partir de *myVmPrivate* VM para o *myVmPublic* VM. Rotas predefinidas do Azure, encaminhar o tráfego diretamente entre sub-redes. 

Utilize o seguinte comando para SSH para a *myVmPublic* VM a partir de *myVmPrivate* VM:

```bash 
ssh azureuser@myVmPublic
```

Utilize o seguinte comando para instalar a rota de rastreio no *myVmPublic* VM:

```bash 
sudo apt-get install traceroute
```

Utilize o seguinte comando para testar o encaminhamento de tráfego de rede para o *myVmPrivate* VM a partir de *myVmPublic* VM.

```bash
traceroute myVmPrivate
```

A resposta é semelhante ao seguinte exemplo:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a NVA. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmPrivate* VM. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.

Fechar as sessões SSH para ambos os *myVmPublic* e *myVmPrivate* VMs.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criou uma NVA simple que encaminhados tráfego de sub-rede público para uma sub-rede privada. Implementar uma variedade de NVAs pré-configurados que efetuam funções de rede, tais como a firewall e a otimização de WAN do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Para saber mais sobre o encaminhamento, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md) e [gerir uma tabela de rota](manage-route-table.md).

Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Para saber como, consulte [restringir o acesso de rede para recursos de PaaS](tutorial-restrict-network-access-to-resources-cli.md).
