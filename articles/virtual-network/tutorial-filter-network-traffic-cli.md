---
title: Filtrar o tráfego de rede - CLI do Azure | Microsoft Docs
description: Neste artigo, irá aprender a filtrar o tráfego de rede a uma sub-rede, com um grupo de segurança de rede, utilizando a CLI do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 11dc0e5f6ee398b2a745ed60cbc166e2a1697c3e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Filtrar o tráfego de rede com um grupo de segurança de rede utilizando a CLI do Azure

Pode filtrar o tráfego de rede de entrada para e de saída de uma sub-rede de rede virtual com um grupo de segurança de rede. Grupos de segurança de rede contêm regras de segurança que filtrar o tráfego de rede pelo endereço IP, porta e protocolo. Regras de segurança são aplicadas a recursos implementados numa sub-rede. Neste artigo, vai aprender a:

* Criar regras de grupo e de segurança de um segurança de rede
* Criar uma rede virtual e associar um grupo de segurança de rede a uma sub-rede
* Implementar máquinas virtuais (VM) a uma sub-rede
* Filtros de tráfego de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Um grupo de segurança de rede contém regras de segurança. Especificam regras de segurança de uma origem e de destino. As origens e destinos podem ser grupos de segurança de aplicações.

### <a name="create-application-security-groups"></a>Criar grupos de segurança de aplicações

Em primeiro lugar, crie um grupo de recursos para todos os recursos criados neste artigo com [criar grupo az](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos a *eastus* localização: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Criar um grupo de segurança de aplicações com [az rede asg criar](/cli/azure/network/asg#az_network_asg_create). Um grupo de segurança de aplicações permite-lhe para servidores de grupo com os requisitos de filtragem de portas semelhantes. O exemplo seguinte cria dois grupos de segurança da aplicação.

```azurecli-interactive
az network asg create \
  --resource-group myResourceGroup \
  --name myAsgWebServers \
  --location eastus

az network asg create \
  --resource-group myResourceGroup \
  --name myAsgMgmtServers \
  --location eastus
```

### <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Criar um grupo de segurança de rede com [az rede nsg criar](/cli/azure/network/nsg#az_network_nsg_create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNsg*: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Criar regras de segurança

Criar uma regra de segurança com [criar regras de nsg de rede az](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). O exemplo seguinte cria uma regra que permite o tráfego de entrada a partir da internet para o *myWebServers* grupo de segurança de aplicações através das portas 80 e 443:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-Web-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgWebServers" \
  --destination-port-range 80 443
```

O exemplo seguinte cria uma regra que permite o tráfego de entrada a partir da Internet para o *myMgmtServers* grupo de segurança de aplicações através da porta 22:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 110 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgMgmtServers" \
  --destination-port-range 22
```

Neste artigo, SSH (porta 22) está exposto à internet para o *myAsgMgmtServers* VM. Para ambientes de produção, em vez de exposição a porta 22 à internet, recomenda-se que o se ligar aos recursos do Azure que pretende gerir com um [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [privada](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ligação de rede.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria um virtual com o nome *myVirtualNetwork*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

Adicionar uma sub-rede para uma rede virtual com [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). O exemplo seguinte adiciona uma sub-rede designada *mySubnet* para a rede virtual e, em seguida, associa a *myNsg* grupo de segurança de rede ao mesmo:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual para que possa validar tráfego filtragem num passo posterior. 

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM que irá servir como um servidor web. O `--asgs myAsgWebServers` opção faz com que o Azure tornar a interface de rede cria para a VM membro o *myAsgWebServers* grupo de segurança da aplicação.

O `--nsg ""` opção é especificada para impedir a criação de um grupo de segurança de rede predefinido para a interface de rede Azure cria quando cria a VM do Azure. Para simplificar este artigo, é utilizada uma palavra-passe. As chaves são normalmente utilizadas nas implementações de produção. Se utilizar chaves, também tem de configurar o reencaminhamento para os restantes passos de agente SSH. Para obter mais informações, consulte a documentação para o cliente SSH. Substitua `<replace-with-your-password>` no seguinte comando com uma palavra-passe da sua escolha.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgWebServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

A VM demora alguns minutos a criar. Depois de criada a VM, é devolvido o resultado semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmWeb",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a VM a partir da internet num passo posterior.  Crie uma VM para servir como um servidor de gestão:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgMgmtServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

A VM demora alguns minutos a criar. Depois de criada a VM, tenha em atenção o **publicIpAddress** na saída devolvida. Este endereço é utilizado para aceder a VM no próximo passo. Não continue com o passo seguinte até que o Azure acaba de criar a VM.

## <a name="test-traffic-filters"></a>Filtros de tráfego de teste

Utilize o comando que se segue para criar uma sessão SSH com o *myVmMgmt* VM. Substitua *<publicIpAddress>* com o endereço IP público da sua VM. No exemplo acima, o endereço IP é *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Quando lhe for pedido para uma palavra-passe, introduza a palavra-passe que introduziu no [criar VMs](#create-virtual-machines).

A ligação tiver êxito, porque a porta 22 é permitida a entrada da Internet para o *myAsgMgmtServers* grupo de segurança de aplicações que a interface de rede ligada à *myVmMgmt* a VM está na.

Utilize o seguinte comando para SSH para a *myVmWeb* VM a partir de *myVmMgmt* VM:

```bash 
ssh azureuser@myVmWeb
```

A ligação é bem-sucedida porque uma regra de segurança predefinida de cada grupo de segurança de rede permite que o tráfego através de todas as portas entre todos os endereços IP numa rede virtual. Não é possível SSH para a *myVmWeb* VM a partir da Internet porque a regra de segurança para o *myAsgWebServers* não permite a porta 22 de entrada da Internet.

Utilize os seguintes comandos para instalar o servidor de web nginx no *myVmWeb* VM:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

O *myVmWeb* VM tem permissão de saída à Internet para obter nginx porque uma regra de segurança predefinida permite que todo o tráfego de saída à Internet. Saída de *myVmWeb* sessão SSH, deixando-na `username@myVmMgmt:~$` linha de comandos do *myVmMgmt* VM. Para obter o ecrã de boas-vindas nginx do *myVmWeb* VM, introduza o seguinte comando:

```bash
curl myVmWeb
```

Termine a sessão do *myVmMgmt* VM. Para confirmar se consegue aceder a *myVmWeb* web server a partir de fora do Azure, introduza `curl <publicIpAddress>` do seu computador. A ligação tiver êxito, porque a porta 80 é permitida a entrada da Internet para o *myAsgWebServers* grupo de segurança de aplicações que a interface de rede ligada à *myVmWeb* a VM está na.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou um grupo de segurança de rede e associados-la a uma sub-rede de rede virtual. Para obter mais informações sobre grupos de segurança de rede, consulte [descrição geral de grupo de segurança de rede](security-overview.md) e [gerir um grupo de segurança de rede](manage-network-security-group.md).

Tráfego de rotas do Azure entre sub-redes por predefinição. Em vez disso, pode optar por encaminhar o tráfego entre sub-redes através de uma VM, que serve como uma firewall, por exemplo. Para saber como, consulte [criar uma tabela de rota](tutorial-create-route-table-cli.md).