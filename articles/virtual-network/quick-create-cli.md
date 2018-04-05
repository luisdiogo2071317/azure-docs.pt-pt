---
title: Criar uma Azure Virtual Network - CLI do Azure | Microsoft Docs
description: Saiba mais rapidamente criar uma rede virtual com a CLI do Azure. Uma rede virtual permite que os recursos do Azure, tais como as máquinas virtuais, em privado comunicar entre si e com a internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: d07f06a1a70c859544c3b1ceb6146dc11e4d10aa
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Criar uma rede virtual com a CLI do Azure

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VM), em privado comunicar entre si e com a Internet. Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas VMs para a rede virtual. Em seguida, ligar a uma VM a partir da internet e comunicar em privado com outra VM.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 


## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de poder criar uma rede virtual, terá de criar um grupo de recursos que contém a rede virtual. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual da predefinição denominada *myVirtualNetwork* com uma sub-rede designada *predefinido*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual:

### <a name="create-the-first-vm"></a>Criar a primeira VM

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`. O `--no-wait` opção cria a VM em segundo plano, para que possa continuar para o passo seguinte. O exemplo seguinte cria uma VM chamada *myVm1*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Criar a VM segundo

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

A VM demora alguns minutos a criar. Depois de criada a VM, a CLI do Azure devolve um resultado semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

Tome nota do **publicIpAddress**. Este endereço é utilizado para ligar à VM a partir da Internet no próximo passo.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da internet

Substitua `<publicIpAddress>` com o endereço IP público do seu *myVm2* VM no comando os seguintes passos e, em seguida, introduza o seguinte comando:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-privately-between-vms"></a>Comunicar modo privado entre VMs

Para confirmar privada comunicação entre o *myVm2* e *myVm1* VMs, introduza o seguinte comando:

```bash
ping myVm1 -c 4
```

Receberá quatro respostas de *10.0.0.4*.

Sair da sessão SSH com o *myVm2* VM.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos que contém:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma rede virtual predefinido e duas VMs. Ligado a uma VM a partir da Internet e comunidades de forma clara modo privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, consulte [gerir uma rede virtual](manage-virtual-network.md). 

Por predefinição, o Azure permite a comunicação privada sem restrições entre máquinas virtuais, mas só permite a entrada de SSH de sessões para VMs com Linux através da Internet. Para saber como permitir ou restringir a diferentes tipos de comunicação de rede de e para as VMs, avançar para o próximo tutorial.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede](tutorial-filter-network-traffic-cli.md)
