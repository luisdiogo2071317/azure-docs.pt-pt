---
title: Criar uma VM com um endereço IP público estático - CLI do Azure | Documentos da Microsoft
description: Saiba como criar uma VM com um endereço IP público estático usando a interface de linha de comandos (CLI) do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd44971162a79e53b731c5c89316f14e8bb0a1a6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651964"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli"></a>Criar uma VM com um endereço IP público estático, com a CLI do Azure

> [!div class="op_single_selector"]
> * [Portal do Azure](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [CLI do Azure](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Clássico)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Este artigo explica como utilizar o modelo de implementação do Resource Manager, que é recomendado pela Microsoft para as implementações mais novas em vez do modelo de implementação clássica.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>Criar a VM

Os valores na "" para as variáveis nos passos que se seguem a criar recursos com as definições do cenário. Altere os valores, conforme apropriado, para o seu ambiente.

1. Instalar o [CLI 2.0 do Azure](/cli/azure/install-az-cli2) se ainda não tiver instalado.
2. Criar um par de chaves público e privado SSH para VMs do Linux, concluindo os passos a [criar um par de chaves público e privado SSH para VMs do Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. A partir de um shell de comando, o início de sessão com o comando `az login`.
4. Crie a VM ao executar o script que se segue num computador Linux ou Mac. O endereço IP público do Azure, a rede virtual, a interface de rede e recursos da VM têm de existir na mesma localização. Embora os recursos não têm de existir no mesmo grupo de recursos, o seguinte script fazem.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

Além de criar uma VM, o script cria:
- Um único premium disco gerido por predefinição, mas tiver outras opções para o tipo de disco, que pode criar. Leitura a [criar uma VM do Linux com a CLI 2.0 do Azure](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artigo para obter detalhes.
- Rede virtual, sub-rede, NIC e recursos de endereço IP público. Em alternativa, pode utilizar *existente* rede virtual, sub-rede, NIC ou recursos de endereço IP público. Para saber como usar recursos de rede existentes em vez de criar recursos adicionais, introduza `az vm create -h`.

## <a name = "validate"></a>Validar a criação da VM e o endereço IP público

1. Introduza o comando `az resource list --resouce-group IaaSStory --output table` para ver uma lista dos recursos criados pelo script. Deve haver cinco recursos no resultado retornado: interface, o disco, o endereço IP público, a rede virtual e uma máquina virtual de rede.
2. Introduza o comando `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. No resultado retornado, tenha em atenção o valor de **IpAddress** e que o valor de **PublicIpAllocationMethod** é *estático*.
3. Antes de executar o comando seguinte, remova o <>, substitua *nome de utilizador* com o nome que utilizou para o **nome de utilizador** variáveis no script e substitua *ipAddress*com o **ipAddress** do passo anterior. Execute o seguinte comando para ligar à VM: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Remover a VM e os recursos associados

Recomenda-se que elimina os recursos criados neste exercício, se não usá-los em produção. A VM, endereço IP público e recursos de disco incorrer em custos, enquanto estiverem aprovisionados. Para remover os recursos criados durante este exercício, conclua os seguintes passos:

1. Para ver os recursos no grupo de recursos, execute o `az resource list --resource-group IaaSStory` comando.
2. Certifique-se não há nenhum recurso no grupo de recursos, que não sejam os recursos criados pelo script neste artigo. 
3. Para eliminar todos os recursos criados neste exercício, execute o `az group delete -n IaaSStory` comando. O comando elimina o grupo de recursos e todos os recursos que nele contidos.
 
## <a name="set-ip-addresses-within-the-operating-system"></a>Conjunto de endereços IP no sistema operativo

Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure no sistema de operativo da máquina virtual. É recomendável que não atribuir estaticamente IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário, como quando [atribuição de IP de vários endereços para uma VM do Windows](virtual-network-multiple-ip-addresses-cli.md). Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído para o Azure [interface de rede](virtual-network-network-interface-addresses.md#change-ip-address-settings), ou pode perder a conectividade para a máquina virtual. Saiba mais sobre [endereço IP privado](virtual-network-network-interface-addresses.md#private) definições.

## <a name="next-steps"></a>Passos Seguintes

Qualquer tráfego de rede pode fluir de e para a VM que criou neste artigo. Pode definir regras de segurança de entrada e saída dentro de um grupo de segurança de rede que limitam o tráfego que pode fluir de e para a interface de rede, a sub-rede ou ambos. Para saber mais sobre os grupos de segurança de rede, veja [descrição geral de grupo de segurança de rede](security-overview.md).