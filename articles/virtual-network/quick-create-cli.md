---
title: Criar uma rede virtual - início rápido - CLI do Azure | Microsoft Docs
description: Neste início rápido, vai aprender a criar uma rede virtual com o Portal do Azure. Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais, comuniquem em privado entre si e com a Internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: bb45b2b4ecd89187e94066bc81782174738fe3a9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38232706"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Início Rápido: criar uma rede virtual com a CLI do Azure

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VM), comuniquem em privado entre si e com a Internet. Neste início rápido, vai aprender a criar uma rede virtual. Depois de criar uma rede virtual, vai implementar duas VMs na rede virtual. Em seguida, vai ligar a uma VM a partir da Internet e comunicar em privado com a outra VM.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este início rápido requer a execução da versão 2.0.28 ou posterior da CLI do Azure. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). 


## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de criar uma rede virtual, tem de criar um grupo de recursos para conter a rede virtual. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual predefinida com o nome *myVirtualNetwork*, com uma sub-rede com o nome *predefinido*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual:

### <a name="create-the-first-vm"></a>Criar a primeira VM

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). Se as chaves SSH ainda não existirem numa localização de chaves predefinida, o comando cria-as. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`. A opção `--no-wait` cria a VM em segundo plano, para que possa prosseguir para o passo seguinte. O exemplo seguinte cria uma VM com o nome *myVm1*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Criar a segunda VM

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

A criação da VM demora alguns minutos. Depois de a VM ser criada, a CLI do Azure devolve um resultado semelhante ao seguinte exemplo: 

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

Anote o **publicIpAddress**. Este endereço é utilizado para ligar à VM a partir da Internet no próximo passo.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da Internet

Substitua `<publicIpAddress>` pelo endereço IP público da sua VM *myVm2* no comando seguinte e, em seguida, introduza o seguinte comando:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Comunicar entre VMs

Para confirmar a comunicação privada entre as VMs *myVm2* e *myVm1*, introduza o seguinte comando:

```bash
ping myVm1 -c 4
```

Receberá quatro respostas de *10.0.0.4*.

Saia da sessão SSH com a VM *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar [az group delete](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos que contém:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma rede virtual predefinida e duas VMs. Ligou a uma VM a partir da Internet e comunicou em privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, veja [Gerir uma rede virtual](manage-virtual-network.md). 

Por predefinição, o Azure permite a comunicação privada sem restrições entre máquinas virtuais, mas só permite ligações de ambiente de trabalho remoto de entrada a VMs do Windows a partir da Internet. Para saber como permitir ou restringir diferentes tipos de comunicação de rede de e para as VMs, veja [Filtrar tráfego de rede](tutorial-filter-network-traffic.md).
