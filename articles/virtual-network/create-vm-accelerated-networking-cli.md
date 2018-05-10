---
title: Criar uma máquina virtual do Azure com acelerados rede | Microsoft Docs
description: Saiba como criar uma máquina virtual Linux com acelerados da rede.
services: virtual-network
documentationcenter: na
author: gsilva5
manager: gedegrac
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: gsilva
ms.custom: ''
ms.openlocfilehash: 0f7f389df96f38bea3634bf712af3f9bf4bdde09
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Criar uma máquina virtual Linux com acelerados da rede

Neste tutorial, irá aprender a criar uma máquina virtual (VM) do Linux com acelerados da rede. Para criar uma VM do Windows com acelerados da rede, consulte [criar uma VM do Windows com a rede acelerados](create-vm-accelerated-networking-powershell.md). Na melhoria de rede permite que a virtualização de e/s de raiz única (SR-IOV) para uma VM, melhorando em grande medida o desempenho de rede. Este caminho de elevado desempenho ignora o anfitrião datapath, reduzindo a latência, interferência e utilização da CPU, para utilização com cargas de trabalho de rede mais demanding em tipos VM suportados. A imagem seguinte mostra a comunicação entre duas VMs com e sem redes na melhoria:

![Comparação](./media/create-vm-accelerated-networking/accelerated-networking.png)

Sem redes na melhoria, todo o tráfego de rede que entra e sai da VM tem atravessar o anfitrião e o comutador virtual. O comutador virtual fornece todos os imposição de política, tais como grupos de segurança de rede, listas de controlo de acesso, isolamento e outros serviços de rede virtualizado para tráfego de rede. Para obter mais informações sobre comutadores virtuais, leia o [Virtualização de rede do Hyper-V e do comutador virtual](https://technet.microsoft.com/library/jj945275.aspx) artigo.

Com redes na melhoria, o tráfego de rede chega à interface de rede da VM (NIC) e, em seguida, é reencaminhado para a VM. Todas as políticas de rede que o comutador virtual aplica-se agora são descarregadas sendo aplicadas no hardware. Aplicar a política de hardware permite que o NIC para reencaminhar o tráfego de rede diretamente para a VM, ignorando o anfitrião e o comutador virtual, enquanto mantém todos os a política que aplicada no anfitrião.

As vantagens do funcionamento em rede na melhoria só se aplicam à VM que está ativada no. Para obter os melhores resultados, é ideal ativar esta funcionalidade em, pelo menos, duas VMs ligadas à mesma rede de Virtual do Azure (VNet). Quando a comunicação entre VNets ou ligação no local, esta funcionalidade tem um impacto mínimo para geral de latência.

## <a name="benefits"></a>Benefícios
* **Reduzir a latência / superiores pacotes por segundo (pps):** removendo o comutador virtual do datapath remove o tempo de pacotes gastam no anfitrião para o processamento da política e aumenta o número de pacotes que podem ser processados dentro da VM.
* **Reduzido interferência:** comutador Virtual de processamento depende da quantidade de política que tem de ser aplicadas e a carga de trabalho da CPU que está a fazer o processamento. Descarregar a imposição de política para o hardware remove esse variabilidade fornecendo pacotes diretamente para a VM, a remoção do anfitrião para comunicação de VM e todas as interrupções de software e comutadores de contexto.
* **Diminuir a utilização da CPU:** ignorar o comutador virtual no anfitrião leva a menor utilização da CPU para processar o tráfego de rede.

## <a name="supported-operating-systems"></a>Sistemas operativos suportados
São suportadas as seguintes distribuições Box na galeria do Azure: 
* **Ubuntu 16.04** 
* **SLES 12 SP3** 
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS Linux**
* **Debian "Stretch Database" com o backports kernel**
* **Oracle Linux 7.4**

## <a name="limitations-and-constraints"></a>Limitações e restrições

### <a name="supported-vm-instances"></a>Instâncias VM suportadas
É suportada na melhoria de redes objetivo mais comum e tamanhos de instância com otimização de computação com vCPUs 2 ou mais.  Este série suportado é: D/série DSv2 e F/Fs

As instâncias que suportem Hyper-Threading, acelerados redes é suportada em instâncias VM com vCPUs 4 ou mais. Série suportado é: D/DSv3, I/ESv3, Fsv2 e Ms/Mms.

Para obter mais informações sobre as instâncias VM, consulte [tamanhos de VM com Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regiões
Disponível em todas as regiões do Azure públicas, bem como Azure Government nuvens.

### <a name="network-interface-creation"></a>Criação de interface de rede 
Na melhoria de redes só podem ser ativada para uma NIC de novo. Não pode ser ativada para uma NIC que existente.
### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Ativar acelerados rede numa VM em execução
Um tamanho VM suportado sem redes na melhoria ativada só pode ter a funcionalidade ativada quando está parado e desalocada.  
### <a name="deployment-through-azure-resource-manager"></a>Implementação através do Azure Resource Manager
Não é possível implementar máquinas virtuais (clássicas) com acelerados da rede.

## <a name="create-a-linux-vm-with-azure-accelerated-networking"></a>Criar uma VM com Linux com redes na melhoria do Azure

Embora este artigo fornece os passos para criar uma máquina virtual com redes na melhoria utilizando a CLI do Azure, também pode [criar uma máquina virtual com redes na melhoria no portal do Azure](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Quando criar uma máquina virtual no portal, em **definições**, selecione **ativado**, em **acelerados redes**. A opção para ativar na melhoria de redes não aparece no portal, salvo se tiver selecionado um [sistema operativo suportado](#supported-operating-systems) e [tamanho da VM](#supported-vm-instances). Depois de criar a máquina virtual, tem de concluir as instruções em [confirmar que está ativada na melhoria de redes](#confirm-that-accelerated-networking-is-enabled).

### <a name="create-a-virtual-network"></a>Criar uma rede virtual

Instalar a versão mais recente [Azure CLI 2.0](/cli/azure/install-az-cli2) e início de sessão para um Azure conta através de [início de sessão az](/cli/azure/reference-index#az_login). Nos exemplos a seguir, substitua os nomes dos parâmetros de exemplo com os seus próprios valores. Os nomes de parâmetros de exemplo incluídos *myResourceGroup*, *myNic*, e *myVm*.

Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup* no *centralus* localização:

```azurecli
az group create --name myResourceGroup --location centralus
```

Selecione uma região de Linux suportada listada na [Linux acelerados redes](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual denominada *myVnet* com uma sub-rede:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

### <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede
Criar um grupo de segurança de rede com [az rede nsg criar](/cli/azure/network/nsg#az_network_nsg_create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

O grupo de segurança de rede contém várias regras de predefinidas, um deles desativa todos os acessos de entrada da Internet. Abrir uma porta para permitir o acesso SSH para a máquina virtual com [criar regras de nsg de rede az](/cli/azure/network/nsg/rule#az_network_nsg_rule_create):

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

### <a name="create-a-network-interface-with-accelerated-networking"></a>Criar uma interface de rede com redes na melhoria

Criar um endereço IP público com [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create). Um endereço IP público não é necessário se não tiver intenção para aceder a máquina virtual a partir da Internet, mas para concluir os passos neste artigo, é necessário.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Criar uma interface de rede com [nic da rede az criar](/cli/azure/network/nic#az_network_nic_create) com redes na melhoria ativada. O exemplo seguinte cria uma interface de rede com o nome *myNic* no *mySubnet* sub-rede do *myVnet* rede virtual e associa o  *myNetworkSecurityGroup* grupo de segurança de rede para a interface de rede:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

### <a name="create-a-vm-and-attach-the-nic"></a>Criar uma VM e anexe o NIC
Quando criar a VM, especifique o NIC que criou com `--nics`. Selecione um tamanho e a distribuição listados na [Linux acelerados redes](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM chamada *myVM* com a imagem de UbuntuLTS e um tamanho que suporte redes acelerados (*Standard_DS4_v2*):

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Para obter uma lista de todos os tamanhos de VM e características, consulte [tamanhos de VM com Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Assim que a VM é criada, é devolvido o resultado semelhante ao seguinte exemplo de saída. Anote o **publicIpAddress**. Este endereço é utilizado para aceder a VM em passos subsequentes.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

### <a name="confirm-that-accelerated-networking-is-enabled"></a>Certifique-se na melhoria de rede está ativada

Utilize o seguinte comando para criar uma sessão SSH com a VM. Substitua `<your-public-ip-address>` com o endereço IP público atribuído a virtual máquina que criou e substitua *azureuser* se tiver utilizado um valor diferente para `--admin-username` quando criou a VM.

```bash
ssh azureuser@<your-public-ip-address>
```

A partir da Bash shell, introduza `uname -r` e confirme que a versão de kernel é uma das seguintes versões, ou superior:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Confirme que o dispositivo Mellanox VF está exposto a VM com o `lspci` comando. O resultado devolvido é semelhante ao seguinte resultado:

```bash
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

Verifique a existência de atividade no VF (função virtual) com o `ethtool -S eth0 | grep vf_` comando. Se receber resultado semelhante ao seguinte exemplo de saída, na melhoria de rede está ativada e a funcionar.

```bash
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
Na melhoria de redes está agora ativada para a VM.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Ativar acelerados redes em VMs existentes
Se tiver criado uma VM sem acelerados redes, é possível ativar esta funcionalidade numa VM existente.  A VM tem de suportar redes acelerados pelo cumprir os seguintes pré-requisitos que também estão descritos acima:

* A VM tem de ser um tamanho suportado para acelerados redes
* A VM tem de ser uma imagem de galeria do Azure suportada (e a versão de kernel para Linux)
* Todas as VMs num conjunto de disponibilidade ou VMSS tem de ser parado/desalocada antes de ativar acelerados redes em qualquer NIC

### <a name="individual-vms--vms-in-an-availability-set"></a>Individuais VMs & VMs na disponibilidade de um conjunto
Primeiro parar/Desalocação da VM ou, se um conjunto de disponibilidade, todas as VMs no conjunto de:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

Importante,. tenha em atenção, se a VM foi criada individualmente, sem um conjunto de disponibilidade, que apenas tem de parar/anular atribuição de VM individuais para ativar acelerados redes.  Se a VM foi criada com um conjunto de disponibilidade, todas as VMs incluídas no conjunto de disponibilidade tem de ser parado/desalocada antes de ativar acelerados redes em qualquer um dos NICs. 

Depois de parar, ative acelerados redes no NIC da sua VM:

```azurecli
az network nic update \
    --name myVM -n myNic \
    --resource-group myResourceGroup \
    --accelerated-networking true
```

Reiniciar a VM ou, se num conjunto de disponibilidade, todas as VMs no conjunto e confirme que a rede acelerados está ativado: 

```azurecli
az vm start --resource-group myResourceGroup \
    --name myVM
```

### <a name="vmss"></a>VMSS
VMSS é ligeiramente diferente, mas segue mesmo fluxo de trabalho.  Em primeiro lugar, pare as VMs:

```azurecli
az vmss deallocate \
    --name myvmss \
    --resource-group myrg
```

Assim que as VMs estão paradas, atualize a propriedade acelerados rede sob a interface de rede:

```azurecli
az vmss update --name myvmss \
    --resource-group myrg \
    --set virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].enableAcceleratedNetworking=true
```

. Tenha em atenção, uma VMSS tem as atualizações VM que se aplicam a atualizações com três definições diferentes, graduais manuais e o automáticas.  Nestas instruções, a política estiver definida como automático para que o VMSS selecionará as alterações imediatamente após o reinício.  Para o definir como automático para que as alterações são imediatamente captadas: 

```azurecli
az vmss update \
    --name myvmss \
    --resource-group myrg \
    --set upgradePolicy.mode="automatic"
```

Finalmente, reinicie o VMSS:

```azurecli
az vmss start \
    --name myvmss \
    --resource-group myrg
```

Uma vez, reinicia, aguarde que as atualizações concluir, mas uma vez concluído, será apresentado o VF dentro da VM.  (Certifique-se que estiver a utilizar um tamanho de SO e VM suportado.)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Redimensionar VMs existentes com acelerados da rede

As VMs com acelerados rede ativada só podem ser redimensionadas para VMs que suportam acelerados redes.  

Não pode ser redimensionada uma VM com acelerados rede ativada para uma instância VM não suporta acelerados redes utilizando a operação de redimensionamento.  Em vez disso, para redimensionar um estas VMs: 

* Parar/Deallocate a VM ou em caso de um conjunto de disponibilidade/VMSS, pare/anular atribuição de todas as VMs no conjunto/VMSS.
* Tem de ser desativada na melhoria de redes no NIC da VM ou se numa disponibilidade conjunto/VMSS, todas as VMs no conjunto/VMSS.
* Depois de acelerados rede estiver desativado, o conjunto de disponibilidade/VM/VMSS podem ser movidos para um novo tamanho que não suporta acelerados redes e reiniciado.  

