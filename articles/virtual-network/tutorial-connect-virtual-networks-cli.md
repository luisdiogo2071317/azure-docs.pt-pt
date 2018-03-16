---
title: Ligar redes virtuais com o peering de rede virtual - CLI do Azure | Microsoft Docs
description: Saiba como ligar redes virtuais com o peering de rede virtual.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: bbf2e757e2d9ad76c59394ba0138a61fd4029d15
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Ligar redes virtuais com o peering de rede virtual com a CLI do Azure

Pode ligar redes virtuais entre si ao peering de rede virtual. Depois de redes virtuais em modo de peering, recursos em ambas as redes virtuais conseguem comunicar entre si, com a mesma latência e largura de banda, como se os recursos foram na mesma rede virtual. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar duas redes virtuais
> * Ligar duas redes virtuais com um peering de rede virtual
> * Implementar uma máquina virtual (VM) em cada rede virtual
> * Comunicar entre VMs

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia de introdução requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Criar redes virtuais

Antes de criar uma rede virtual, tem de criar um grupo de recursos para a rede virtual e todos os outros recursos criados neste artigo. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork1* com o prefixo de endereço *10.0.0.0/16*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Criar uma rede virtual denominada *myVirtualNetwork2* com o prefixo de endereço *10.1.0.0/16*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Redes virtuais do elemento de rede

São estabelecidas Peerings entre os IDs de rede virtual, pelo que, primeiro tem de obter o ID de cada rede virtual com [mostrar de vnet az rede](/cli/azure/network/vnet#az_network_vnet_show) e armazenar o ID numa variável.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Criar um peering do *myVirtualNetwork1* para *myVirtualNetwork2* com [az rede vnet peering criar](/cli/azure/network/vnet/peering#az_network_vnet_peering_create). Se o `--allow-vnet-access` parâmetro não for especificado, um peering é estabelecido, mas não há qualquer comunicação possam circular através do mesmo.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

No resultado devolvido após executa o comando anterior, verá que o **peeringState** é *iniciado*. Os peering permanecem no *iniciado* estado até que crie o peering de *myVirtualNetwork2* para *myVirtualNetwork1*. Criar um peering do *myVirtualNetwork2* para *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

No resultado devolvido após executa o comando anterior, verá que o **peeringState** é *ligado*. Azure também alterar o estado do peering do *myVirtualNetwork1 myVirtualNetwork2* peering para *ligado*. Confirme que o estado do peering para o *myVirtualNetwork1 myVirtualNetwork2* peering alterado para *ligado* com [az rede Mostrar peering vnet](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Recursos de uma rede virtual não é possível comunicar com os recursos na outra rede virtual até que o **peeringState** para peerings em ambas as redes virtuais é *ligado*. 

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie uma VM em cada rede virtual para que possa comunicar entre eles num passo posterior.

### <a name="create-the-first-vm"></a>Criar a primeira VM

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM chamada *myVm1* no *myVirtualNetwork1* rede virtual. Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`. O `--no-wait` opção cria a VM em segundo plano, para poder continuar para o passo seguinte.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Criar a VM segundo

Criar uma VM com o *myVirtualNetwork2* rede virtual.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

A VM demora alguns minutos a criar. Depois de criada a VM, a CLI do Azure mostra as informações semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a VM a partir da Internet num passo posterior.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

Utilize o seguinte comando para criar uma sessão SSH com o *myVm2* VM. Substitua `<publicIpAddress>` com o endereço IP público da sua VM. No exemplo anterior, o endereço IP público é *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

Enviar um ping a VM no *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

Receber quatro respostas. 

Feche a sessão SSH para a *myVm2* VM. 

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

**<a name="register"></a>Registar-se para a pré-visualização de peering de rede virtual global**

O peering de redes virtuais na mesma região encontra-se em disponibilidade geral. Peering redes virtuais em diferentes regiões está atualmente em pré-visualização. Consulte [atualizações da rede Virtual](https://azure.microsoft.com/updates/?product=virtual-network) de regiões disponíveis. A ponto redes virtuais em regiões, primeiro tem de registar para a pré-visualização, concluindo os seguintes passos (dentro da subscrição cada rede virtual que pretende elemento está no):

1. Registar-se para a pré-visualização, introduzindo os seguintes comandos:

  ```azurecli-interactive
  az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
  az provider register --name Microsoft.Network
  ```

2. Confirme que são registadas para a pré-visualização, introduzindo o seguinte comando:

  ```azurecli-interactive
  az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
  ```

  Se tentar elemento redes virtuais em diferentes regiões antes do **RegistrationState** recebe depois de introduzir o comando anterior é de saída **registada** para ambas as subscrições, falha de peering. .

## <a name="next-steps"></a>Passos seguintes

Neste artigo, aprendeu a ligar duas redes com o peering de rede virtual. Neste artigo, aprendeu a ligar duas redes, na mesma localização do Azure, com o peering de rede virtual. Também pode elemento redes virtuais no [regiões diferentes](#register), na [diferentes subscrições do Azure](create-peering-different-subscriptions.md#portal) e pode criar [hub- and -spoke designs de rede](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) com peering. Antes de peering redes virtuais de produção, recomenda-se que exaustivamente familiarizar-se com o [descrição geral do peering](virtual-network-peering-overview.md), [gerir peering](virtual-network-manage-peering.md), e [oslimitesderedevirtual](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Pode [ligar o seu próprio computador a uma rede virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) através de uma VPN e interaja com os recursos numa rede virtual, ou em redes virtuais em modo de peering. Continue a amostras de script para scripts reutilizáveis concluir a muitas das tarefas abrangidas os artigos de rede virtual.

> [!div class="nextstepaction"]
> [Exemplos de script de rede virtual](../networking/cli-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
