---
title: "Criar uma máquina virtual do Azure com acelerados rede | Microsoft Docs"
description: "Saiba como criar uma máquina virtual Linux com acelerados da rede."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 908d81c363a556917d211e0bcc92188f849fb690
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking"></a>Criar uma máquina virtual Linux com acelerados da rede

> [!IMPORTANT] 
> Máquinas virtuais têm de ser criadas com acelerados rede ativada. Esta funcionalidade não é possível ativar as máquinas virtuais existentes. Pode seguir os passos abaixo para ativar acelerados redes:
>   1. Elimine a máquina virtual.
>   2. Voltar a criar a máquina virtual com acelerados rede ativada.
>

Neste tutorial, irá aprender a criar uma máquina virtual (VM) do Linux com acelerados da rede. Na melhoria de rede permite que a virtualização de e/s de raiz única (SR-IOV) para uma VM, melhorando em grande medida o desempenho de rede. Este caminho de elevado desempenho ignora o anfitrião datapath, reduzindo a latência, interferência e utilização da CPU, para utilização com cargas de trabalho de rede mais demanding em tipos VM suportados. A imagem seguinte mostra a comunicação entre duas VMs com e sem redes na melhoria:

![Comparação](./media/create-vm-accelerated-networking/accelerated-networking.png)

Sem redes na melhoria, todo o tráfego de rede que entra e sai da VM tem atravessar o anfitrião e o comutador virtual. O comutador virtual fornece todos os imposição de política, tais como grupos de segurança de rede, listas de controlo de acesso, isolamento e outros serviços de rede virtualizado para tráfego de rede. Para obter mais informações sobre comutadores virtuais, leia o [Virtualização de rede do Hyper-V e do comutador virtual](https://technet.microsoft.com/library/jj945275.aspx) artigo.

Com redes na melhoria, o tráfego de rede chega à interface de rede da VM (NIC) e, em seguida, é reencaminhado para a VM. Todas as políticas de rede que o comutador virtual aplica-se agora são descarregadas sendo aplicadas no hardware. Aplicar a política de hardware permite que o NIC para reencaminhar o tráfego de rede diretamente para a VM, ignorando o anfitrião e o comutador virtual, enquanto mantém todos os a política que aplicada no anfitrião.

As vantagens do funcionamento em rede na melhoria só se aplicam à VM que está ativada no. Para obter os melhores resultados, é ideal ativar esta funcionalidade em, pelo menos, duas VMs ligadas à mesma rede de Virtual do Azure (VNet). Quando a comunicação entre VNets ou ligação no local, esta funcionalidade tem um impacto mínimo para geral de latência.

## <a name="benefits"></a>Benefícios
* **Reduzir a latência / superiores pacotes por segundo (pps):** removendo o comutador virtual do datapath remove o tempo de pacotes gastam no anfitrião para o processamento da política e aumenta o número de pacotes que podem ser processados dentro da VM.
* **Reduzido interferência:** comutador Virtual de processamento depende da quantidade de política que tem de ser aplicadas e a carga de trabalho da CPU que está a fazer o processamento. Descarregar a imposição de política para o hardware remove esse variabilidade fornecendo pacotes diretamente para a VM, a remoção do anfitrião para comunicação de VM e todas as interrupções de software e comutadores de contexto.
* **Diminuir a utilização da CPU:** ignorar o comutador virtual no anfitrião leva a menor utilização da CPU para processar o tráfego de rede.

## <a name="supported-operating-systems"></a>Sistemas operativos suportados
* **Ubuntu 16.04**: 4.11.0-1013 ou a versão mais recente de kernel
* **SLES SP3**: 4.4.92-6.18 ou a versão mais recente de kernel
* **RHEL**: 7.4.2017120423 ou a versão mais recente de kernel
* **CentOS**: 7.4.20171206 ou a versão mais recente de kernel

## <a name="supported-vm-instances"></a>Instâncias VM suportadas
É suportada na melhoria de redes objetivo mais comum e tamanhos de instância com otimização de computação com vCPUs 4 ou mais. Instâncias como D/DSv3 ou I/ESv3 que suportam o Hyper-Threading, acelerados redes é suportada em instâncias VM com 8 ou mais vCPUs.  Série suportado é: D/série DSv2, D/DSv3, I/ESv3, Fs/F/Fsv2 e Ms/Mms. 

Para obter mais informações sobre as instâncias VM, consulte [tamanhos de VM com Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regiões
Disponível em todas as regiões do Azure públicas com a exceção Ásia Oriental.   Nuvem do Azure Government ainda não é suportada.

## <a name="limitations"></a>Limitações
Existem as seguintes limitações ao utilizar esta capacidade:

* **Criação de interface de rede:** Accelerated redes só podem ser ativada para uma NIC de novo. Não pode ser ativada para uma NIC que existente.
* **A criação de VM:** A NIC com redes na melhoria ativada só podem ser anexado a uma VM quando é criada a VM. O NIC não pode ser ligado a uma VM existente. Se adicionar a VM para um disponibilidade existente, todas as VMs no conjunto de disponibilidade tem também de ter acelerados rede ativada.
* **Apenas a implementação através do Gestor de recursos do Azure:** não é possível implementar máquinas virtuais (clássicas) com acelerados da rede.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Instalar a versão mais recente [Azure CLI 2.0](/cli/azure/install-az-cli2) e início de sessão para um Azure conta através de [início de sessão az](/cli/azure/#login). Nos exemplos a seguir, substitua os nomes dos parâmetros de exemplo com os seus próprios valores. Os nomes de parâmetros de exemplo incluídos *myResourceGroup*, *myNic*, e *myVm*.

Crie um grupo de recursos com [az group create](/cli/azure/group#create). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup* no *centralus* localização:

```azurecli
az group create --name myResourceGroup --location centralus
```

Tem de selecionar uma região de Linux suportada listada na [Linux acelerados redes](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview).

Criar uma rede virtual com [az rede vnet criar](/cli/azure/network/vnet#create). O exemplo seguinte cria uma rede virtual denominada *myVnet* com uma sub-rede:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede
Criar um grupo de segurança de rede com [az rede nsg criar](/cli/azure/network/nsg#create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

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

## <a name="create-a-network-interface-with-accelerated-networking"></a>Criar uma interface de rede com redes na melhoria

Criar um endereço IP público com [az público-ip da rede criar](/cli/azure/network/public-ip#az_network_public_ip_create). Um endereço IP público não é necessário se não tiver intenção para aceder a máquina virtual a partir da Internet, mas para concluir os passos neste artigo, é necessário.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Criar uma interface de rede com [nic da rede az criar](/cli/azure/network/nic#create) com redes na melhoria ativada. O exemplo seguinte cria uma interface de rede com o nome *myNic* no *mySubnet* sub-rede do *myVnet* rede virtual e associa o  *myNetworkSecurityGroup* grupo de segurança de rede para a interface de rede:

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

## <a name="create-a-vm-and-attach-the-nic"></a>Criar uma VM e anexe o NIC
Quando criar a VM, especifique o NIC que criou com `--nics`. Tem de selecionar um tamanho e a distribuição listados em [Linux acelerados redes](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview). 

Crie uma VM com [az vm create](/cli/azure/vm#create). O exemplo seguinte cria uma VM chamada *myVM* com a imagem de UbuntuLTS e um tamanho que suporte redes acelerados (*Standard_DS4_v2*):

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

Assim que a VM é criada, é devolvido o resultado semelhante ao seguinte exemplo de saída. Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a VM em passos subsequentes.

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

## <a name="confirm-that-accelerated-networking-is-enabled"></a>Certifique-se na melhoria de rede está ativada

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
