---
title: Diagnosticar um máquina virtual encaminhamento problema de rede - CLI do Azure | Microsoft Docs
description: Neste artigo, irá aprender a diagnosticar um problema encaminhamento de rede de máquina virtual utilizando a capacidade de salto seguinte do observador de rede do Azure.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: fcb7ec2e40b5c0e8794d2f4d70395dcbecca019c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Diagnosticar um máquina virtual encaminhamento problema de rede - CLI do Azure

Neste artigo, implementar uma máquina virtual (VM) e, em seguida, verifique as comunicações com um endereço IP e o URL. Determinar a causa de uma falha de comunicação e como resolvê-lo.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Depois de verificar a versão do CLI, execute `az login` para criar uma ligação com o Azure. Os comandos da CLI neste artigo são formatados para executar na shell de deteção.

## <a name="create-a-vm"></a>Criar uma VM

Antes de poder criar uma VM, tem de criar um grupo de recursos que contém a VM. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). Se as chaves SSH ainda não existirem numa localização de chaves predefinida, o comando cria-as. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`. O exemplo seguinte cria uma VM chamada *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

A criação da VM demora alguns minutos. Não continue com os restantes passos até que a VM é criada e a CLI devolve um resultado.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Para testar a comunicação de rede com o observador de rede, tem de ativar primeiro um observador de rede na região a VM que pretende testar está a ser e, em seguida, utilizar a capacidade de salto seguinte do observador de rede para testar a comunicação.

### <a name="enable-network-watcher"></a>Ativar o observador de rede

Se já tiver um observador de rede ativado na região EUA leste, avance para o [salto seguinte utilize](#use-next-hop). Utilize o [observador de rede az configurar](/cli/azure/network/watcher#az-network-watcher-configure) comando para criar um observador de rede na região EUA Leste:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Utilizar o próximo salto

Azure cria automaticamente as rotas para destinos de predefinição. Pode criar rotas personalizadas que substituem as rotas predefinidas. Por vezes, as rotas personalizadas podem fazer com comunicação falhar. Para testar o encaminhamento de uma VM, utilize [Mostrar-next-hop do observador de rede az](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) para determinar o próximo salto de encaminhamento quando o tráfego é destinado a um endereço específico.

Comunicações de saída da VM para um dos endereços IP para www.bing.com de teste:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Após alguns segundos, o resultado informa-o que o **nextHopType** é **Internet**e que o **routeTableId** é **rota de sistema**. Este resultado permite-lhe saber que existe uma rota válida para o destino.

Comunicações de saída da VM para 172.31.0.100 de teste:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Informa-o resultado devolvido que **nenhum** é o **nextHopType**e que o **routeTableId** também é **rota de sistema**. Este resultado permite-lhe saber que, enquanto existir uma rota de sistema válido para o destino, não há nenhum salto seguinte para encaminhar o tráfego para o destino.

## <a name="view-details-of-a-route"></a>Ver detalhes de uma rota

Para analisar ainda mais a encaminhamento, reveja as rotas efetivas para a interface de rede com o [nic Mostrar-eficaz--a tabela de rotas de rede az](/cli/azure/network/nic#az-network-nic-show-effective-route-table) comando:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

O seguinte texto está incluído na saída devolvida:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Quando utilizado o `az network watcher show-next-hop` comandos para testar a comunicação de saída para 13.107.21.200 no [próximo salto de utilização](#use-next-hop), a rota com o **addressPrefix** 0.0.0.0/0** foi utilizado para encaminhar o tráfego para o endereço, uma vez que nenhuma outra rota no resultado inclui o endereço. Por predefinição, todos os endereços especificados não o prefixo do endereço de outra rota são encaminhados para a internet.

Quando tiver utilizado o `az network watcher show-next-hop` de comandos para testar as comunicações de saída para 172.31.0.100 no entanto, o resultado informado que não ocorreu nenhum tipo de salto seguinte. Na saída devolvida Consulte também o seguinte texto:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

Como pode ver no resultado do `az network watcher nic show-effective-route-table` comando, apesar de não existir uma rota predefinida para o prefixo 172.16.0.0/12, que inclui o 172.31.0.100 endereço, a **nextHopType** é **nenhum**. Azure cria uma rota predefinida para 172.16.0.0/12, mas não especifica um tipo de próximo salto até um motivo para. Se, por exemplo, o intervalo de endereços 172.16.0.0/12 tiver adicionado ao espaço de endereços da rede virtual, o Azure altera o **nextHopType** para **rede Virtual** da rota. Uma verificação de, em seguida, seria mostrar **rede Virtual** como o **nextHopType**.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar [az group delete](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos que contém:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma VM e diagnosticado encaminhamento de rede da VM. Aprendeu a que o Azure cria várias rotas predefinidas e testado encaminhamento para duas destinos diferentes. Saiba mais sobre [encaminhamento no Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e como [criar rotas personalizadas](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Para ligações de saída VM, também é possível determinar a latência e e rejeições tráfego de rede entre a VM e um ponto final com o observador de rede [resolver problemas de ligação](network-watcher-connectivity-cli.md) capacidade. Pode monitorizar a comunicação entre uma VM e um ponto final, tais como um endereço IP ou o URL, ao longo do tempo, utilizar a capacidade de monitor de ligação do observador de rede. Para saber como, consulte [monitorizar uma ligação de rede](connection-monitor.md).