---
title: "Encaminhar o tráfego de rede - CLI do Azure | Microsoft Docs"
description: "Saiba como encaminhar o tráfego de rede com uma tabela de rota utilizando a CLI do Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 3c16c774fa1c8a5c8bf50b4f4f9d0bfb283318e3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Encaminhar o tráfego de rede com uma tabela de rota utilizando a CLI do Azure

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma firewall. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma tabela de rota
> * Criar uma rota
> * Associar uma tabela de rota para uma sub-rede de rede virtual
> * Encaminhamento de teste
> * Resolver problemas de encaminhamento

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia de introdução requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Criar uma tabela de rota

Tráfego de rotas do Azure entre todas as sub-redes na rede virtual, por predefinição. Para saber mais sobre as rotas predefinidas do Azure, consulte o artigo [rotas de sistema](virtual-networks-udr-overview.md). Substituir predefinição do Azure encaminhamento, pode criar uma tabela de rota que contém rotas e associar a tabela de rotas para uma sub-rede de rede virtual.

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

Uma tabela de rota contém zero ou mais rotas. Criar uma rota na tabela de rotas com [criar rota de tabela de rotas de rede az](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

A rota irá direcionar todo o tráfego destinado ao prefixo do endereço 10.0.1.0/24 através de uma aplicação virtual de rede com o endereço IP 10.0.2.4. O dispositivo de rede virtual e a sub-rede com o prefixo de endereço especificado são criados em passos posteriores. A rota substitui a predefinição do Azure encaminhamento, que encaminha o tráfego entre sub-redes diretamente. Cada rota especifica um tipo de salto seguinte. O tipo de próximo salto dá instruções ao Azure como encaminhar o tráfego. Neste exemplo, o tipo de próximo salto é *VirtualAppliance*. Para saber mais sobre os tipos de salto seguintes disponíveis no Azure e, quando a utilizá-los, consulte [próximo salto tipos](virtual-networks-udr-overview.md#custom-routes).

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

Pode associar uma tabela de rota para zero ou mais sub-redes. Uma sub-rede pode ter zero ou uma tabela de rotas associada ao mesmo. Tráfego de saída de uma sub-rede é encaminhado com base nas rotas predefinidas do Azure e as rotas personalizadas que adicionou a uma tabela de rota que associar a uma sub-rede. Associar o *myRouteTablePublic* tabela de rotas para o *pública* sub-rede com [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

Antes de implementar as tabelas de rotas para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com [encaminhamento no Azure](virtual-networks-udr-overview.md) e [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Encaminhamento de teste

Para testar o encaminhamento, irá criar uma máquina virtual que funciona como a aplicação virtual de rede que a rota que criou no passo anterior encaminha através de. Depois de criar a aplicação virtual de rede, irá implementar uma máquina virtual para o *pública* e *privada* sub-redes. Em seguida, irá encaminhar o tráfego do *pública* sub-rede para o *privada* sub-rede através da aplicação virtual de rede.

### <a name="create-a-network-virtual-appliance"></a>Criar uma aplicação virtual de rede

No passo anterior, criou uma rota que especificada uma aplicação virtual de rede como o tipo de salto seguinte. Uma máquina virtual com uma aplicação de rede é frequentemente referida como uma aplicação virtual de rede. Em ambientes de produção, a aplicação virtual de rede que implementa é frequentemente uma máquina virtual previamente configurada. Várias aplicações virtuais de rede estão disponíveis a partir de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Neste artigo, é criada uma máquina virtual básica. 

Criar uma aplicação virtual de rede no *DMZ* sub-rede com [az vm criar](/cli/azure/vm#az_vm_create). Quando cria uma máquina virtual, o Azure cria e atribui um endereço IP público para a máquina virtual, por predefinição. O `--public-ip-address ""` parâmetro dá instruções ao Azure não para criar e atribuir um endereço IP público para a máquina virtual, uma vez que a máquina virtual não tem de estar ligada a partir da Internet. Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`.

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

A máquina virtual demora alguns minutos a criar. Continue para o passo seguinte até que o Azure acaba de criar a máquina virtual e devolve um resultado sobre a máquina virtual. Em ambientes de produção, a aplicação virtual de rede que implementa é frequentemente uma máquina virtual previamente configurada. Várias aplicações virtuais de rede estão disponíveis a partir de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Tem de ativar o reencaminhamento IP para cada Azure [interface de rede](virtual-network-network-interface.md) ligado a uma máquina virtual, que reencaminha o tráfego destinado a qualquer endereço IP que não está atribuído à interface de rede. Quando criou a aplicação virtual de rede no *DMZ* sub-rede, o Azure criada automaticamente uma interface de rede com o nome *myVmNvaVMNic*, anexar a interface de rede para a máquina virtual, e o endereço IP privado atribuído *10.0.2.4* à interface de rede. Ativar o reencaminhamento IP para a interface de rede com [atualização de nic de rede az](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Na máquina virtual, o sistema operativo ou uma aplicação em execução numa máquina virtual, deve também conseguir encaminhar o tráfego de rede. Quando implementa uma aplicação virtual de rede num ambiente de produção, o dispositivo normalmente filtra, regista ou efetua algumas outra função antes do reencaminhamento de tráfego. No entanto, neste artigo o sistema operativo simplesmente reencaminham todo o tráfego que recebe. Ativar o reencaminhamento IP no sistema de operativo da máquina virtual com [conjunto de extensão de vm az](/cli/azure/vm/extension#az_vm_extension_set), que executa um comando que permite que o reencaminhamento IP no sistema operativo.

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
O comando pode demorar um minuto para executar.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas máquinas virtuais na rede virtual para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da aplicação virtual de rede num passo posterior. 

Criar uma máquina virtual no *pública* sub-rede com [az vm criar](/cli/azure/vm#az_vm_create). O `--no-wait` parâmetro ativa do Azure ao executar o comando em segundo plano, para poder continuar para o comando seguinte. Para simplificar este artigo, é utilizada uma palavra-passe. As chaves são normalmente utilizadas nas implementações de produção. Se utilizar chaves, também tem de configurar o reencaminhamento de agente SSH. Para obter mais informações, consulte a documentação para o cliente SSH. Substitua `<replace-with-your-password>` no seguinte comando com uma palavra-passe da sua escolha.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Criar uma máquina virtual no *privada* sub-rede.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

A máquina virtual demora alguns minutos a criar. Depois da máquina virtual é criada, a CLI do Azure mostra as informações semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a máquina virtual a partir da Internet num passo posterior.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Encaminhar o tráfego através de uma aplicação virtual de rede

Utilize o seguinte comando para criar uma sessão SSH com o *myVmMgmt* máquina virtual. Substitua  *<publicIpAddress>*  com o endereço IP público da sua máquina virtual. No exemplo acima, o endereço IP é *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Quando lhe for pedido para uma palavra-passe, introduza a palavra-passe que selecionou no [criar máquinas virtuais](#create-virtual-machines).

Utilize o seguinte comando para instalar traceroute no *myVmMgmt* máquina virtual:

```bash 
sudo apt-get install traceroute
```

Utilize o seguinte comando para testar o encaminhamento de tráfego de rede para o *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual.

```bash
traceroute myvmweb
```

A resposta é semelhante ao seguinte exemplo:

```bash
traceroute to myvmweb (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Pode ver que o tráfego é encaminhado diretamente a partir de *myVmMgmt* máquina virtual para o *myVmWeb* máquina virtual. Rotas predefinidas do Azure, encaminhar o tráfego diretamente entre sub-redes. 

Utilize o seguinte comando para SSH para a *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual:

```bash 
ssh azureuser@myVmWeb
```

Utilize o seguinte comando para instalar traceroute no *myVmWeb* máquina virtual:

```bash 
sudo apt-get install traceroute
```

Utilize o seguinte comando para testar o encaminhamento de tráfego de rede para o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual.

```bash
traceroute myvmmgmt
```

A resposta é semelhante ao seguinte exemplo:

```bash
traceroute to myvmmgmt (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a rede aplicação virtual. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmMgmt* máquina virtual. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.

Fechar as sessões SSH para ambos os *myVmWeb* e *myVmMgmt* máquinas virtuais.

## <a name="troubleshoot-routing"></a>Resolver problemas de encaminhamento

Conforme aprendido nos passos anteriores, Azure aplica-se rotas predefinidas, que pode, opcionalmente, substituir com as seus próprios rotas. Por vezes, o tráfego poderão não ser encaminhado conforme esperado que seja. Utilize [Mostrar-next-hop do observador de rede az](/cli/azure/network/watcher#az_network_watcher_show_next_hop) para determinar a forma como o tráfego é encaminhado entre duas máquinas virtuais. Por exemplo, o seguinte comando os testes de encaminhamento de tráfego do *myVmWeb* (10.0.0.4) máquina virtual para o *myVmMgmt* (10.0.1.4) máquina virtual:

```azurecli-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region.
az network watcher configure --locations eastus --resource-group myResourceGroup --enabled true

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 10.0.1.4 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVmWeb \
  --out table
```
O seguinte resultado é devolvido após uma curta espera:

```azurecli
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

A saída informa que o próximo salto endereço IP para o tráfego de *myVmWeb* para *myVmMgmt* é 10.0.2.4 (o *myVmNva* máquina virtual), que o tipo de próximo salto é  *VirtualAppliance*, sendo que a tabela de rota que faz com que o encaminhamento *myRouteTablePublic*.

As rotas em vigor para cada interface de rede são uma combinação de rotas predefinidas do Azure e as rotas que define. Consulte todas as rotas eficazes para uma interface de rede numa máquina virtual com [nic Mostrar-eficaz--a tabela de rotas de rede az](/cli/azure/network/nic#az_network_nic_show_effective_route_table). Por exemplo, para mostrar as rotas efetivas para o *MyVmWebVMNic* interface de rede no *myVmWeb* máquina virtual, introduza o seguinte comando:

```azurecli-interactive
az network nic show-effective-route-table \
  --name MyVmWebVMNic \
  --resource-group myResourceGroup
```

Todas as rotas predefinidas e a rota adicionado no passo anterior, são devolvidos.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criar uma aplicação virtual de rede que encaminhados tráfego de sub-rede público para uma sub-rede privada. Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Avançar para o próximo tutorial para saber como restringir o acesso de rede para recursos do Azure PaaS.

> [!div class="nextstepaction"]
> [Restringir o acesso de rede para recursos de PaaS](virtual-network-service-endpoints-configure.md#azure-cli)
