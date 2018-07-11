---
title: Criar uma VM do Linux no Azure com várias NICs | Documentos da Microsoft
description: Saiba como criar uma VM do Linux com vários NICs ligados ao mesmo com os modelos da CLI 2.0 do Azure ou do Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: 257b80c30823be41893be8659845d4fcbc922da3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932277"
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Como criar uma máquina virtual Linux no Azure com rede várias placas de interface
Pode criar uma máquina virtual (VM) no Azure que tenha várias interfaces de rede virtual (NICs) ligadas ao mesmo. Um cenário comum é ter diferentes sub-redes para conectividade de front-end e back-end ou uma rede dedicada para uma solução de monitorização ou cópia de segurança. Este artigo fornece detalhes sobre como criar uma VM com várias NICs ligadas a si e como adicionar ou remover NICs a uma VM existente. Diferentes [tamanhos de VM](sizes.md) suporta um número variável de NICs, então, da mesma forma a dimensionar sua VM.

Este artigo fornece detalhes sobre como criar uma VM com vários NICs com a CLI 2.0 do Azure. 


## <a name="create-supporting-resources"></a>Criar recursos de suporte
Instalar a versão mais recente [CLI do Azure 2.0](/cli/azure/install-az-cli2) e para iniciar sessão no Azure através da conta [início de sessão az](/cli/azure/reference-index#az_login).

Nos exemplos a seguir, substitua os nomes de parâmetros de exemplo pelos seus próprios valores. Os nomes de parâmetros de exemplo incluídos *myResourceGroup*, *mystorageaccount*, e *myVM*.

Primeiro, crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Criar a rede virtual com [vnet de rede de az criar](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual denominada *myVnet* e a sub-rede com o nome *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Crie uma sub-rede para o tráfego de back-end com [criar a sub-rede de vnet de rede de az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). O exemplo seguinte cria uma sub-rede denominada *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Crie um grupo de segurança de rede com [nsg de rede de az criar](/cli/azure/network/nsg#az_network_nsg_create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Criar e configurar vários NICs
Criar dois NICs com [nic da rede de az criar](/cli/azure/network/nic#az_network_nic_create). O exemplo seguinte cria dois NICs, com o nome *myNic1* e *myNic2*, ligado o grupo de segurança de rede, com uma NIC para ligar a cada sub-rede:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Criar uma VM e ligar as NICs
Ao criar a VM, especifique os NICs que criou com `--nics`. Terá também de ter cuidado ao selecionar o tamanho da VM. Existem limites para o número total de NICs que podem ser adicionados a uma VM. Leia mais sobre [tamanhos de VM do Linux](sizes.md). 

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

Adicionar tabelas de roteamento para o SO convidado, concluindo os passos em [configurar o sistema operacional convidado para vários NICs](#configure-guest-os-for- multiple-nics).

## <a name="add-a-nic-to-a-vm"></a>Adicionar uma NIC a uma VM
Os passos anteriores criaram uma VM com várias NICs. Também pode adicionar NICs a uma VM existente com a CLI 2.0 do Azure. Diferentes [tamanhos de VM](sizes.md) suporta um número variável de NICs, então, da mesma forma a dimensionar sua VM. Se for necessário, pode [redimensionar uma VM](change-vm-size.md).

Crie outro NIC com [nic da rede de az criar](/cli/azure/network/nic#az_network_nic_create). O exemplo seguinte cria um NIC com o nome *myNic3* ligada ao back-end sub-rede e de rede de grupo de segurança criado nos passos anteriores:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

Para adicionar um NIC a uma VM existente, desaloque a VM com [az vm deallocate](/cli/azure/vm#az_vm_deallocate). O exemplo seguinte desaloca a VM com o nome *myVM*:


```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Adicionar a NIC com o [az vm nic adicionar](/cli/azure/vm/nic#az_vm_nic_add). O exemplo seguinte adiciona *myNic3* ao *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Iniciar a VM com [início de vm de az](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

Adicionar tabelas de roteamento para o SO convidado, concluindo os passos em [configurar o sistema operacional convidado para vários NICs](#configure-guest-os-for- multiple-nics).

## <a name="remove-a-nic-from-a-vm"></a>Remover um NIC de uma VM
Para remover um NIC de uma VM existente, desaloque a VM com [az vm deallocate](/cli/azure/vm#az_vm_deallocate). O exemplo seguinte desaloca a VM com o nome *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Remover a NIC com o [az vm nic remover](/cli/azure/vm/nic#az_vm_nic_remove). O exemplo seguinte remove *myNic3* partir *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Iniciar a VM com [início de vm de az](/cli/azure/vm#az_vm_start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Criar vários NICs com modelos do Resource Manager
Modelos Azure Resource Manager utilizam ficheiros declarativos do JSON para definir o seu ambiente. Pode ler uma [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Modelos do Resource Manager fornecem uma forma de criar várias instâncias de um recurso durante a implementação, como a criação de várias NICs. Utilizar *cópia* para especificar o número de instâncias para criar:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Leia mais sobre [criar várias instâncias usando *cópia*](../../resource-group-create-multiple.md). 

Também pode utilizar um `copyIndex()` para, em seguida, anexar um número para um nome de recurso, o que permite que crie `myNic1`, `myNic2`, etc. O código a seguir mostra um exemplo de acrescentar o valor de índice:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Pode ler um exemplo completo de [criando vários NICs com modelos do Resource Manager](../../virtual-network/template-samples.md).

Adicionar tabelas de roteamento para o SO convidado, concluindo os passos em [configurar o sistema operacional convidado para vários NICs](#configure-guest-os-for- multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Configurar o sistema operacional convidado para vários NICs
Quando adiciona várias NICs a uma VM do Linux, tem de criar regras de encaminhamento. Estas regras permitem que a VM enviar e receber tráfego que pertence a uma NIC específicas. O tráfego, caso contrário, que pertence *eth1*, por exemplo, não consegue processar corretamente a rota padrão definido.

Para corrigir este problema de encaminhamento, adicione duas tabelas de roteamento para */etc/iproute2/rt_tables* da seguinte forma:

```bash
echo "200 eth0-rt" >> /etc/iproute2/rt_tables
echo "201 eth1-rt" >> /etc/iproute2/rt_tables
```

Para efetuar a alteração persistente e aplicadas durante a ativação da pilha de rede, edite */etc/sysconfig/network-scripts/ifcfg-eth0* e */etc/sysconfig/network-scripts/ifcfg-eth1*. Alterar a linha *"NM_CONTROLLED = yes"* ao *"NM_CONTROLLED = não"*. Sem essa etapa, o adicional regras/encaminhamento não são automaticamente aplicadas.
 
Em seguida, expanda as tabelas de roteamento. Vamos supor que temos a seguinte configuração no local:

*Encaminhamento*

```bash
default via 10.0.1.1 dev eth0 proto static metric 100
10.0.1.0/24 dev eth0 proto kernel scope link src 10.0.1.4 metric 100
10.0.1.0/24 dev eth1 proto kernel scope link src 10.0.1.5 metric 101
168.63.129.16 via 10.0.1.1 dev eth0 proto dhcp metric 100
169.254.169.254 via 10.0.1.1 dev eth0 proto dhcp metric 100
```

*Interfaces*

```bash
lo: inet 127.0.0.1/8 scope host lo
eth0: inet 10.0.1.4/24 brd 10.0.1.255 scope global eth0    
eth1: inet 10.0.1.5/24 brd 10.0.1.255 scope global eth1
```

Em seguida, seria criar os seguintes ficheiros e adicionar as regras adequadas e as rotas a cada:

- */etc/sysconfig/network-scripts/rule-eth0*

    ```bash
    from 10.0.1.4/32 table eth0-rt
    to 10.0.1.4/32 table eth0-rt
    ```

- */etc/sysconfig/network-scripts/route-eth0*

    ```bash
    10.0.1.0/24 dev eth0 table eth0-rt
    default via 10.0.1.1 dev eth0 table eth0-rt
    ```

- */etc/sysconfig/network-scripts/rule-eth1*

    ```bash
    from 10.0.1.5/32 table eth1-rt
    to 10.0.1.5/32 table eth1-rt
    ```

- */etc/sysconfig/network-scripts/route-eth1*

    ```bash
    10.0.1.0/24 dev eth1 table eth1-rt
    default via 10.0.1.1 dev eth1 table eth1-rt
    ```

Para aplicar as alterações, reinicie o *rede* service da seguinte forma:

```bash
systemctl restart network
```

As regras de encaminhamento estão corretamente em vigor e se pode ligar a cada interface conforme necessário.


## <a name="next-steps"></a>Passos Seguintes
Revisão [tamanhos de VM do Linux](sizes.md) ao tentar criar uma VM com várias NICs. Preste atenção para o número máximo de NICs oferece suporte a cada tamanho de VM. 
